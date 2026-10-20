# SOLIDWORKS API / Document Manager open-source tools for drawing and BOM extraction and checking (as of 2026-10-20)

Method note: repo facts were collected 2026-10-20 through the GitHub search API (GitHub MCP), which returns stars, `pushed_at` (last push) and license, and from README pages fetched with WebFetch. `updated_at` in search listings is NOT last commit (it changes on stars too). Where only `updated_at` was available, this is marked "(updated_at, not last push)". codestack.net and help.solidworks.com could NOT be fetched (DNS failure in this sandbox), so API facts from those sites come from search snippets and other repos' code and are marked as such.

## 1. Which GitHub repos do drawing checking / drawing QA / BOM export / custom-property audit with the SOLIDWORKS API?

### Takeaway
There is no mature, widely-starred open-source "SOLIDWORKS drawing checker". The field is fragmented. It consists of (a) one large macro/example library (xarial/codestack, 385★), (b) small single-purpose BOM-export or property tools, mostly 0–5★ and often stale, and (c) a 2026 wave of new Python/COM "agent", "MCP" and "design review" projects. Of these, coleas33/smart_SW and THERMOPAC's extractor are the closest to the planned "extract → JSON → Python rules → report" architecture. All of these are very young or low-adoption.

### Cited Findings
**Closest architectural analogues (extract → JSON → rule check)**
- **coleas33/smart_SW** is described as a "SOLIDWORKS agentic design review pilot: extractor add-in, Python reviewer, Task Pane assistant". It was created 2026-09-13 and has 2★ (updated_at 2026-10-19). Per the README, the extractor is C# on .NET Framework 4.8 x64 against the SOLIDWORKS 2024 API, and it reads the model "through a read-only guard". The Python reviewer (uv, Python 3.11+) has IR models, unit handling, deterministic checks, YAML standards profiles and reporting. The `check standards` command writes session.json, check.json, attention.json and report.md. Its design principle: "Numbers come from tested code: the model decides what to investigate and explains, and never computes a verdict". The license is AGPL-3.0-or-later because it depends on PyMuPDF. It has 831 commits. It is a pilot on one licensed SW2024 seat, and its extractor data coverage and JSON schema are not documented in the README. — [smart_SW](https://github.com/coleas33/smart_SW)
- **THERMOPAC/ThermopacCommunication-R1** has a `local-agent/extractor/extract_dimensions.py` written in Python/pywin32.
  - It first tries `swModel.GetDimensionNames` → `Parameter(name)` → `IsReference()`, `GetTolerance()` (`tol.Type != 0` means toleranced), `GetSystemValue2("")`.
  - It falls back to walking drawing views with `GetFirstView`/`GetNextView`, then `GetDisplayDimensions` or `GetFirstDisplayDimension5`/`GetNext5`, then `GetDimension2(0)`.
  - It reads tolerance `MinValue`/`MaxValue`, converts values to mm, and returns counts plus a 20-item sample as a dict (JSON-ready).
  - It uses a defensive "try several method names" helper (`get_com_value`) to survive late-binding and version differences.
  - Repo metadata (stars, license) was not checked. — [extract_dimensions.py](https://github.com/THERMOPAC/ThermopacCommunication-R1/blob/HEAD/local-agent/extractor/extract_dimensions.py)
- **rd3n-ai/iso7200-cad-standard** is an "Open engineering-metadata standard ... SSOT model properties, ISO 7200 title blocks, machine-readable audit rulebook. SolidWorks reference implementation." It is Python, Apache-2.0 and 1★, created 2026-08-22, with last push 2026-08-22 (a one-day dump). It is useful as a rulebook / property-schema reference for title-block audits. — [GitHub search API](https://github.com/rd3n-ai/iso7200-cad-standard)
- **Yanru1004/bom_check** ("Check Bom and Solidworks part list") is Python, 0★ (updated_at 2026-05-24). Its contents were not inspected. — [repo](https://github.com/Yanru1004/bom_check)
- **easyhard01/EdmBOM_CSharp** is a "CMP equipment BOM management comparison tool (SolidWorks EPDM + Excel)". It is C#, 1★, created 2026-09-20. It is a semiconductor-equipment (CMP) BOM comparison and is domain-relevant to the Korean team. Its contents were not inspected. — [repo](https://github.com/easyhard01/EdmBOM_CSharp)

**BOM export / BOM extraction**
- **matt454357/sw-BOM-Scan** ("Extract bill of materials from SolidWorks assembly files into sqlite database file") is C#, 2★, with last activity around 2023-07 (updated_at). It is a stale but simple reference for assembly traversal → DB. — [repo](https://github.com/matt454357/sw-BOM-Scan)
- **reikus/xlBOM-with-thumbnails-Community-Edition** is a "SOLIDWORKS add-in that exports Bill of Materials to Excel with thumbnails". It has 4★ (updated_at 2026-03-26). — [repo](https://github.com/reikus/xlBOM-with-thumbnails-Community-Edition)
- **Filipsv/BOMmakro** is a VBA "Macro for export BOM from SolidWorks to Excel", 1★, created 2017. — [repo](https://github.com/Filipsv/BOMmakro)
- **Varunkumar-HN/SolidWorks-Macros** contains macros for balloons, BOMs, views and blocks in multi-sheet drawings. It has 1★ and was created 2025-08. — [repo](https://github.com/Varunkumar-HN/SolidWorks-Macros)
- **jrobelia/solidworks-inventree-addin** is a C# task-pane add-in that compares and pushes properties and BOMs to InvenTree. It has 3★ and 25 open issues (updated_at 2026-10-01). It is a reference for property-diff UX. — [repo](https://github.com/jrobelia/solidworks-inventree-addin)
- **OmniaGit/odooplm** is an open-source PLM for Odoo with SolidWorks integration and BOM revisioning. It is Python, 164★, and active (updated_at 2026-10-19). It is a PLM rather than a checker. — [repo](https://github.com/OmniaGit/odooplm)

**Custom properties / references**
- **ramugopal92/solidworks-custom-properties-manager** is a VBA batch editor for custom properties, 1★, created 2026-03. — [repo](https://github.com/ramugopal92/solidworks-custom-properties-manager)
- **robkite/Obtain-SolidWorks-Custom-Properties** is C# "example code to obtain SolidWorks custom properties both via the application and the Document Manager API". It has 1★ and was created 2016. — [repo](https://github.com/robkite/Obtain-SolidWorks-Custom-Properties)
- **stefansterk/SWhereUsed** is a Python web app that shows where-used for parts, assemblies and drawings "without opening SOLIDWORKS". It indexes files with the Document Manager. It has 1★ and was created 2026-10-04. — [repo](https://github.com/stefansterk/SWhereUsed)
- **jasonnicholson/ReplaceReference** is C# and replaces references in a drawing, assembly or part. It is archived, 3★. — [repo](https://github.com/jasonnicholson/ReplaceReference)

**Drawing batch export (related batch patterns)**
- **buianhtuan2903/autoexport-solidwork** is a C# desktop tool to auto-export drawings to PDF/DWG, 29★ (updated_at 2026-10-07). — [repo](https://github.com/buianhtuan2903/autoexport-solidwork)
- **kilwizac/solidworks-automate-drawing** is a VBA macro that creates views, imports dimensions and "copies title-block properties". It has 1★ and was created 2026-07. — [repo](https://github.com/kilwizac/solidworks-automate-drawing)

**Built-in / commercial alternative (not open source)**
- SOLIDWORKS Design Checker (included in SOLIDWORKS Premium) has a "Dangling Dimension / Annotation" check under the Drawings tab that lists dangling items across multi-sheet drawings. — [Javelin blog 2019](https://www.javelin-tech.com/blog/2019/04/dangling-dimensions-solidworks-design-checker/)

### Inferences
- No open-source project found implements a full drawing QA ruleset covering tolerance presence, override text, dangling annotations, title-block completeness, BOM-vs-assembly consistency and revision-table checks. The team's tool would be novel in scope. Reusable pieces are code patterns (THERMOPAC extractor, codestack macros) and architecture (smart_SW's IR → deterministic checks → JSON/Markdown report).
- smart_SW's AGPL license makes code reuse in an internal tool legally sensitive if it is ever distributed. Treat it as an architecture reference only.
- The 2026 surge of SOLIDWORKS "MCP server" repos (e.g., Slacker-LLC/solidworks-mcp 22★, alisamsam/Solidworks-MCP 174★, haunchen/solidworks-mcp 7★) focuses on modeling and drawing creation, not checking. Their COM-wrapping code in Python/pywin32 is a source of working call patterns.

### Gaps
- Commit counts and last-push dates for most small repos could not be verified, because gh repository-scoped API access was blocked and search only gives updated_at.
- The contents of bom_check, EdmBOM_CSharp and xlBOM were not inspected.
- No repo was found matching "pySldWrap" or "swpy" by those names. Python wrapper equivalents are listed in Q2.

## 2. Frameworks/libraries that make this easier (license, maturity, what they expose)

### Takeaway
For .NET, xCAD.NET (MIT, about 1,000 commits, 217★, pushed 2026-09-28) is the most capable and actively maintained. Its main differentiator is a unified API over both SOLIDWORKS and Document Manager. CodeStack (MIT, 385★) is the largest example and macro library. For Python, the options are thin wrappers: pyswx (MIT, beta, SW2024+), swtoolkit and pySolidWorks (both stale since 2021–2023), and SolidWrap. The planned tool will mostly call raw COM through pywin32 and use these wrappers as references.

### Cited Findings
| Project | Lang | ★ | Last push | License | Notes |
|---|---|---|---|---|---|
| [xarial/xcad](https://github.com/xarial/xcad) | C# | 217 | 2026-09-28 | MIT | "Framework for developing CAD applications for SOLIDWORKS, including add-ins, stand-alone applications, macro features, property manager pages". 1,010 commits. NuGet Xarial.XCad.SolidWorks. Same traversal code works for SOLIDWORKS and Document Manager. — [README](https://github.com/xarial/xcad) |
| [xarial/xcad-examples](https://github.com/xarial/xcad-examples) | C# | 88 | (updated_at 2026-09-20) | n/a | Add-in and stand-alone examples built with xCAD |
| [xarial/codestack](https://github.com/xarial/codestack) | VBA/C# | 385 | 2026-05-07 | MIT | "Library of SOLIDWORKS API, SOLIDWORKS PDM API, VBA and Desktop code examples, applications macros"; 460 open issues; source of codestack.net |
| [CAD-Booster/SolidDNA](https://github.com/CAD-Booster/SolidDNA) | C# | 160 | (updated_at 2026-10-15) | MIT | Fork of AngelSix SolidDNA (original "wasn't being maintained"); 827 commits; NuGet CADBooster.SolidDna; .NET Framework 4.8; add-in focused — [README](https://github.com/CAD-Booster/SolidDNA) |
| [weianweigan/SolidWorksLookup](https://github.com/weianweigan/SolidWorksLookup) | C# | 110 | (updated_at 2026-10-19) | n/a | Interactive API object explorer, useful for discovering properties on views and annotations |
| [deloarts/pyswx](https://github.com/deloarts/pyswx) | Python | 22 | 2026-01-26 | MIT | Typed wrapper from SW2024 API help; Python 3.12; SW2024+/Win11; beta, v0.6.0, 78 commits; ICustomPropertyManager complete; ByRef outputs returned as tuples; unimplemented methods raise NotImplementedError; raw `com_object` accessible — [README](https://github.com/deloarts/pyswx) |
| [Glutenberg/swtoolkit](https://github.com/Glutenberg/swtoolkit) | Python | 74 | 2021-03-29 | MIT | "SolidWorks Toolkit for Python" — stale |
| [mklanac/pySolidWorks](https://github.com/mklanac/pySolidWorks) | Python | 78 | 2023-03-09 | GPL-3.0 | Open model, modify dims, export DXF, FEA — stale; GPL |
| [SeanYeatts/SolidWrap](https://github.com/SeanYeatts/SolidWrap) | Python | 14 | 2025-02-03 | MIT | Wrapper for SolidWorks and PDM APIs |
| [ldevillez/pySwTools](https://github.com/ldevillez/pySwTools) | Python | 10 | (updated_at 2026-04-28) | n/a | CLI tools |
| [muerus/SolidWorks-Python](https://github.com/muerus/SolidWorks-Python) | Python | 9 | (created 2026-10-06) | n/a | Embedded CPython add-in via pythonnet, "Pythonic API" — very new |
| [schwitters/openswx](https://github.com/schwitters/openswx) | C++20 | 31 | 2026-10-09 | MIT | Reads SLDPRT/SLDASM/SLDDRW with NO SolidWorks install (see Q5) |
| [BlueByteSystemsInc/BlueByte.SOLIDWORKS.DocumentManager.Extensions](https://github.com/BlueByteSystemsInc/BlueByte.SOLIDWORKS.DocumentManager.Extensions) | C# | 0 | (2022) | n/a | Extension methods for the DM API |
| [kilwizac/solidworks-api-mcp](https://github.com/kilwizac/solidworks-api-mcp), [Erfouni/solidworks-drawing-api-2026](https://github.com/Erfouni/solidworks-drawing-api-2026), [skarpsill/SWAPI](https://github.com/skarpsill/SWAPI) | TS/Py | 9/4/2 | 2026 | n/a | Searchable SOLIDWORKS API documentation databases (e.g., "14,000+ methods") for AI-assisted coding |

All star and push data above come from the GitHub search API, queried 2026-10-20.

### Inferences
- For a Python-first team, using pywin32 directly plus a small in-house helper layer (the THERMOPAC-style multi-name getattr) is likely more robust than adopting any Python wrapper. All Python wrappers are either stale or beta.
- If a C# extractor (headless console app) is acceptable, xCAD.NET (MIT) reduces boilerplate and allows switching to Document Manager for property and reference-only reads. Python would still do the rules and Excel stages.

### Gaps
- xCAD's specific coverage of drawing annotations and tables was not confirmed. The README fetch did not mention drawings, sheets, views or tables, and xcad.net docs were not fetched.
- The original angelsix/solidworks repo was not located by search. CAD-Booster/SolidDNA is the maintained continuation.

## 3. Concrete API methods for extraction, and version pitfalls

### Takeaway
The standard drawing traversal is: sheets → views (`GetFirstView`/`GetNextView`; the first "view" is the sheet itself) → display dimensions (`GetFirstDisplayDimension5`/`GetNext5`, or `GetDisplayDimensions`) → `GetDimension2` / `GetTolerance`, plus the annotation chain (`GetFirstAnnotation3`/`GetNext3`). Dangling status is `IAnnotation.IsDangling`, not a display-dimension method. Override detection is weakly supported: you get a boolean flag, and the text has to be reconstructed from prefix/suffix/callout parts. BOM tables can be read from the drawing (TableAnnotation) or headlessly via Document Manager.

### Cited Findings
- **Dimension traversal and tolerance (working Python code):** `GetFirstView` → `GetNextView`; `GetDisplayDimensions` or `GetFirstDisplayDimension5`/`GetNext5`; `GetDimension2(0)`; `GetTolerance()` with `.Type` (0 = none), `MinValue`/`MaxValue`; `GetSystemValue2("")` returns meters; `IsReference()` flags driven dimensions. The code tries `GetNext5`, then `GetNext4`, then `GetNext`, to survive version differences. — [THERMOPAC extractor](https://github.com/THERMOPAC/ThermopacCommunication-R1/blob/HEAD/local-agent/extractor/extract_dimensions.py)
- **Official example:** SOLIDWORKS help has "Get Dimension Values in Drawing" examples in VBA, VB.NET and C# for 2010–2022. — [help 2021 C#](https://help.solidworks.com/2021/english/api/sldworksapi/get_dimension_values_in_drawing_example_csharp.htm), [help 2022 VB.NET](https://help.solidworks.com/2022/English/api/sldworksapi/Get_Dimension_Values_in_Drawing_Example_VBNET.htm)
- **Dangling annotations:**
  - Forum VBA loops views with `GetFirstAnnotation3`/`GetNext3`, tests `IsDangling`, and deletes. — [Visiativ forum](https://forum.mycad.visiativ.com/t/comment-supprimer-les-cotes-bancales-en-vba-sur-solidworks2016/104848)
  - A Reddit user notes `IsDangling` is on the annotation object, not the display dimension. Iterating display dimensions only covers dimensions, not notes or GTols. — [r/SolidWorks mirror](https://red.127.pp.ua/r/SolidWorks/comments/1jjpt8a/dangling_annotations_macro)
  - Other threads cover finding and deleting dangling dimensions. — [3DSwym: delete dangling](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/delete-all-the-dangling-dimensions_GjZHVgFpSo-VcVvyOZ1i9A), [3DSwym: find dangling by custom prop](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/how-to-find-dangling-dimension-by-custom-prp_PyLXO5ZiSECdPhHV8wjGoQ)
- **Overridden dimension text:**
  - Forum users report the override API returns only a boolean ("I can tell a dimension is overwritten, but still no value"), and `GetText` does not directly return the override value. — [3DSwym: get dimension override value text](https://3dswym.3dexperience.3ds.com/post/solidworks-user-forum/get-dimension-override-value-text-api_1hJFPIE3T6C51lPeVN3Daw), [3DSwym: code to find overwritten dimensions](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/code-to-find-overwritten-dimensions_TTaX_VdVTyG_md06K9MBTg)
  - Another approach reads `GetText(swDimensionTextPrefix/Suffix/...)` parts. The prefix can contain full text like "20X .201 THRU ALL". — [3DSwym: displayed text length](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/find-the-displayed-dimension-text-string-length_61udg8QjRWy37hMPLSe3TQ), [3DSwym: detecting dimension value that's not dim](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/detecting-dimension-value-thats-not-dim_JgrE1TORSvWnsCpyPShsIQ)
  - Per the search summary of the help enum, `swDimensionTextAll` is "used only by SetText", so it cannot be used to read text. This was not directly verified because help.solidworks.com was unreachable.
- **BOM tables (full API):**
  - `IBomFeature::GetTableAnnotations` gets table annotations from the BOM feature. — [help 2019](https://help.solidworks.com/2019/english/api/sldworksapi/SolidWorks.Interop.sldworks~SolidWorks.Interop.sldworks.IBomFeature~GetTableAnnotations.html)
  - Pitfall: a user reports it returns an empty array even when the BOM is populated, with no resolution in the thread. — [3DSwym](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/gettableannotations-method-ibomfeature-returns-empty-array_A0wbYZ9KTk6_e-_WNAw76A)
  - The legacy `IBomTable` (Excel-based BOM) requires `Attach3` before use and `Detach` after. — [help 2013 IBomTable::GetHeaderText](https://help.solidworks.com/2013/English/api/sldworksapi/SolidWorks.Interop.sldworks~SolidWorks.Interop.sldworks.IBomTable~GetHeaderText.html)
- **Custom properties (Document Manager):**
  - `ISwDMDocument::GetCustomProperty` / `ISwDMConfiguration::GetCustomProperty` return the evaluated value "from when the file was last saved in SOLIDWORKS". Call `GetCustomPropertyNames` first. — [DM help 2015](https://help.solidworks.com/2015/English/api/swdocmgrapi/SOLIDWORKS.Interop.swdocumentmgr~SOLIDWORKS.Interop.swdocumentmgr.ISwDMDocument~GetCustomProperty.html)
  - `ISwDMConfiguration14::GetCustomProperty2` returns values without the "fromparent+" prefix for external referenced documents. — [DM help 2023](https://help.solidworks.com/2023/English/api/swdocmgrapi/SOLIDWORKS.Interop.swdocumentmgr~SOLIDWORKS.Interop.swdocumentmgr.ISwDMConfiguration14~GetCustomProperty2.html)
  - `ISwDMConfiguration4::GetAllCustomPropertyNamesAndValues` is also available. — [DM help 2021](https://help.solidworks.com/2021/english/api/swdocmgrapi/SolidWorks.Interop.swdocumentmgr~SolidWorks.Interop.swdocumentmgr.ISwDMConfiguration4~GetAllCustomPropertyNamesAndValues.html)
- **pywin32 ByRef pitfall:** `OpenDoc6` takes ByRef `Errors`/`Warnings`. From Python they must be passed as `win32com.client.VARIANT(pythoncom.VT_BYREF | pythoncom.VT_I4, 0)`. Plain ints cause "Type mismatch". — [OpenDoc6 help 2015](https://help.solidworks.com/2015/english/api/sldworksapi/SolidWorks.Interop.sldworks~SolidWorks.Interop.sldworks.ISldWorks~OpenDoc6.html), [StackOverflow mirror](https://developer.cloud.tencent.cn/ask/sof/107545106), [MATLAB same error](https://se.mathworks.com/matlabcentral/answers/2130401-type-mismatch-error-using-solidworks-api-in-matlab)
- pyswx handles ByRef by returning tuples (result, errors, warnings). — [pyswx README](https://github.com/deloarts/pyswx)

### Inferences
These are the author's working knowledge of the SOLIDWORKS API and were NOT verified in this session, because help.solidworks.com was unreachable. Verify each one against the help for the team's SW version.
- GD&T: `IView.GetFirstGTOL` → `IGtol.GetNextGTOL`; `IGtol.GetFrameValues` / `GetFrameSymbols3`; datums via `GetFirstDatumTag`.
- Notes: `IView.GetFirstNote` → `INote.GetNext`; `INote.GetText`; `INote.IsBomBalloon`. Title-block notes live on the sheet-format "view" (the first view returned by `GetFirstView`), and linked properties appear as `$PRP:"..."` / `$PRPSHEET:"..."` in raw text.
- Tables: `IView.GetTableAnnotations` → `ITableAnnotation.Type` (swTableAnnotation_BillOfMaterials, swTableAnnotation_RevisionTable), `RowCount`, `ColumnCount`, `Text(r,c)` / `DisplayedText(r,c)`, `RowHidden`. `IBomTableAnnotation.GetComponents2(row, config)` links BOM rows to components, enabling BOM-vs-assembly checks. Revision table: `ISheet.RevisionTable` → `IRevisionTableAnnotation`.
- Display dimension details: `IDisplayDimension.GetOverride`, `GetText(swDimensionTextPrefix|Suffix|CalloutAbove|CalloutBelow)`, `ShowParenthesis`, `IsReferenceDim`. `IAnnotation.IsDangling` applies to all annotation types.
- Custom properties (full API): `IModelDocExtension.CustomPropertyManager(config).Get6(name, useCached, out val, out resolved, out wasResolved, out linkToProp)` (Get5/Get4 in older versions); `GetNames`.
- Version pitfall: numbered method variants (`GetNext5` vs `GetNext4`, `Get6` vs `Get5`, `GetAllExternalReferences5` vs `4`) differ by release. Pin the API level to the oldest SW version used in-house, or use the try-fallback pattern.

### Gaps
- The codestack.net macro pages (e.g., "find dangling annotations", "export BOM to Excel", "find overridden dimensions") could not be fetched (DNS failure). GitHub code search in xarial/codestack for `IsDangling` and `GetOverride` returned 0 hits, probably because of code-search indexing limits. Direct page URLs and exact macro names therefore remain unverified.
- No authoritative source was found for reliably reading override text on newer versions. The forum threads lack resolutions.

## 4. Patterns: batch processing, headless/invisible SOLIDWORKS, export to JSON/Excel

### Takeaway
Open-source tools batch-process either by driving a running or invisible SOLIDWORKS over COM (pywin32 / C# console) or by bypassing SOLIDWORKS with Document Manager or file parsers. The JSON-first, multi-artifact output in smart_SW and the defensive soft-failure extraction in THERMOPAC are the most reusable patterns.

### Cited Findings
- THERMOPAC's extractor uses a soft-failure design ("errors logged, partial data returned"), returns plain dicts (JSON-serializable), and caps loops (view_index < 100, dim_index < 500). — [extract_dimensions.py](https://github.com/THERMOPAC/ThermopacCommunication-R1/blob/HEAD/local-agent/extractor/extract_dimensions.py)
- smart_SW splits work into a C# extractor (IR dump), a Python deterministic checker and a report stage. Outputs are session.json, check.json, attention.json and report.md, and the standards profiles are YAML. — [smart_SW](https://github.com/coleas33/smart_SW)
- yunylll/solidworks-urdf-tool describes itself as "Headless SolidWorks 2026 → URDF/STL exporter. Drive it from CLI, Python or MCP" (3★, created 2026-10-07). infei12306/solidworks-bridge includes logic to "recover a session wedged on a modal dialog" (3★). — [urdf-tool](https://github.com/yunylll/solidworks-urdf-tool), [solidworks-bridge](https://github.com/infei12306/solidworks-bridge)
- Batch export tools exist: buianhtuan2903/autoexport-solidwork (C#, 29★, drawings → PDF/DWG) and Johanss-on/Assem2DXF (macro). — [autoexport](https://github.com/buianhtuan2903/autoexport-solidwork), [Assem2DXF](https://github.com/Johanss-on/Assem2DXF)
- sw-BOM-Scan exports to SQLite. xlBOM and BOMmakro export to Excel. — [sw-BOM-Scan](https://github.com/matt454357/sw-BOM-Scan), [xlBOM](https://github.com/reikus/xlBOM-with-thumbnails-Community-Edition)

### Inferences
These are suggested design choices for the team's tool, not sourced findings.
- Start SOLIDWORKS with `win32com.client.DispatchEx("SldWorks.Application")` (or a versioned ProgID such as `SldWorks.Application.32` for SW2024), set `Visible=False`, and use `OpenDoc6` with Silent and ReadOnly options. Use `DocumentVisible(False, type)` to suppress windows, and `CloseDoc` after each file.
- Restart SOLIDWORKS every N files to avoid memory growth and modal-dialog hangs, which solidworks-bridge's "wedged session recovery" feature confirms happen.
- Run extraction in a subprocess with a timeout.
- Emit one JSON per drawing (schema-versioned). Run Python rules (pydantic models plus a YAML rulebook, as in smart_SW / iso7200-cad-standard). Write Excel with openpyxl, with one row per finding and a hyperlink/ID back to sheet, view and annotation name.

### Gaps
- No primary benchmark data was found on throughput (files/hour) or on licensing terms for unattended COM automation. The SOLIDWORKS EULA terms for server or unattended use were not researched.

## 5. Document Manager API usage on GitHub (no SOLIDWORKS needed) and license-key handling

### Takeaway
Document Manager (DM) can read custom properties, configurations, external references and table cell text (including BOMs) from saved files without running SOLIDWORKS. It needs a license key, which is free for subscription customers on request. Python usage is via win32com `SwDocumentMgr.SwDMClassFactory`. GitHub examples are few and small. openswx is an MIT, license-free alternative parser, but it is new and does not cover drawing annotations.

### Cited Findings
- **Python DM pattern (yzlin286-wq/sw_drawing_studio, `app/services/docmgr_service.py`):**
  - Calls `win32com.client.Dispatch("SwDocumentMgr.SwDMClassFactory")`, then `.GetApplication(key)`, then `app.GetDocument(path, 3 /*swDmDocumentDrawing*/, readOnly)`, then `GetAllExternalReferences5()` with a fallback to `GetAllExternalReferences4()`, then `ReplaceReference`.
  - The license key is resolved from env var `SW_DM_LICENSE_KEY`, then config/docmgr.yaml, then config/app.yaml.
  - A probe function checks for the interop DLL under `C:\Program Files\SOLIDWORKS Corp\...` and returns a warning instead of failing when no key is present.
  - Repo stars and license were not checked. — [docmgr_service.py](https://github.com/yzlin286-wq/sw_drawing_studio/blob/HEAD/app/services/docmgr_service.py)
- DM help has a "Get Table Cell Text" example (C# and VB.NET) for reading BOM table cell text from a drawing. It requires a license key and the swdocumentmgr interop. — [DM help 2025 C#](https://help.solidworks.com/2025/english/api/swdocmgrapi/Get_Table_Cell_Text_Example_CSharp.htm), [DM help 2013 VB.NET](https://help.solidworks.com/2013/English/api/swdocmgrapi/Get_Table_Cell_Text_Example_VBNET.htm). The page content was not fetched (DNS failure), so the method names come from search summaries.
- License pitfall: `SwDMClassFactory.GetApplication` with an invalid or wrongly assigned key fails with "ActiveX can't create the object". — [SOLIDWORKS forum thread 18091](https://forum.solidworks.com/thread/18091)
- DM property values are those "from when the file was last saved in SOLIDWORKS", so values are not re-evaluated. — [DM help](https://help.solidworks.com/2015/English/api/swdocmgrapi/SOLIDWORKS.Interop.swdocumentmgr~SOLIDWORKS.Interop.swdocumentmgr.ISwDMDocument~GetCustomProperty.html)
- Other GitHub DM examples (all small, 0–1★):
  - [robkite/Obtain-SolidWorks-Custom-Properties](https://github.com/robkite/Obtain-SolidWorks-Custom-Properties): C#, compares full API vs DM.
  - [patrickgamp/TraverseSolidWorksComponentsUsingDocumentManagerAPI](https://github.com/patrickgamp/TraverseSolidWorksComponentsUsingDocumentManagerAPI): C#, 2021, assembly traversal.
  - [BlueByteSystemsInc/DocumentManagerExtensions](https://github.com/BlueByteSystemsInc/DocumentManagerExtensions) (2022).
  - [BlueByteSystemsInc/DrawingPropertyChangePropertyTypeBug_DocumentManager](https://github.com/BlueByteSystemsInc/DrawingPropertyChangePropertyTypeBug_DocumentManager): a documented DM bug where deleting and re-adding a property with a different type "Works fine for parts but not drawings".
  - [stefansterk/SWhereUsed](https://github.com/stefansterk/SWhereUsed): Python, 2026, where-used index via DM.
- xCAD.NET supports DM as an alternative backend with the same code as SOLIDWORKS for open, traverse and close. — [xcad README](https://github.com/xarial/xcad)
- **openswx (no SW, no DM key):** MIT, C++20, 31★, 28 commits, last push 2026-10-09. It reads document type and version, global and per-configuration custom properties, preview PNGs, mass properties, cut lists, component references, sheet names and view references, and builds a recursive BOM as JSON. It supports SW2015+ (chunk format) and ≤2014 (OLE2). It does not document extraction of annotations or dimensions. It needs Windows-to-host path mapping for references, and it asks for sample files to improve compatibility. — [openswx](https://github.com/schwitters/openswx)

### Inferences
- A hybrid design is likely best. DM (or openswx) would handle fast, license-light bulk reads: title-block custom properties, references, BOM table cell text, and revision-table cell text (if the DM table API exposes it). The full SOLIDWORKS COM API would be used only for things DM cannot see: dimensions, tolerances, GD&T, dangling status and override flags.
- Keep the DM key out of source code (env var / config file, as in sw_drawing_studio). The key is tied to the company's subscription.

### Gaps
- The exact DM license-key request process in 2026 (portal name, eligibility) was not verified. The forum/vendor snippet only says it is free and must be requested.
- Whether DM's table API distinguishes BOM vs revision tables, and whether it returns displayed or raw text, is unverified.
- openswx's correctness on real production files and on SW2024–2026 files is untested. It is 6 months old.
