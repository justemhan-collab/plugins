# STEP/CAD BOM Extraction and Cross-Validation (CAD BOM vs ERP BOM vs Drawing Parts List): Open-Source Approaches

Research date: 2026-10-20. Repo metadata (stars, `updated_at`) came from the GitHub search API on 2026-10-20. GitHub's `updated_at` is when the repo record last changed. It can move without a code commit, so treat it as approximate activity, **not a verified last-commit date**. PyPI versions and dates came from the PyPI JSON API on the same day. The opencascade.com, dev.opencascade.org and openbom.com sites did not resolve for full-page fetches from this environment. Claims from those domains come from search-result snippets and are marked as such.

Project context: a Korean semiconductor-equipment design team plans a Python tool that runs SOLIDWORKS API → JSON → rule checks → Excel review sheet. Its BOM inputs are a SOLIDWORKS BOM exported to Excel and an ERP BOM in Excel.

## Q1. How do projects extract assembly structure and quantities from STEP? What are the pitfalls (instance naming, flattened exports, configurations)?

### Takeaway
Every serious open Python route uses OpenCascade's XDE/XCAF document model, through pythonocc-core, OCP/CadQuery, build123d or FreeCAD. They read the STEP file with `STEPCAFControl_Reader` into a `TDocStd_Document`, walk `XCAFDoc_ShapeTool` free shapes → `GetComponents` → `IsReference`/`GetReferredShape`, and count quantities as the number of reference (instance) labels that point at the same prototype label. The main pitfalls are these:
- Names sit on the instance (NAUO) in some places and on the product in others.
- Exporters add suffixes to duplicated instances.
- Some exporters flatten or split the assembly ("atomic" or per-part files).
- Configurations are lost or turned into separate products.

The quantity must be rolled up by the receiving tool. STEP does not carry a quantity field.

