# Category-Specific Drawing Review Checklists (ASSY / M / F / S / T / D / P / Q / E / X)

Research date: 2026-10-28. Context: Korean semiconductor equipment maker (FOUP/EFEM: air curtain, N2 purge stocker, dehumidifier), SolidWorks-based.

Method note: Only web search was available for most sources. Direct page fetches to www.thefabricator.com, www.protolabs.com and help.solidworks.com failed because those hosts are outside this session's allowed network domains. Findings below are therefore drawn from search-result summaries of those pages, plus other reachable pages. Treat numeric rules as "vendor guidance, confirm with your own supplier" unless a standard is named. Items marked **[UNVERIFIED]** are from training knowledge or weak sources and were not confirmed in this session.

---

## Q1. Which published checklists/guides give concrete review items, per DIVISION category?

### Takeaway
No single published checklist covers all categories. Concrete, numeric items exist mainly for sheet metal (S: fabricator DFM guides), purchased motion parts (D: maker ordering-code structures), UHP piping (T: Swagelok/vendor specs, SEMI F20-referenced fittings), and weldments (F: AWS/ISO symbol conventions plus CAD weldment docs). Machined parts (M) and fasteners (P) are covered by general drafting/DFM sources and rules of thumb. For ASSY, Q, E and X, the sources found were not category-specific, so those checklists have to be built mostly from internal practice.

### Cited Findings

#### S 판금 (sheet metal)
- Minimum inside bend radius depends on material. Typical guidance is about 1T for mild steel, 1.5T to 2T for annealed stainless, and 1T to 1.5T for 5052-H32 aluminum. Stiffer alloys such as 6061 may need 4 to 8 times the thickness — [Yijin Solution bending guide](https://yijinsolution.com/sheet-metal-guides/sheet-metal-bending-guide/); [Fabcon DFM guide](https://fabcon.com/articles/?p=534)
- Korean design tip: if the inside R is smaller than the thickness, the outer surface cracks. Suggested minimums are SPCC R ≥ 1t and SUS304 R ≥ 1.5t, with a larger R needed for hardened material or bends perpendicular to the rolling direction (grain direction) — [whole-spec sheet metal design rules (KR)](https://whole-spec.pages.dev/design-tips/sheet-metal-design-rules) (blog-level source)
- Hole-to-bend distance: 2.5T + R for standard holes (some manuals allow 2T + R for holes under 1 in.), and 4T + R for long slots and cutouts — [mfgrobots enclosure guide](https://www.mfgrobots.com/Article/mfg/it/54223.html). Fabcon uses a 4T rule for holes, slots and hardware relative to bend lines — [Fabcon](https://fabcon.com/articles/?p=127). The Korean blog gives a minimum of 2t — [whole-spec](https://whole-spec.pages.dev/design-tips/sheet-metal-design-rules). **The sources conflict (2t, 2T+R, 2.5T+R, 4T), so the company must set its own value with its 판금 vendor.**
- Minimum flange: at least 4T according to one source, or 3T + R according to Fabcon. Another source puts it at about 77% of the V-die opening for air bending, which depends on tooling — [Fabcon](https://fabcon.com/articles/?p=534); [Yijin](https://yijinsolution.com/sheet-metal-guides/sheet-metal-bending-guide/)
- Protolabs' toolkit gives flange length guidelines as multiples of thickness plus bend radius, varying by material and thickness, e.g. 3.0 × T + R for 0.024 to 0.050 in. aluminum — [Protolabs Design for Sheet Metal Toolkit](https://www.protolabs.com/resources/design-for-sheet-metal-fabrication-toolkit/) (via search summary)
- Bend relief: at least 1T wide and T + R deep where bend lines meet — [Fabcon](https://fabcon.com/articles/?p=534); [robu.in sheet metal basics](https://robu.in/sheet-metal-design-basics-in-2021-complete-list-of-things-that-you-must-know/)
- Hole edge distance (Protolabs): at least 1/16 in. from the edge for material 0.036 in. thick or less, and at least 1/8 in. for thicker material — [Protolabs 7 Ways to Improve Sheet Metal Parts](https://www.protolabs.com/resources/design-tips/8-ways-to-improve-sheet-metal-parts/) (via search summary)
- Hardware (PEM) inserts: Protolabs says to space holes for inserts according to the hardware maker's specs. SendCutSend lists clearances to edges, other inserts and bends in each insert's spec sheet; for example, a ¼-20 nut in 0.063 in. 5052 aluminum needs 0.340 in. from center to edge — [Protolabs tolerances one-pager](https://protolabs.com/media/roognect/sheet-metal-tolerances-one-pager-2.pdf); [SendCutSend PEM guide](https://sendcutsend.com/blog/pem-hardware-inserts-in-sheet-metal/). One rule of thumb keeps PEM at least 4T from the bend tangent — [Fabcon](https://fabcon.com/articles/?p=127)
- Flat pattern (전개도): flat-pattern accuracy depends on entering the correct bend allowance (연신율/K-factor). Otherwise rework is needed — [Daum SolidWorks cafe post](https://m.cafe.daum.net/solidworks2004/ikP/498) (low reliability). K-factor definition: BA = π(R + K·T)·A/180 — [SOLIDWORKS help: K-Factor](https://help.solidworks.com/2022/English/SolidWorks/sldworks/c_K_Factor.htm)
- Nesting: sheets are typically cut from 1219×2438 mm (4×8 ft) stock, so the flat size should be checked against sheet size for yield — [whole-spec](https://whole-spec.pages.dev/design-tips/sheet-metal-design-rules)

#### F 프레임 (weldment / 제관, aluminum profile frame)
- Weld symbol minimum content is the arrow, reference line and weld symbol. A symbol below the reference line means arrow side (AWS convention). The first operation goes on the reference line closest to the arrow. A general note can cover predominant welds, e.g. "Unless otherwise indicated, all fillet welds are 5/16 in." — [Mastering Welding Drawings (AWS-style summary)](https://db-01-us-west-2.aws.onerain.com/welding-drawings) (secondary source)
- AWS A2.4 (US) and ISO 2553 (international) standardize the symbology. ISO 2553 recognizes two systems for arrow side and other side, so the drawing must state which standard applies — [schematex welding symbols (KR)](https://schematex.js.org/ko/docs/welding); [ISO 2553 (BSI)](https://knowledge.bsigroup.com/products/scheme-of-symbols-for-welding). **KS B 0052 text was not found in this session. [UNVERIFIED]: KS B 0052 is understood to be harmonized with ISO 2553. Confirm against the KS document.**
- ISO approach: the drawing shows design essentials (weld size, quality level), and production chooses the joint preparation and process — [schematex summary of ISO 2553](https://schematex.js.org/ko/docs/welding)
- Weld before or after machining: CAD weldment models have separate preparation, weld and machining states. Views of the machining state show post-weld chamfers, cuts and holes. Manually placed drawing weld symbols are copies and do not update with the model, so model-linked weld symbols are preferred — [Autodesk Inventor help: weld annotations](https://help.autodesk.com/cloudhelp/2026/ENU/Inventor-Help/files/GUID-E3FB266D-8FE5-48B2-8761-137A4B323C25.htm); [Inventor weldment states](https://help.autodesk.com/cloudhelp/2014/ENU/Inventor/files/GUID-4135DCE0-0820-4573-AB83-339B7AB201D3.htm)
- No authoritative source was found for distortion allowance or machining stock (가공 여유) on 제관 parts (see Gaps).
- Cut list (SolidWorks): structural-member cut-list items carry LENGTH, ANGLE1, ANGLE2, DESCRIPTION, MATERIAL, QUANTITY and TOTAL LENGTH. LENGTH and angles cannot be edited by hand. Weight is computed only from the first body in each cut-list folder, so each folder must hold identical bodies. SW 2025 SP3 added BOM grouping by PROFILE, LENGTH, ANGLE1/2 and DESCRIPTION — [SOLIDWORKS help: Custom Properties in Weldment Cut Lists](https://help.solidworks.com/2023/english/SolidWorks/sldworks/t_custom_props_weldment_cut_lists_2.htm); [SW 2025 What's New](https://help.solidworks.com/2025/English/WhatsNew/c_wn2025_weldments_group_weld_prof_quant.htm); [CATI weldments part 4](https://www.cati.com/?p=119989)
- Aluminum profile callouts:
  - Length is the total length, measured vertex to vertex when angle cuts exist.
  - Vendor examples give a cut tolerance of ±0.2 mm and an angle tolerance of ±0.25° (angle range 30° to 89°).
  - End tapping depends on the profile series: M8 × 16 deep for the 30-series and M12 × 24 deep for the 45-series. Some angle-cut services do not offer tapping.
  - Sources: [Ooznest 30x30 cut-to-size](https://resources.ooznest.co.uk/aluminium-extrusion-system/aluminium-extrusion/30-series-t-slot-aluminium-extrusion/30x30mm-t-slot-aluminium-extrusion-profile-silver-cut-to-size-var-aep-t-3030-s-cts); [Ooznest 45x90](https://resources.ooznest.co.uk/aluminium-extrusion-system/aluminium-extrusion/45-series-t-slot-aluminium-extrusion/45x90mm-t-slot-aluminium-extrusion-profile-silver-cut-to-size-var-aep-t-4590-s-cts)
  - Faztek quotes ±0.010 in. on one page and ±0.015 in. on another (inconsistent) — [Faztek cutting](https://datasheets.globalspec.com/ds/3060/Faztek)
  - MISUMI: cut surfaces and altered areas are not surface treated, and tapped-hole alterations are specified against marked positions on the outline drawing — [MISUMI](https://uk.misumi-ec.com/vona2/detail/110302687940)

#### M 일반가공품 (machined parts)
- Thread callout format example: M10×1.5-6H means metric coarse thread, 10 mm diameter, 1.5 mm pitch, 6H internal tolerance class. Repeated features use count notation ("4X"). A 45° countersink or chamfer at thread start reduces burrs. For blind tapped holes, specify minimum full-thread depth rather than full depth — [Fictiv 9 tips](https://www.fictiv.com/articles/9-tips-for-better-engineering-drawings-that-save-time-money); [dewin DFM checklist](https://dewinvietnam.pages.dev/blog/dfm-checklist-metal-parts-vietnam)
- Edge break: a general "deburr all edges" note is acceptable — [UF EML2322L design & manufacturing tips](https://web.mae.ufl.edu/designlab/Lab%20Assignments/EML2322L%20Design%20&%20Manufacturing%20Tips.pdf). Edge-break callouts are usually defined in the title block — [Chemring supplier inspection guide](https://www.chemring.com/~/media/Files/C/Chemring-V3/pdfs/general-inspection-measurement-guidelines-for-ced-suppliers.pdf). If later operations such as sanding or coating affect an edge break, the callout should allow for it — [Zimmer Group requirements catalog for drawing parts](https://www.zimmer-group.com/fileadmin/daten/service/download-center/lieferanten/Requirements_catalog_for_drawing_parts__0538_EN.pdf)
- Surface roughness works like a tolerance: without it there is no acceptance criterion, and over-specifying adds cost — [UF tips](https://web.mae.ufl.edu/designlab/Lab%20Assignments/EML2322L%20Design%20&%20Manufacturing%20Tips.pdf). Print interpretation per ASME Y14.5-2018 — [Chemring](https://www.chemring.com/~/media/Files/C/Chemring-V3/pdfs/general-inspection-measurement-guidelines-for-ced-suppliers.pdf)
- Tolerance review: for each callout, ask what breaks functionally if it were looser, and relax non-functional ones. A vendor anecdote reports about 18% unit-price reduction from doing this (illustrative only) — [dewin DFM checklist](https://dewinvietnam.pages.dev/blog/dfm-checklist-metal-parts-vietnam)
- Dimension each feature once, in the view showing its true shape — [Fictiv](https://www.fictiv.com/articles/9-tips-for-better-engineering-drawings-that-save-time-money)
- Zimmer Group publishes a full "Requirements catalog for drawing parts" for suppliers. It is a good model for a company-wide supplier drawing requirement document — [Zimmer 0538 EN](https://www.zimmer-group.com/fileadmin/daten/service/download-center/lieferanten/Requirements_catalog_for_drawing_parts__0538_EN.pdf)

#### T 배관 (piping/tubing: gas, vacuum, N2)
- Tube wall thickness: choose it from Swagelok's allowable working-pressure table for each tube OD. For gas service, choose a wall outside the shaded area of the table. Do not use walls outside the listed ranges, because fittings are tested only at the listed min/max walls — [Swagelok tubing data sheet MS-01-107](https://www.swagelok.com.cn/downloads/webcatalogs/en/MS-01-107.pdf); [Swagelok tubing data](https://www.swagelok.com/-/media/distributor-media/c-g/calgary/services/pdf/tubing-data-sheet.ashx)
- Required straight tube length from a bend to the fitting is tabulated per OD, so the tube can bottom in the fitting. Tube ends must be deburred — [Swagelok MS-01-107](https://www.swagelok.com.cn/downloads/webcatalogs/en/MS-01-107.pdf)
- Large-size fittings (1-1/4 in. to 2 in.; 28 to 50 mm) require a hydraulic swaging unit — [Swagelok tubing data](https://www.swagelok.com/-/media/distributor-media/c-g/calgary/services/pdf/tubing-data-sheet.ashx)
- UHP EP 316L tubing:
  - Typical inner-surface spec is Ra ≤ 0.25 µm (10 µin), with 0.13 µm (5 µin) on request. Ra is undefined on circumferential welds and cold-worked fitting areas.
  - Material is 316L (UNS S31603 / 1.4404 / 1.4435) to ASTM A269/A270/A632, prepared for orbital welding.
  - Source: [Dockweiler ultron spec](https://citech-c3.duckdns.org/file/DW_Specifications_ultron_EN.pdf) (third-party hosted copy)
- Parker UHP weld fittings are described as meeting SEMI F20 cleanliness and corrosion requirements, with standard internal Ra 10 µin and PE finish 5 µin — [Parker UHP weld fittings](https://ph.parker.com/us/es/uhp-weld-fitttings)
- An older UHP gas spec (Sandia, 1994) calls for seamless 316L per ASTM A269/A632, sulfur control, and hydrogen annealing before EP. It is background only because of its age — [Sandia 15481 UHP Gas Distribution](https://webarchive.library.unt.edu/web/20130224114525/http:/www.sandia.gov/engstds/ConstSpecs/Div_15/15481_Ultra_High_Purity_Gas_Distribution_Sys_1194.pdf)
- Helium leak rate: fab gas systems commonly use limit leak rates below 1×10⁻⁹ mbar·L/s — [Inficon UL1000 Fab brochure](https://www.ajvs.com/library/Inficon_UL_1000_Fab_Brochure.pdf); [Pfeiffer He leak detection in semiconductor fabs](https://www.pfeiffer-vacuum.com/api/empolis/resource/environment/project1_p/documents/pfeifferSharepointProd/9937-helium-leak-detection-in-semiconductor-factories-pi0418pen.pdf). 1.0×10⁻⁹ atm·cc/s ≈ 1.013×10⁻⁹ mbar·L/s — [MPF leak detection worksheet](https://mpfpi.com/wp-content/uploads/2020/06/Leak-Detection-Science-WS003.pdf)
- One trade blog says threaded connections are not recommended for UHP. Its leak-rate table (orbital weld about 10⁻¹², compression 10⁻⁹ to 10⁻¹⁰) is **[UNVERIFIED]** — [industrialmonitordirect](https://industrialmonitordirect.com/blogs/knowledgebase/ultra-high-purity-gas-pipeline-surface-roughness-connection-specs)
- SEMI F20, F81 and related standards were not retrievable. Normative Ra, weld-bead and cleanliness acceptance criteria could not be confirmed.

#### D 운동류 (purchased motion components)
- SMC order codes are field-structured: series, mounting, bore, stroke, cushion suffix, auto-switch model, and switch-count field. A "nil" entry has meaning (e.g. no switch, standard air cushion). Made-to-order suffixes (e.g. -XB6 heat-resistant, -XC6 stainless) change the part. Some part numbers bundle brackets and switches — [SMC CP96 catalog](https://automationdistribution.com/content/files/pdf/cp96.pdf); [SMC configurator pages](https://www.smcpneumatics.com/53787.htm)
- SMC's configurator requires the number with dashes and no spaces. Incompatible options appear grey, conflicts red, and not-recommended options amber — [smcpneumatics.com](https://www.smcpneumatics.com/53787.htm). Implication for review: verify the full code in the maker's configurator, not just the series.

#### P 고정류 (fasteners, fittings, standard parts)
- Thread engagement rule of thumb: about 1×D in steel and 2×D in aluminum, so the screw breaks before the threads strip — [Nord-Lock: thread engagement in a tapped hole](https://www.nord-lock.com/insights/bolting-tips/2015/the-experts-thread-engagement-in-a-tapped-hole/). Tameson gives 1.0 to 1.5D for steel and 2.0 to 2.5D for aluminum — [Tameson thread engagement chart](https://tameson.com/thread-engagement-chart.html). Softer materials need calculation rather than the rule of thumb — [Nord-Lock](https://www.nord-lock.com/insights/bolting-tips/2015/the-experts-thread-engagement-in-a-tapped-hole/)
- **[UNVERIFIED]**: ISO 4762 corresponds to KS B 1003 for hex socket head cap screws. Common grades are 12.9 alloy steel and A2-70/A4-70 stainless. Cleanroom/EFEM practice often avoids zinc-plated or cadmium coatings. These were not confirmed in this session.

#### ASSY / Q 셋업파트 / E 기타 / X
- No external category-specific checklists were found for assembly drawings, setup/installation parts, or miscellaneous items in this session. ASME Y14.24-type drawing types (assembly, installation drawing, and vendor item drawing) are the closest framework — [CR4 discussion of ASME Y14.24 drawing types](https://cr4.globalspec.com/thread/124228/ASME-Y14-24-Drawing-Types) (forum level)
- SolidWorks Design Checker assembly checks cover component materials, mate errors/warnings and external references — [CATI Design Checker](https://www.cati.com/?p=132014)

### Inferences (proposed checklist content per DIVISION; derived from findings plus general practice)

**ASSY**
- BOM completeness: every component has an item number, company part number or vendor code, quantity and DIVISION code.
- Balloons match BOM items.
- No interference: run Interference Detection with zero unexplained interferences.
- Assembly notes cover torque values, threadlocker or adhesive (cleanroom/outgassing-compatible), orientation and order of assembly, and maintenance/service access for filters, fans and sensors in air curtain or dehumidifier units.
- Mates are free of errors, and there are no external references to unreleased files.
- Typical defects: a component present in the model but missing from the BOM, toolbox fasteners suppressed or excluded from the BOM, wrong quantity of fasteners, outdated subassembly revision.

**M 일반가공품**
- Tolerances are achievable and have a functional basis.
- Datum scheme is defined (A/B/C) and consistent with the mating parts.
- Thread callouts in ISO 965 / KS B 0211-style format with tolerance class and depth.
- Hole callouts come from Hole Wizard (not hand-typed).
- Edge break or chamfer general note is present.
- Surface roughness is on functional surfaces only, with a general roughness value given.
- Material and heat treatment are specified.
- Surface treatment is specified (anodize type/color, electroless Ni), with allowance for coating thickness on fits.
- Typical defects: over-tight general tolerance, missing thread depth, roughness on every surface, no coating allowance, tap drill vs thread mismatch.

**F 프레임**
- Weld symbols follow the declared standard (ISO 2553 / KS B 0052 / AWS A2.4) and are consistent on arrow side vs other side.
- General weld note (default fillet size) is present.
- Weld-then-machine sequence is stated on the drawing, with machined surfaces identified on the machining-state view and machining stock allowed.
- Distortion control: stress relief, flatness after welding.
- Cut list table matches the model (profile, length, angle, quantity).
- Aluminum profile: series and maker code, cut length vertex-to-vertex, cut/angle tolerance, end tap size/depth, and post-cut surface treatment note.
- Typical defects: weld symbols manually drawn and stale, weld-before-machining not stated, cut-list folders mixing non-identical bodies (wrong weight/quantity), unspecified profile maker, tapping not available on angle cut.

**S 판금**
- Thickness and material (SPCC/SECC/SUS304/AL5052) match the gauge table.
- Inside R ≥ minimum for the material.
- K-factor or bend table matches the vendor's.
- Flat pattern is present (dimensioned or DXF supplied).
- Minimum flange length is respected.
- Hole/slot-to-bend distance is respected.
- Bend relief is present.
- Grain direction is noted where bending hard materials.
- PEM/hardware callout gives maker part number and side, with edge/bend clearance per maker spec.
- Finish (powder coat color code, plating) is specified, and masking for ground/contacts is noted.
- Typical defects: flange too short for the die, holes distorted near bends, missing relief causing tearing, PEM too close to a bend, wrong K-factor causing flat-size error.

**T 배관**
- Tube OD and wall per a pressure-rating table.
- Material 316L, EP or BA grade with Ra value.
- Fitting type and maker code (Swagelok tube fitting vs VCR face seal vs weld fitting).
- Straight length before fittings and bend radius.
- Orbital weld spec, cleanliness class (cleaned for UHP, double-bagged), and He leak test criterion (e.g. ≤1×10⁻⁹ mbar·L/s) noted on the drawing.
- Gas/line identification labels (N2, CDA, vacuum, exhaust).
- Typical defects: wall thickness outside the fitting's tested range, missing leak spec, threaded joints in a UHP line, mixing BA and EP.
- For N2 purge stockers specifically, the purge line is likely clean-dry-gas grade rather than full UHP specialty gas. Grade selection (EP vs BA vs standard) should be a design decision recorded on the drawing. **[inference]**

**D 운동류**
- Exact full maker model code is present, including stroke, mounting, cushion, switch model, switch quantity and MTO suffix, and has been validated in the maker configurator.
- Load/moment rating checked against the application (LM guide, ball screw life).
- Sensor/switch options and cable length are specified.
- Lead time is recorded.
- Alternate/equivalent maker approval is recorded.
- The maker's catalog 2D/3D is attached as reference, not as a company manufacturing drawing.
- Typical defects: series-only code, stroke in the model not matching the code, switch omitted from the code, MTO suffix lost.

**P 고정류**
- Standard number (ISO 4762 / KS B 1003, ISO 4032 nuts, etc.) is stated.
- Size × length is checked against grip and engagement (1D steel / 2D aluminum).
- Property class / stainless grade is specified.
- Coating suits the cleanroom.
- Washer/locking method is specified.
- Fittings carry maker code.
- Typical defects: screw bottoming in a blind hole, length too short for 2D in aluminum, plated screws in cleanroom areas, missing nut/washer in the BOM.

**Q 셋업파트 / E 기타 / X**
- Recommend a light review: identification (part number/vendor code), quantity, whether a drawing is required, and installation/field-fit notes (shims, leveling feet, anchor, cable tray).
- X likely means "no review / reference only" and should be defined internally. **[inference; meaning of E/X not externally defined]**

### Gaps
- KS B 0052 (용접 기호), KS B 1003 and ISO 965 texts were not accessed. Correspondence to ISO is **[UNVERIFIED]**.
- SEMI F20, F81, F19, F73 and similar normative criteria (Ra, weld bead width/discoloration acceptance, particle/moisture/O2 tests) were not retrievable. Only vendor statements were found.
- No authoritative source was found on weld distortion allowance or machining stock for 제관 frames.
- No Korean 제관 or 판금 review checklist document was found; only blog-level Korean sources.
- The Protolabs, SendCutSend and TheFabricator pages could not be fetched (network allowlist), so exact tables were not verified.

---

## Q2. How do companies handle purchased-part "drawings" (vendor code in BOM, no drawing) in review?

### Takeaway
The formal framework is ASME Y14.24. A **Vendor Item Drawing (VID / vendor item control drawing)** documents an off-the-shelf item used without alteration and lists approved vendor(s) and their part numbers. A **Source Control Drawing (SCD)** is used for critical items that need source qualification. For commodity items, many companies skip drawings and use an approved vendor list (AVL) with a manufacturer part number in the BOM.

### Cited Findings
- Vendor Item Control Drawings specify purchased items approved for the design and used without alteration, selection or source qualification. Source Control Drawings are for critical applications where source qualification testing is required — [DoD DI-SESS-81000E excerpt (Scribd)](https://www.scribd.com/document/429161122/DI-SESS-81000E)
- A VID lists the vendor name, address, CAGE code and part number. Alternate vendors can be added to the VID later without revising every parts list that references it — [Eng-Tips: spec control drawings](https://eng-tips.com/threads/spec-control-drawings.160568); [Eng-Tips: source control drawings](https://eng-tips.com/threads/source-control-drawings.50485) (forum)
- Commodity items (e.g. hose clamps) usually get an approved-vendor list rather than an SCD — [Eng-Tips](https://eng-tips.com/threads/source-control-drawings.50485) (forum)
- One forum post says ASME Y14.100 invokes Y14.24 for VIDs. This is unconfirmed — [CR4](https://cr4.globalspec.com/thread/124228/ASME-Y14-24-Drawing-Types)
- Argonne APS has a CAD procedure on BOMs and purchased parts, which shows a lab handling purchased parts as BOM entries — [APS ProE BOMs & Purchased Parts](https://APS.ANL.gov/files/APS-Uploads/DD/Procedurals/ProE_BOMs-n-Purchsd_Parts.pdf) (not fetched; content unverified)

### Inferences
- For D/P/T-fittings, review the **BOM line**, not a drawing. Required fields: maker, full order code, description, quantity, AVL/alternate status, and catalog datasheet link/attachment.
- A catalog drawing attached as if it were a manufacturing drawing is a defect: it should be marked "reference/vendor item". A company VID-style sheet is warranted only for modified purchased items (e.g. a cylinder with a custom rod end, or an MISUMI part with alterations), which should then move to M or have a VID.
- Review gate for purchased items:
  1. Code validated in the maker configurator.
  2. 3D model downloaded from the maker matches the code.
  3. Lead time and EOL status checked.
  4. Alternate approved.

### Gaps
- No Korean-industry source was found describing 구매품 도면 검수 practice. The ASME Y14.24 standard text itself was not accessed.

---

## Q3. Which items can be automatically checked from SolidWorks data vs require humans?

### Takeaway
Many attribute- and parameter-level checks can be automated:
- Custom properties, material assigned, drafting standard and fonts, overridden dimensions, mate errors and external references (Design Checker).
- Sheet metal radius, thickness and K-factor (API `ISheetMetalFeatureData`).
- Weldment cut-list properties (LENGTH, ANGLE1/2, DESCRIPTION, QUANTITY).
- Toolbox fastener part numbers and descriptions.
- Hole Wizard hole standard and size.

Functional judgement needs a human: tolerance adequacy, datum logic, weld adequacy and sequence, maintenance access, cleanliness grade choice, purchased-part fitness.

### Cited Findings
- SOLIDWORKS Design Checker checks dimensioning standard, custom properties, units, fonts, overridden dimensions, arrow styles, sheet formats, layers, title blocks, part materials, assembly mate errors, external references, feature errors and fully defined sketches. Requirements are saved as a .swstd file and run with "Check Active Document". Built-in standards include ANSI, ISO, DIN, JIS, BSI, GOST and GB — [SOLIDWORKS Design Checker overview](https://help.solidworks.com/2025/English/swconntutorial/Design_Checker_Overview.htm); [SW help: Document Checks](https://help.solidworks.com/2024/English/SolidWorks/SolidWorks_Design_Checker/r_document_checks.htm); [CATI Design Checker](https://www.cati.com/?p=132014)
- Design Checker has a "Standard Hole Sizes" check. One forum user reports that it flagged every Hole Wizard hole as custom, so behaviour may depend on the hole standard configuration (unconfirmed) — [3DSwym forum: hole wizard and bend surfaces](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/hole-wizard-and-bend-surfaces_H-G_7xPjRqiiwtTd7j333A)
- DFMXpress has sheet metal rules, including a "Bend Radii" rule. Its thresholds were not visible in this session — [SW DFMXpress Bend Radii](https://help.solidworks.com/2025/English/SolidWorks/dfmxpress/c_rules_sheet_metal_bend_radius.htm)
- API: `ISheetMetalFeatureData.BendRadius` reads and writes the bend radius. The custom bend allowance object can be set to the K-factor type. `IPartDoc.InsertBends2` takes radius, bend table, K-factor and bend allowance — [SW API BendRadius](https://help.solidworks.com/2017/English/api/sldworksapi/SolidWorks.Interop.sldworks~SolidWorks.Interop.sldworks.ISheetMetalFeatureData~BendRadius.html); [SW API InsertBends2](https://help.solidworks.com/2021/English/api/sldworksapi/SOLIDWORKS.Interop.sldworks~SOLIDWORKS.Interop.sldworks.IPartDoc~InsertBends2.html)
- Weldment cut-list properties (LENGTH, ANGLE1, ANGLE2, DESCRIPTION, MATERIAL, QUANTITY, TOTAL LENGTH) are generated automatically and are not hand-editable, so they can be read and compared programmatically. Weight is computed only from the first body per folder — [SW help: cut list properties](https://help.solidworks.com/2023/english/SolidWorks/sldworks/t_custom_props_weldment_cut_lists_2.htm)
- Toolbox: each size/configuration can carry its own part number and description, and custom properties can be added as configuration-specific (Configure Component PropertyManager). Forum users report that Toolbox part numbers can be cryptic and BOM defaults can be overwritten, and many save fasteners as discrete stock parts instead — [SW help: Configure Component for Toolbox](https://help.solidworks.com/2024/English/Solidworks/sldworks/HIDD_DVE_TOOLBOXSIZER.htm); [CATI Customizing Toolbox](https://www.cati.com/?p=118015); [3DSwym: fastener part numbers in BOMs](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/fastener-part-numbers-in-bom-s_0ShVWa3nQlyE5m5cNVavHQ)
- **[UNVERIFIED, training knowledge]**: The SolidWorks API exposes Hole Wizard parameters via `IWizardHoleFeatureData2` (standard, fastener type, size, depth, thread class, tap drill). This was not confirmed in this session because the Hole Wizard search returned no results.

### Inferences (automation split)

| Check | Auto (SW data/API/Design Checker) | Human |
|---|---|---|
| Custom properties filled (part no., DIVISION, material, finish, maker, model code) | Auto | — |
| Material assigned and matches the property text | Auto | — |
| Title block, drafting standard, fonts, overridden dims | Auto (Design Checker) | — |
| Sheet metal T vs gauge table, R ≥ min(material), K-factor = company value | Auto (API) | Exceptions |
| Hole-to-bend / PEM-to-bend distance | Partial (custom geometry script needed; no built-in check found) | Yes |
| Flat pattern exists and DXF exported | Auto | — |
| Weldment cut list consistent (identical bodies per folder, profile description present) | Auto | — |
| Weld symbols present and correct | Weak (count only) | Yes |
| Weld/machining sequence, distortion | — | Yes |
| Hole Wizard used vs plain cut; standard holes only | Auto (feature type plus Design Checker hole size) | — |
| Thread engagement / screw length vs tapped depth | Partial (needs mate/geometry script) | Yes |
| Toolbox fastener has company part number and description | Auto | — |
| BOM has every component with a DIVISION code; quantity check | Auto | — |
| Interference | Auto (Interference Detection) | Judge results |
| Purchased part code validity (D) | Partial (format/regex per maker) | Yes (configurator) |
| Tolerance and datum adequacy, roughness appropriateness | — | Yes |
| Piping spec (Ra, leak, cleanliness) noted | Auto (note/property presence only) | Yes (correctness) |
| Maintenance access, assembly sequence | — | Yes |

### Gaps
- Exact DFMXpress thresholds, the full Design Checker check list, and Hole Wizard API details could not be fetched because help.solidworks.com is outside the session network allowlist.
- No source was found on SolidWorks PDM-integrated review workflows in Korean companies.