### Cited Findings
**Core mechanism (OCCT XDE/XCAF)**
- XDE defines an **Instance** as "a replication of another shape with a location". Assembly structure, names, colors, layers, validation properties and GD&T are all handled through XDE tools. Source: the OCCT XDE user guide in the repo (`dox/user_guides/xde/xde.md`, master branch) — [OCCT xde.md](https://raw.githubusercontent.com/Open-Cascade-SAS/OCCT/master/dox/user_guides/xde/xde.md)
- API to tell instances from prototypes: `ShapeTool->IsReference(label)` says whether a label is a located reference. `GetLocation(label)` returns its placement, and `GetReferredShape` returns the referenced original (the "Instances and References for Components" section) — [OCCT xde.md](https://raw.githubusercontent.com/Open-Cascade-SAS/OCCT/master/dox/user_guides/xde/xde.md)
- The XDE docs (snippet) say one label can be reached through several paths, so a single occurrence is identified by its full path. This matters when the same sub-assembly is reused at several levels — [OCCT commit/xde doc (git.rbts.co mirror)](https://git.rbts.co/OpenCascade/occt/commit/d6dfeac244d42bcd78435d10f47064c4c65562d1)
- From an OCCT forum thread (snippet): the names read from components through `GetComponents` belong to the **usage-occurrence (instance)** entities, not the part definitions. To get the product/part name you must follow `GetReferredShape` — [OCCT forum: Finding the name of an assembly part](https://dev.opencascade.org/content/finding-name-assembly-part-step-file-using-xde); [OCCT forum: Extract hierarchy/tree of products](https://dev.opencascade.org/content/extract-hierarchy-tree-products)
- An OCCT sample Tcl script builds an assembly with two instances of one nut, each with its own instance name. It is the reference pattern for counting repeated references — [xde.tcl sample](https://dev.opencascade.org/system/files/forum/xde.tcl)
- XDE also reads and writes **validation properties** (volume, centroid, area) from STEP and provides a tool to check them. These can be cross-checked against SOLIDWORKS mass properties to detect a wrong or changed part — [OCCT xde.md](https://raw.githubusercontent.com/Open-Cascade-SAS/OCCT/master/dox/user_guides/xde/xde.md)

**Project catalog: STEP structure extraction**

| Project | URL | Approach | Stack | Maturity (2026-10-20) | License | Limitations |
|---|---|---|---|---|---|---|
| OCCT (Open CASCADE Technology) | https://github.com/Open-Cascade-SAS/OCCT | Reference C++ implementation of STEP AP203/214/242 read/write via XDE (`STEPCAFControl`, `XCAFDoc_ShapeTool`, `XCAFDoc_DimTolTool`) | C++ | ~3.0k stars; repo updated 2026-10-19 | LGPL-2.1 with exception (per project; license not re-verified via API) | Low-level API; documentation for PMI/metadata is sparse; site dev.opencascade.org is main forum |
| pythonocc-core | https://github.com/tpaviot/pythonocc-core | SWIG Python wrapper of all of OCCT; you write the XCAF traversal yourself | Python/SWIG, conda-distributed | ~2.0k stars; updated 2026-10-16 | LGPL-3.0 (per project, not API-verified) | Distributed via conda (the PyPI entry is stale at 0.16); no ready BOM function. The pythonocc docs page for `STEPCAFControl` is only an API index with no tree-walking example ([pythonocc docs](https://pythonocc-documentation.readthedocs.io/en/review-gen-apidoc-rtd/apidoc/OCC.Core.STEPCAFControl.html)) |
| OCP | https://github.com/CadQuery/OCP | pybind11 bindings of OCCT used by CadQuery/build123d; pip-installable as `cadquery-ocp` (8.0.1.1.0 uploaded 2026-10-05 — version numbering suggests OCCT 8.0 base, **unverified**) | Python/pybind11 | ~205 stars; updated 2026-10-09 | Apache-2.0 (PyPI) | Raw OCCT API; static methods have `_s` suffix (e.g. `GetComponents_s`) |
| CadQuery | https://github.com/CadQuery/cadquery | `cq.Assembly` with `importStep` (`cadquery/occ_impl/importers/assembly.py`), which walks XCAF labels: `GetComponents_s` → `IsReference_s` → `GetReferredShape_s`, reads names via `TDataStd_Name`, materials via `XCAFDoc_Material` (name/description/density) and colors. The Assembly object also carries a `metadata` dict | Python + OCP | ~6.0k stars; PyPI 2.8.0 (2026-06-21) | Apache-2.0 | It builds a CadQuery assembly tree, not a BOM. You still roll up quantities yourself. It does not import PMI |
| build123d | https://github.com/gumyr/build123d | CAD-as-code on OCP; `import_step` returns a labelled shape tree | Python + OCP | ~3.5k stars; PyPI 0.13.0 (2026-09-21) | Apache-2.0 | Same as CadQuery; assembly-import fidelity not verified in this research |
| FreeCAD (core Assembly workbench, 1.0+) | https://github.com/FreeCAD/FreeCAD | STEP import via OCCT; the **Assembly CreateBom** tool builds a BOM from a selected assembly or the document, with configurable columns (weight, custom part number); missing properties show "N/A" ([FreeCAD wiki, CreateBom](https://wiki.freecad.org/Assembly_CreateBom/cs)) | Python/C++ | (stars not retrieved) | LGPL-2.1+ | The wiki page is marked unfinished. It is not confirmed that a STEP-imported assembly keeps instance structure for the BOM |
| Assembly4 BOM macro / FreeCAD-Assembly4-BOM | https://github.com/Ediforce44/FreeCAD-Assembly4-BOM ; macro on FreeCAD forum ([makeBomCmd.py](https://devtalk.freecad.org/uploads/short-url/A4WMB9eAwFpUbm6mDIfkLatyiZ.py)) | Counts how often each linked part appears to get quantities, then writes user part-info properties into a FreeCAD spreadsheet | Python (FreeCAD) | 5 stars; updated 2024-12 | (not verified) | Tied to the Assembly4 workbench |
| Assembly3 ImportFromSTEP | https://wiki.freecad.org/Assembly3_ImportFromSTEP/en | Imports a STEP assembly into a single FreeCAD document as an Assembly3 tree | FreeCAD addon | — | — | Legacy workbench |
| mozman/steputils | https://github.com/mozman/steputils | Pure-Python Part 21 (ISO 10303-21) parser/DOM; could count `NEXT_ASSEMBLY_USAGE_OCCURRENCE` entities without OCCT | Python, antlr4 runtime (<4.10) | 53 stars; PyPI 0.1 (2022-03-29); README says "**THIS PROJECT IS IN PLANNING STATE!**" ([README](https://raw.githubusercontent.com/mozman/steputils/master/README.md)) | MIT | Effectively dormant; no assembly or PMI semantics |
| hsu-aut/STEP-to-JSON | https://github.com/hsu-aut/STEP-to-JSON | Parses STEP text to extract the product/assembly structure as JSON | JavaScript (Node) | 11 stars; updated 2025-11 | (not verified) | Small academic tool; text-level parsing |
| JaderoChan/StepToJson | https://github.com/JaderoChan/StepToJson | Converts STEP to JSON in Python | Python | 3 stars | (not verified) | Hobby-level |
| SharpGuanaco/FusionSTEPExtractor | https://github.com/SharpGuanaco/FusionSTEPExtractor | Script that extracts a BOM from a Fusion 360 STEP file | — | 0 stars; 2022 | — | Single-vendor, unmaintained |
| kovacsv/occt-import-js | https://github.com/kovacsv/occt-import-js | Emscripten/WASM OCCT STEP/IGES importer that outputs a JSON mesh plus a node hierarchy | C++/WASM/JS | ~289 stars | (not verified; LGPL-2.1 likely, from OCCT) | Geometry/hierarchy oriented, no PMI |
| usnistgov/SFA (STEP File Analyzer and Viewer) | https://github.com/usnistgov/SFA | Converts any Part 21 file to a spreadsheet of entities and attributes. Includes NAUO/product entities, PMI and validation-property checks | Tcl | 198 stars; updated 2026-10-03 | Data.gov lists "other-license-specified" (U.S. Government work); check the repo ([data.gov](https://catalog.data.gov/dataset/step-file-analyzer-and-viewer-software)) | Windows GUI/CLI. Analysis tool, not a library |
| schwitters/openswx | https://github.com/schwitters/openswx | C++20 toolkit that reads .SLDPRT/.SLDASM/.SLDDRW **without SOLIDWORKS**. Includes a recursive BOM builder and a web inspection server with "profile-driven BOM transformations" | C++20 | 31 stars; created 2026-04, updated 2026-10-18 | (not verified) | Reverse-engineered file format, very new. Reliability on production files unknown |
| matt454357/sw-BOM-Scan | https://github.com/matt454357/sw-BOM-Scan | Extracts BOMs from SOLIDWORKS assembly files into SQLite (via the SOLIDWORKS API) | C# | 2 stars; 2023 | (not verified) | Old, small |
| easyhard01/EdmBOM_CSharp | https://github.com/easyhard01/EdmBOM_CSharp | "CMP equipment BOM management comparison tool (SolidWorks EPDM + Excel)", a semiconductor-equipment domain example | C# | 1 star; created 2026-09 | (not verified) | Very new; content not inspected |

**Pitfalls**
- **Duplicate instance naming:** a PTC community thread (snippet) reports that repeated identical parts get suffixed instance names in STEP, and the suffix pattern varies across systems. The source system rolls identical parts into one counted line, while STEP holds one master product with several instances, so **quantity must be rebuilt on import** — [PTC community](https://community.ptc.com/modeling-323/index10.html)
- **Atomic/split export:** the SOLIDWORKS STEP export options include exporting assemblies as **atomic STEP files** (recommended for large assemblies). Each part then lands in its own file, and the BOM must be rebuilt from the top-level references — [SOLIDWORKS 2026 STEP Export Options](https://help.solidworks.com/2026/English/SolidWorks/Sldworks/HIDD_EXPORT_OPTIONS_STEP.htm)
- **Names lost in some exporters:** for example, a Shapr3D forum thread on object names not being exported in STEP — [Shapr3D discourse](https://discourse.shapr3d.com/t/object-names-not-exported-in-step/15036)
- **Configurations / part numbers:** older SOLIDWORKS help (2010–2012) documents a "STEP Configuration Data for Export" dialog where product information can be entered per component — [SOLIDWORKS 2012 help](https://help.solidworks.com/2012/English/SolidWorks/Sldworks/Exporting_Configuration_Data.htm). An Autodesk Inventor thread shows the common request to use the **part number instead of the file name** as the STEP product name — [Autodesk forum](https://forums.autodesk.com/t5/inventor-programming-ilogic/step-export-using-partnumber-for-name-instead-of-filename/m-p/10111183)
- **Sum vs separate rows:** BOM exporters such as DriveWorks have a "Sum Instance Quantities" option that controls whether same-configuration instances are summed or listed on separate rows. That choice changes how the BOM Excel looks for the same model — [DriveWorks docs](https://docs.driveworkspro.com/Topic/GTExportBOM)

### Inferences
- For this team the **authoritative structure source should be the SOLIDWORKS API**, not STEP. Read `IComponent2` traversal, `ReferencedConfiguration`, custom properties, suppressed/excluded-from-BOM flags and virtual components, then serialize to JSON. STEP loses configuration identity and the "exclude from BOM" flags, and mangles instance names (`part<1>`, `part<2>` style suffixes). STEP is best used as an **independent second extraction**: count NAUO references per product via OCP/XCAF to cross-check the SOLIDWORKS-API JSON quantities. Treat this as a "golden" check that SOLIDWORKS-side BOM-table settings did not hide parts.
- Quantity rule: qty(child under parent) = number of reference labels under the parent's prototype label whose `GetReferredShape` equals the child prototype. Total (flattened) qty = product of quantities along every path, summed over all paths, because of the multi-path note in the XDE docs.
- Key for matching STEP to the SOLIDWORKS/ERP rows: the **product name taken from the referred (prototype) label**, not the instance label. Strip trailing instance suffixes (`<n>`, `:n`, `_n`) and the configuration suffix before matching.
- Reusable components: CadQuery's `importers/assembly.py` (Apache-2.0) is a compact, permissively licensed reference implementation of XCAF traversal plus name/material/color extraction in Python. It can be copied or adapted. For a dependency-free sanity check, a simple regex count of `NEXT_ASSEMBLY_USAGE_OCCURRENCE` lines in the Part 21 text is often enough.

### Gaps
- I could not verify exactly how current SOLIDWORKS (2025/2026) fills the STEP `PRODUCT.id`/`PRODUCT.name` fields: file name vs configuration name vs a "Part Number" custom property. A test export and file inspection is recommended.
- I did not verify how SOLIDWORKS STEP export handles virtual components, envelopes, "exclude from BOM" or toolbox parts.
- No license or last-commit verification was possible through the GitHub API (blocked in this session), so the licenses in the table are from PyPI or prior knowledge, as marked.

## Q2. Can AP242 PMI (semantic dimensions/GD&T), part names, properties and material be read with open tools? What is the status of OCCT support?

### Takeaway
Yes, partially. OCCT's XDE reads **semantic** GD&T (dimensions, geometric tolerances, datums) and tessellated presentation through `XCAFDoc_DimTolTool`. It follows the CAx-IF AP242 PMI recommended practices, but completeness depends on the exporter, and forum reports show gaps such as values returning 0 and presentation PMI missing on export. User-defined attributes (custom properties) are read into `TDataStd_NamedData` on shape labels since roughly OCCT 7.8/7.9. Materials are read via `XCAFDoc_Material`. NIST's SFA is the reference open tool for inspecting and validating AP242 PMI. For SOLIDWORKS users, AP242 custom-property export exists (2024+, MBD "Publish to STEP242"), which makes STEP a viable carrier of part number and material metadata.

### Cited Findings
- XDE "can read and write GD&T data" of three types: dimensions, geometric tolerances and datums. It supports both "semantic presentation, i.e. data is stored in a machine-consumable way" and "tessellated presentation". The guide refers to the CAx-IF PMI Recommended Practices (rec_pracs_pmi_v40) — [OCCT xde.md](https://raw.githubusercontent.com/Open-Cascade-SAS/OCCT/master/dox/user_guides/xde/xde.md)
- GD&T is managed by `XCAFDoc_DimTolTool`, with attributes `XCAFDoc_Dimension`, `XCAFDoc_GeomTolerance` and `XCAFDoc_Datum`, and access objects `XCAFDimTolObjects_DimensionObject`, `XCAFDimTolObjects_GeomToleranceObject` and `XCAFDimTolObjects_DatumObject`. `GetRefShapeLabel` links a GD&T item to its shapes, and list methods return all dimensions, tolerances and datums — [OCCT xde.md](https://raw.githubusercontent.com/Open-Cascade-SAS/OCCT/master/dox/user_guides/xde/xde.md)
- The OCCT user guide (search snippet) says GD&T is imported only when assigned to shapes, edges, vertices or shape groups. Constructive geometry is not translated as referenced shapes — [OCCT forum/doc snippet](https://dev.opencascade.org/content/get-dimensions-data-stepap242)
- Forum report (snippet): reading Creo-exported AP242 dimensions, `getValues` on `XCAFDimTolObjects_DimensionObject` returned 0, and only the name and vertex points were available. The reply suggested re-exporting with different settings — [OCCT forum: Get dimensions data for STEP(AP242)](https://dev.opencascade.org/content/get-dimensions-data-stepap242)
- Forum report (snippet): a user loading NIST sample `nist_ctc_02_asme1_ap242-e2.stp` could list GD&T via the DimTol tool but struggled to display the tessellated labels — [opencascade.com forum](https://www.opencascade.com/content/xcaf-getting-gdt-visible-form)
- Export side: a developer using `STEPCAFControl_Writer` got only semantic PMI and no presentation PMI. The question was unresolved in the snippet — [OCCT forum](https://dev.opencascade.org/content/missing-presentation-pmi-step-export-stepcafcontrolwriter). Tracker issue 26859 covers AP242 export of dimensions, tolerances and datums — [OCCT tracker 26859](https://tracker.dev.opencascade.org/view.php?id=26859)
- **User-defined attributes (UDA):** the OCCT team announced STEP UDA import, available only through XCAF reading. Metadata is stored as a `TDataStd_NamedData` (key-value) attribute on the shape label and read with `GetNamedProperties`. The DRAW command `XGetProperties` dumps it — [OCCT forum: New feature – STEP Import UDA](https://dev.opencascade.org/content/new-feature-step-import-user-defined-attributes). A user on 7.8.0 could not find `SetMetaMode` and believed it arrived in 7.8.2 (**unverified**) — [OCCT forum: How to read UDA in 7.8.0](https://dev.opencascade.org/content/how-read-uda-data-latest-occ780); [OCCT 7.9.0 STEPCAFControl_Reader refman](https://dev.opencascade.org/doc/occt-7.9.0/refman/html/class_s_t_e_p_c_a_f_control___reader.html)
- **Material:** CadQuery's STEP importer reads `XCAFDoc_Material` via `MaterialRefGUID` (name, description, density, density unit), which shows material is reachable from Python through OCP — [CadQuery importers/assembly.py](https://raw.githubusercontent.com/CadQuery/cadquery/master/cadquery/occ_impl/importers/assembly.py)
- **SOLIDWORKS side:** since 2024, SOLIDWORKS MBD can export selected **custom properties** to STEP 242 ("Publish to STEP242", Custom Properties → Edit). The API exposes `IMBDSTEP242Data.CustomPropertiesToPublish`, valid when `PublishWithAllCustomProperties` is false. The 2025 dialog supports AP242 Editions 1.0–3.0 — [SW 2024 What's New: Exporting Custom Properties to STEP 242](https://help.solidworks.com/2024/English/WhatsNew/c_wn2024_mbd_exp_cust_prop_step_242.htm); [Publish to STEP242 PropertyManager 2025](https://help.solidworks.com/2025/english/SolidWorks/sldworks/r_publish_step242.htm); [API: CustomPropertiesToPublish](https://help.solidworks.com/2025/English/api/sldworksapi/SolidWorks.Interop.sldworks~SolidWorks.Interop.sldworks.IMBDSTEP242Data~CustomPropertiesToPublish.html); [MBD PMI export](https://help.solidworks.com/2025/English/SolidWorks/Sldworks/t_MBD_step_242_pmi_export.htm)
- **NIST SFA:** converts a STEP file to a spreadsheet of entities and attributes. It checks semantic and graphic PMI and validation properties against the CAx-IF recommended practices, and views graphic PMI, saved views and datum targets in a browser. Supports AP242/AP203/AP214. CAx-IF uses it for conformance checking. User guide NIST AMS 200-12 — [NIST SFA](https://www.nist.gov/node/413991); [MBx-IF](https://www.mbx-if.org/home/?p=1331); [NIST AMS 200-12](https://nvlpubs.nist.gov/nistpubs/ams/NIST.AMS.200-12.pdf); [GitHub usnistgov/SFA](https://github.com/usnistgov/SFA)
- Semantic PMI is machine-interpretable. Graphic PMI is visual appearance only and "is not intended to be computer-interpretable" — [NIST via search snippet](https://www.nist.gov/node/413991)

### Inferences
- For a SOLIDWORKS-centric team, reading PMI from AP242 is **not the critical path**. Drawing-level checks (title block, BOM table balloons, revision) are easier through the SOLIDWORKS API (`IDrawingDoc`, `IBomTableAnnotation`, `IView`). AP242 PMI is useful later for MBD or supplier exchange.
- If STEP is used as a cross-check, publish AP242 with the custom properties `PartNo`, `Material`, `Description` and `Revision`. Read them with an OCCT ≥7.9 build (OCP 7.9/8.x through CadQuery) via `GetNamedProperties`. That gives a vendor-neutral, CAD-license-free verification path, for example on CI servers without SOLIDWORKS.
- Use NIST SFA as the "ground truth" viewer when OCCT results look wrong (missing PMI, zero values). It shows whether the problem is in the exporter or in the reader.

### Gaps
- Not verified: whether OCP 8.x / pythonocc-core 7.9 expose `GetNamedProperties` and the DimTol object getters cleanly in Python (likely, since they wrap the full API, but untested).
- No open-source Python project was found that does end-to-end "AP242 semantic PMI → rule check" (for example, checking that every hole has a tolerance). This appears to be a gap in the ecosystem.
- The SFA license was not confirmed from the repo.

## Q3. Which open-source BOM comparison/reconciliation tools exist, and what matching logic do they use (part-number keys, fuzzy match, quantity rollup)?

### Takeaway
Open-source mechanical-BOM reconciliation tooling is thin. The most directly relevant is **kcarlton55/bomcheck**: a Python/pandas tool for SOLIDWORKS-vs-ERP (SyteLine) Excel comparison with multi-level support, a per-field mismatch matrix, unit-of-measure conversion and Excel output. The rest are small one-off scripts (mostly electronics/PCB BOM diffs) or PLM systems (OdooPLM, InvenTree add-ins). Matching is almost always an **exact key on part number within the parent assembly**, then a field-by-field compare of quantity, description and UoM. Fuzzy matching is rare and is best implemented in-house (for example with rapidfuzz on descriptions) as a "suggestion" column only.

### Cited Findings
**Catalog: BOM compare / reconciliation**

| Project | URL | Matching logic / features | Stack | Maturity | License | Limitations |
|---|---|---|---|---|---|---|
| **bomcheck** (+ bomcheckgui) | https://github.com/kcarlton55/bomcheck ; https://github.com/kcarlton55/bomcheckgui | Pairs files by name convention `PartNo_sw.xlsx` (CAD) vs `PartNo_sl.xlsx` (ERP). Supports **multilevel BOMs**: ERP via a `Level` column (0,1,2…), CAD via item numbers like `3`, `3.1`, `3.2.1`. Splits them into per-assembly single-level BOMs, then compares by (assy, item/part no). Output Excel has columns `assy, Item, IQDU, Q_sw, Q_sl, Description_sw, Description_sl, U_sw, U_sl`, where **IQDU** flags mismatches in Item, Quantity, Description and UoM. **UoM conversion** (in, ft, mm, m, sqft, gal, l, …) to the ERP's UoM, with lengths without units assumed to be inches by default. Configurable column names and default units via `bomcheck.cfg` | Python (pandas, openpyxl implied) | PyPI 2.7 released 2026-08-15; GitHub 2 stars; repo since 2018 | GPL-3.0-or-later (PyPI) | Single author and few users. File-naming convention required. GPL limits embedding in a proprietary internal tool if distributed (internal-only use is generally fine). Sources: [PyPI bomcheck](https://pypi.org/project/bomcheck/), [help](https://htmlpreview.github.io/?https://github.com/kcarlton55/bomcheck/blob/master/help_files/bomcheck_help.html) |
| BOMulus | https://github.com/L-A-Marchetti/BOMulus | Line-level diff of two BOM versions (added/deleted/updated lines) with a GTK GUI | Go | 14 stars; updated 2026-10-05 | (not verified) | Aimed at electronics BOM versioning |
| Trachelium123/BOM_Compare | https://github.com/Trachelium123/BOM_Compare | Automated BOM table comparison script (Chinese) | Python | 4 stars | (not verified) | Small personal script |
| ybkahveci/jlc_bomlist_checker | https://github.com/ybkahveci/jlc_bomlist_checker | Compares own BOM vs JLCPCB-generated BOM | Python | 3 stars | — | PCB-specific |
| Luis-123-long/BomCadCompare | https://github.com/Luis-123-long/BomCadCompare | Three-way consistency check (CAD coordinate file vs BOM vs placement-machine program) on designator, part no. and coordinates. A three-way-match pattern analogous to CAD vs ERP vs drawing | C# WPF | 1 star; 2026 | — | SMT-specific |
| polyfacet/aras-bom-compare | https://github.com/polyfacet/aras-bom-compare | BOM compare inside Aras Innovator PLM | JavaScript | 2 stars | — | Aras-specific |
| cjones-32/Aegis-BOM-Compare | https://github.com/cjones-32/Aegis-BOM-Compare | Diff of two BOM files for ECN/ECM release e-mails | Python | 1 star; 2023 | — | Small |
| ttran-tech/BOM-Compare, flask-bom-compare; swiftvic/BOM_comparison; astijusar/BomComparer; ArnasStanislovaitis/BOMComparer | (GitHub) | Simple two-file diffs | Java/Python/C# | ≤1 star | — | Toy/exercise level |
| OmniaGit/odooplm | https://github.com/OmniaGit/odooplm | Open-source PLM/PDM for Odoo with CAD integration (SOLIDWORKS, Inventor, Solid Edge…) and BOM revisioning. Engineering BOM to manufacturing BOM inside one ERP | Python (Odoo) | 164 stars; updated 2026-10-19 | (not verified; Odoo modules usually AGPL/LGPL) | Requires adopting Odoo |
| jrobelia/solidworks-inventree-addin | https://github.com/jrobelia/solidworks-inventree-addin | SOLIDWORKS task-pane add-in that fetches, **compares** and applies properties to/from the InvenTree inventory server and pushes BOMs | C# | 3 stars; 2026 | — | InvenTree-specific, but shows a "CAD property ↔ inventory master" compare pattern |
| pawelcel/EasyPDM | https://github.com/pawelcel/EasyPDM | Self-hosted PDM with part numbering, revisions and BOM, plus macros for SOLIDWORKS/Inventor/FreeCAD | TypeScript | 9 stars; 2026-08 | — | New |
| reikus/xlBOM-with-thumbnails | https://github.com/reikus/xlBOM-with-thumbnails-Community-Edition | SOLIDWORKS add-in exporting the BOM to Excel with thumbnails (an idea for the review-sheet layout) | — | 4 stars | — | — |
| CycloneDX/sbom-comparator | https://github.com/CycloneDX/sbom-comparator | Software-BOM compare (Lockheed Martin). Not mechanical, but a good model of the diff-report structure | Java | 18 stars; **archived** | (Apache-2.0 likely; not verified) | Software SBOM |

- Commercial references for the matching approach: Global Shop Solutions "ERP BOM Compare" flags differences and **checks parts against the inventory/item master** — [SoftwareOne listing](https://platform.softwareone.com/product/global-shop-solutions-erp-bom-compare/PCP-7436-3592)

### Inferences
- **Recommended matching pipeline** (synthesized from bomcheck plus common practice):
  1. **Normalize the part number:** NFKC-normalize (to fold full-width Korean-IME characters), trim, uppercase, unify hyphen variants (‐ - – —), and strip revision suffixes (e.g. `-R01`, `_A`) into a separate `rev` field.
  2. **Explode** each source to (parent PN, child PN, qty-per, UoM) edges. Multi-level input comes from `Level` or dotted item numbers, as in bomcheck.
  3. **Roll up** duplicate child rows under the same parent: sum quantities, because CAD BOMs may list the same part in several rows across configurations or "parts only" vs "indented" views.
  4. Do a **full outer join** on (parent PN, child PN). Classify each row: only-in-CAD, only-in-ERP, qty mismatch, UoM mismatch, description mismatch, rev mismatch.
  5. Optional: **flattened compare** (total quantity per leaf part across the whole tree). This catches "structure differs but material is the same" cases, for example when ERP has a phantom or purchased sub-assembly that CAD models as individual parts.
  6. **Fuzzy suggestions** only for unmatched rows. Use rapidfuzz on the normalized PN (catches transposed digits, O/0 swaps) and on the description. Show them as "possible match" for human review, never auto-match.
- bomcheck's **IQDU flag column** and its `_sw`/`_sl` side-by-side layout are a proven, readable report design that can be copied directly into the Excel review sheet. Because of GPL-3.0, consider reimplementing the ideas rather than vendoring the code if the tool might be distributed outside the company.
- The three-way pattern (BomCadCompare: CAD vs BOM vs machine program) maps to the team's **CAD model vs drawing parts list vs ERP**. Join all three on the normalized PN with a presence bitmask such as `CDE` (CAD/Drawing/ERP), analogous to IQDU.

### Gaps
- No open-source tool was found that specifically reconciles **drawing parts-list (BOM table in .SLDDRW/PDF)** against the model and ERP. This will need custom SOLIDWORKS API code (`IBomTableAnnotation` → rows).
- No mature open-source library dedicated to part-number normalization was found. It is typically company-specific regex rules.

## Q4. What checks are typical in CAD-vs-ERP BOM validation, per industry articles?

### Takeaway
Industry sources (mostly vendor blogs such as OpenBOM and Industrios, plus bomcheck's design) converge on a short list of checks:
- part-number existence and completeness
- quantity mismatch, including UoM conversion
- revision mismatch / stale data
- description mismatch
- structural mismatch (engineering vs manufacturing structure, phantoms, purchased sub-assemblies)
- missing make/buy, material and compliance attributes (RoHS/REACH/ITAR)

The evidence base is vendor marketing. No independent, standardized checklist was found.

### Cited Findings
- OpenBOM "10 BOM review mistakes" (search snippet; the page could not be fetched):
  - quantity and **unit** errors look complete and surface only at ordering or first build
  - engineering, suppliers and ERP can each use a different UoM
  - CAD assembly logic often does not match manufacturing needs (build sequence, kitting, purchased subassemblies), causing ERP planning/costing problems
  - compliance data (RoHS, REACH, ITAR) is often omitted
  - recommends an explicit review step verifying **part-number completeness and units** before release

  Source: [OpenBOM blog](https://www.openbom.com/blog/plm-and-bom-management/bom-review-mistakes-quality-cost-supply-chain)
- Industrios: manual re-entry between CAD and ERP produces wrong quantities, mismatched part numbers and outdated revisions — [Industrios: reduce BOM errors](https://industrios.com/news/read/reduce-bom-errors-cad-erp/); [Industrios: CAD-ERP integration](https://industrios.com/?p=3389)
- CAD→ERP integration overview article — [ERP Software Blog (2025-11)](https://erpsoftwareblog.com/2025/11/how-a-cad-to-erp-integration-works/); AI-assisted BOM sync between CAD/PDM/ERP — [getleo.ai blog](https://www.getleo.ai/blog/bom-sync-cad-pdm-erp-ai)
- KPIs for whether reconciliation is working: BOM-related build errors, MRP exceptions, emergency POs — [Racklify](https://racklify.com/encyclopedia/bill-of-materials-for-product-lifecycle-management-integrating-bom-with-plm-and-erp/)
- bomcheck's built-in checks are Item presence, Quantity, Description and UoM (with unit conversion) — [PyPI bomcheck](https://pypi.org/project/bomcheck/)
- XDE validation properties (volume, centroid, area) let a receiving system check that geometry was not altered — [OCCT xde.md](https://raw.githubusercontent.com/Open-Cascade-SAS/OCCT/master/dox/user_guides/xde/xde.md)

**Excel review report (openpyxl) patterns**
- openpyxl 3.1.5 (released 2024-06-28, MIT) is the current stable release (PyPI JSON). Relevant features: `PatternFill`/`Font` styling, `ConditionalFormatting` (CellIsRule/FormulaRule), `DataValidation` (drop-down for reviewer disposition: OK / Fix CAD / Fix ERP / Waive), `ws.freeze_panes`, `ws.auto_filter`, comments, hyperlinks to other sheets, and row grouping (`ws.row_dimensions.group`) for indented multi-level BOMs — [openpyxl docs](https://openpyxl.readthedocs.io/en/stable/)
- The bomcheck output layout (side-by-side `_sw`/`_sl` columns plus an `IQDU` flag string) is a field-tested report format — [PyPI bomcheck](https://pypi.org/project/bomcheck/)

### Inferences
- **Suggested rule set for the semiconductor-equipment team**, mapped onto the SOLIDWORKS API → JSON → rules → Excel flow:
  1. PN present and matching the company PN regex. No blank PN, no file-name-as-PN (e.g. "Part1", "Copy of", Korean temp names).
  2. PN exists in the ERP item master and is not obsolete.
  3. Per-parent quantity equal (CAD vs ERP vs drawing BOM table), and flattened total quantity equal.
  4. UoM consistent (EA vs M for cut lengths of cables, tubes and aluminum profiles; convert units).
  5. Revision equal (model custom property vs drawing title block vs ERP).
  6. Description/spec equal after whitespace and case normalization (Korean/English mix: compare normalized, show raw).
  7. Make/Buy and material consistent with the ERP item type.
  8. Exclude-from-BOM, virtual, envelope and suppressed components are handled consistently.
  9. Duplicate PNs with different descriptions, or the same description with different PNs (possible duplicate item creation).
  10. Phantom/purchased sub-assembly structure differences flagged as "structural" rather than errors.
  11. Optional geometry sanity: mass/volume vs the ERP weight field, or STEP validation properties.
- **Reusable components summary:**
  - OCP/CadQuery `importers/assembly.py` for STEP traversal (Apache-2.0)
  - pythonocc-core for full OCCT access (LGPL)
  - bomcheck for multi-level-split and UoM-conversion logic, and its report layout (GPL-3.0, so reuse ideas or use as an external tool)
  - NIST SFA for STEP/PMI inspection
  - openpyxl + pandas for I/O and report
  - rapidfuzz for fuzzy suggestions (not researched here; MIT, widely used)
  - openswx as an experimental no-SOLIDWORKS reader for .SLDASM, worth watching but not production-ready

### Gaps
- No peer-reviewed or independent study quantifying which CAD-vs-ERP mismatch types are most frequent. Available figures (e.g. "$45,000 scrap from a decimal error") are anecdotal vendor content.
- Korean-language industry sources on BOM verification for semiconductor equipment were not searched in this pass.
- Arena (PTC) and other PLM vendor blogs could not be fetched. The OpenBOM page was available only as a search snippet.
