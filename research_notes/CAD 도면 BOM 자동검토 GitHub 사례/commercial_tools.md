# Commercial and Built-in Drawing/BOM Checking Tools (SOLIDWORKS and other CAD): Baseline for a Custom Python Review Tool

Research date: 2026-10-20. Method: web search only. WebFetch and curl to help.solidworks.com, javelin-tech.com and cadbooster.com failed (DNS/proxy 403), so many findings come from search-result snippets of those pages. They were not read in full. Version years are noted wherever the source shows one. Items marked **[UNVERIFIED]** could not be confirmed from a primary source.

## Q1. What does SOLIDWORKS Design Checker check for drawings? Can it run in batch or through the API? Which license tier?

### Takeaway
Design Checker is a rule-based add-in included with SOLIDWORKS Professional and Premium, not Standard. It checks a document against a `.swstd` standards file. For drawings that means document settings (dimensioning standard, fonts, units), drawing-level items (standard template/sheet format, layers, title block, BOM balloons, dangling dimensions/annotations, overlapping dimensions), and some custom-property checks. It runs in batch through the SOLIDWORKS Task Scheduler "Design Checker" task and through a COM API (`ISWDesignCheck.RunDesignCheck4/5`). Its output is HTML/XML-style reports, not an Excel review sheet. It does not cross-check against ERP, and it does not prove a drawing is fully dimensioned.

### Cited Findings
**License tier**
- Design Checker is an add-in included with SOLIDWORKS Professional and SOLIDWORKS Premium — [Hawk Ridge Systems support](https://support.hawkridgesys.com/hc/en-us/articles/26745157214989-How-to-Use-the-Design-Checker-Task-in-SOLIDWORKS-Task-Scheduler); also stated on [SOLIDWORKS Help 2014 "Welcome to Design Checker"](https://help.solidworks.com/2014/English/SolidWorks/solidworks_design_checker/c_welcome_design_checker.htm) ("available in SolidWorks Professional and SolidWorks Premium only").
- A reseller listing (CAD Micro, now GoEngineer) for 3DEXPERIENCE SOLIDWORKS Professional (term license) lists "design and drawing checking" among Professional features — [CAD Micro shop](https://shop.cadmicro.com/products/3dexperience-solidworks-professional-term-license-1-year).

**Modules and how rules are configured**
- Design Checker has four modules: Check Active Document, Check Against Existing File, Build Checks (Design Check Builder), and the Learn Checks Wizard. Standards are saved as `*.swstd` files — [SOLIDWORKS Help 2021 Welcome](https://help.solidworks.com/2021/english/SolidWorks/solidworks_design_checker/c_welcome_design_checker.htm); [Javelin blog 2022, Learn Checks Wizard](https://www.javelin-tech.com/blog/2022/11/using-the-learn-checks-wizard-in-solidworks-design-checker/); [CATI blog](https://www.cati.com/blog/solidworksdesign-checker/).
- The Learn Checks Wizard builds a `.swstd` from the settings of an existing "golden" document. Learned document-level font checks include font name, style, strikeout, underline, and units. Dimension checks are built by selecting specific dimensions, with parameters such as arrow style and position — [Javelin blog 2022](https://www.javelin-tech.com/blog/2022/11/using-the-learn-checks-wizard-in-solidworks-design-checker/).
- Build Checks groups checks into categories including document checks (dimensioning standard, fonts, units, custom properties), and also annotation, dimension, drawing, feature, part/assembly, and sketch checks — [SOLIDWORKS Help 2019 Build Checks](https://help.solidworks.com/2019/English/SolidWorks/solidworks_design_checker/c_build_checks.htm); [SOLIDWORKS Help 2021 Build Checks](https://help.solidworks.com/2021/english/SolidWorks/solidworks_design_checker/c_build_checks.htm) (seen as snippets only).
- Javelin's product page lists title blocks and custom properties among the elements checked for accuracy and completeness — [Javelin Design Checker page](https://www.javelin-tech.com/3d/solidworks-design-checker/).

**Drawing-specific checks that exist**
- Drawing document checks include standard template check, layer check, and title block check — [CATI blog](https://www.cati.com/blog/solidworksdesign-checker/); [Javelin blog 2022](https://www.javelin-tech.com/blog/2022/11/using-the-learn-checks-wizard-in-solidworks-design-checker/).
- "Dangling Dimension/Annotation" is a drawing check that flags dimensions/annotations whose references are lost — [SOLIDWORKS Help 2020, Dangling Dimension/Annotation](https://help.solidworks.com/2020/English/SolidWorks/solidworks_design_checker/r_dangling_dimension_annotation.htm); how-to in [Javelin blog 2019](https://www.javelin-tech.com/blog/2019/04/dangling-dimensions-solidworks-design-checker/) (which notes UAC set to "Never Notify" to avoid script errors).
- "BOM Balloon" check: flags BOM item numbers that have no balloon — [SOLIDWORKS Help 2021, BOM Balloon](https://help.solidworks.com/2021/english/SolidWorks/solidworks_design_checker/r_bom_balloon.htm) (snippet).
- An Overlapping Dimensions check exists on the Dimensions tab — [CATI "How to Use Design Checker Part 4"](https://www.cati.com/?p=120123) (snippet). The search summary also listed overlapping views and extension-line visible gap as drawing checks, from an [Engineers Rule article, SOLIDWORKS 2017 era](https://www.engineersrule.com/?p=2198). **[UNVERIFIED in current help]**
- Failed checks can be auto-corrected where supported — [TriMech blog](https://store.trimech.com/?p=42727); [CATI](https://www.cati.com/blog/solidworksdesign-checker/).

**Batch and automation**
- SOLIDWORKS Task Scheduler has a "Design Checker" task. You add one or more `.swstd` files and select files or a folder (optionally including subfolders, but not files and folders mixed in one task). It produces a per-file detailed report plus a summary report for multi-file runs, saved in a folder named with the file name and a date/hour stamp. The machine must be on at the scheduled time, otherwise the task runs at next startup — [Hawk Ridge](https://support.hawkridgesys.com/hc/en-us/articles/26745157214989-How-to-Use-the-Design-Checker-Task-in-SOLIDWORKS-Task-Scheduler); [SOLIDWORKS Help 2020, Task Scheduler Design Checker task](https://help.solidworks.com/2020/english/SolidWorks/sldworks/HIDD_TASK_DESIGN_CHECKER.htm).
- Task Scheduler features beyond basic conversion tasks need Professional/Premium, and Standard is limited mostly to conversion tasks — [TriMech blog](https://store.trimech.com/?p=42727); [CATI Task Scheduler](https://www.cati.com/?p=169123) (snippets).
- API: the Design Checker API (`SolidWorks.Interop.dsgnchk`) exposes `ISWDesignCheck.RunDesignCheck4` (standards file, report folder, add to Design Binder, overwrite, AutoCorrect, returns an error code). `RunDesignCheck5` adds a report-format parameter and a result-summary string output. The add-in object is obtained with `GetAddInObject("SWDesignChecker.SWDesignCheck")`, and a version-specific ProgID is recommended — [API Help 2010, RunDesignCheck4](https://help.solidworks.com/2010/english/api/dsgnchkapi/SolidWorks.Interop.dsgnchk~SolidWorks.Interop.dsgnchk.ISWDesignCheck~RunDesignCheck4.html); [API Help 2015, RunDesignCheck5](https://help.solidworks.com/2015/English/api/dsgnchkapi/SOLIDWORKS.Interop.dsgnchk~SOLIDWORKS.Interop.dsgnchk.ISWDesignCheck~RunDesignCheck5.html); [API example 2021](https://help.solidworks.com/2021/english/api/dsgnchkapi/Run_SOLIDWORKS_Design_Checker_Example_VB.htm).
- 2025 help still documents Design Checker — [SOLIDWORKS 2025 Design Checker Overview tutorial](https://help.solidworks.com/2025/English/swconntutorial/Design_Checker_Overview.htm).

**Known limitations raised by users**
- A user forum thread asks whether Design Checker can confirm every model feature is dimensioned on the drawing ("missing dimensions"). The thread gives no definitive yes — [3DSwym forum, Check drawing for missing dimensions](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/check-drawing-for-missing-dimensions_DKMJvsGCQneUMYQj7-cdtw); [3DSwym, Design Checker Functionality](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/design-checker-functionality_92-odL8_Qy2CQtVGAOZUaQ).
- A Visiativ myCAD forum user (French PDM context) wants automatic pre-approval checks for overlapping views, annotations and dangling dimensions and is looking for "an alternative to Design Checker". This suggests users find Design Checker insufficient or awkward for pre-release gating — [myCAD forum](https://forum.mycad.visiativ.com/t/controle-automatique-des-mises-en-plan-avant-validation-alternative-a-design-checker?locale=en).

### Inferences
- The API examples run against the active document inside a running SOLIDWORKS session. A custom batch runner would therefore have to open each file through the SOLIDWORKS API, then call `RunDesignCheck5`. That requires a licensed SOLIDWORKS Professional/Premium seat on the machine, and Design Checker cannot run standalone without SOLIDWORKS.
- A Python tool could call Design Checker via COM (pywin32) and parse its report, reusing existing `.swstd` rules instead of reimplementing font/standard checks.
- The output is report files (HTML, and possibly XML via the RunDesignCheck5 format parameter **[UNVERIFIED which formats]**), not a structured Excel review sheet with reviewer columns.

### Gaps
- No complete, current (2024–2026) list of drawing check types could be read. The help pages were unreachable, so the exact drawing check list for SOLIDWORKS 2025/2026 is unverified.
- Whether Design Checker validates **tolerance values / GD&T frame content** (as opposed to dimension display style) is unverified. No source showed a tolerance-specific check.
- The exact list of `RunDesignCheck5` report formats is unverified.
- Whether custom-property checks can enforce value patterns (regex) or only existence/value equality is unverified.

## Q2. Which tools do BOM vs custom-property consistency checks, and how do the other tools compare?

### Takeaway
No single off-the-shelf SOLIDWORKS tool found here cross-checks the 3D model, the drawing (title block/BOM table) and the ERP BOM together. Coverage is fragmented. Design Checker and PDM data-card/transition conditions check property existence and values. CAD-to-ERP connectors (CADLink, BOM Creator, Global Shop BOM Compare) compare CAD BOM against ERP BOM. Batch property editors (Xarial CAD+, myCADtools BatchProperties) edit properties more than they validate them. Other CAD vendors (AutoCAD CAD Standards, Creo ModelCHECK, NX Check-Mate) offer standards checkers of a similar rule-file style. AI tools (Werk24, CoLab AutoReview, Leo AI, CADDi Drawer) extract data from or review drawing images and PDFs.

### Cited Findings

**SOLIDWORKS PDM (data cards, workflow transition conditions)**
- Workflow transition conditions can test category, file path, object type, revision, and any data-card variable, with comparison operators and an optional configuration tab (blank = all configurations). If a file fails, the state change is blocked with a message box. The object type condition can target BOM/File/Item, but BOM and Item apply to PDM Professional only — [SOLIDWORKS PDM Admin Help 2018, Adding Transition Conditions](https://help.solidworks.com/2018/English/EnterprisePDM/Admin/t_workflow_transition_conditions.htm); Korean help exists for older versions ([2014 Korean](https://help.solidworks.com/2014/korean/EnterprisePDM/Admin/t_workflow_transition_conditions.htm)).
- Conditions are ANDed unless placed under an OR block — [Javelin PDM conditions blog](https://www.javelin-tech.com/blog/?p=12619).
- PDM Professional can export BOMs (e.g. XML) on release for ERP feeds, but it does not compare them by itself — [GoEngineer, Using and exporting BOMs in PDM](https://goengineer.com/blog/using-and-exporting-boms-from-inside-solidworks-pdm).
- Licensing: PDM Standard vs Professional distinction as above. Pricing not public in sources found.

**DriveWorks (DriveWorksXpress / Solo / Pro)**
- DriveWorksXpress (included with SOLIDWORKS) and Pro support input validation in forms ("dynamic validation and validation messages") — [DriveWorks feature comparison](https://docs.driveworkspro.com/Topic/DriveWorksFeatureComparison); [DriveWorksXpress page](https://www.driveworks.co.uk/products/driveworksxpress/).
- Solo adds drawing control (sheets, views, annotation text/positions, layers, break lines, custom properties) and generation reports giving an audit trail of drawing values — [DriveWorks Solo](https://driveworks.co.uk/products/driveworkssolo).
- Limitation: DriveWorks is a rules-based design-automation and configurator tool. It validates inputs to generated designs, not manually made drawings. It is not a drawing checker.

**CAD Booster Drew**
- Drew is a SOLIDWORKS drawing-automation add-in that stores drawing preferences in templates called "Blueprints". It offers a 14-day free trial with all features and no credit card — [CAD Booster page via mirror](https://verifico.duckdns.org/CAD-Booster) (third-party mirror, so verify on cadbooster.com).
- **[UNVERIFIED]** Whether Drew has a drawing-checking/validation feature, and its price. cadbooster.com was unreachable and no search result confirmed either.

**Xarial CAD+ Toolset (Batch+, BOM+ etc.)**
- CAD+ Toolset is a collection of SOLIDWORKS utilities: batch custom property editing, macro library management, and batch macro execution (Batch+). It is listed as a SOLIDWORKS Solution Partner product — [SOLIDWORKS partner page, CAD+ Toolset](https://solidworks.com/partner-product/cad-toolset).
- It comes as a free community edition (open source) plus commercial Standard/Professional/Premium editions. Batch+ and Batch+ Command Line (with macro arguments) are in the community edition. Some modules (e.g. advanced BOM+) are limited to paid tiers — [GitHub cad-plus (fork)](https://github.com/EddyAlleman/cad-plus). Prices are not shown.
- Relevance: Batch+ can run a custom validation macro over many files from the command line, which makes it a possible execution harness for custom checks. It is not itself a rule checker.

**Visiativ myCADtools BatchProperties**
- A batch custom-property tool, part of the myCADtools suite — [Visiativ help 2023, BatchProperties](https://help.visiativ.com/mycadtools/2023/en/BatchProperties.html). Details of validation capability were not examined.

**SOLIDWORKS Inspection (related)**
- It balloons drawings, creates inspection reports (standalone or add-in), and can compare drawing revisions. It is focused on quality inspection, not standards checking — [Javelin video/blog](https://www.javelin-tech.com/blog/?p=15251) (snippet). Third-party alternatives: Albitech Inspection Geni / BallOOn-It (freeware) — [Newswire](https://www.newswire.com/view/content/inspection-geni-for-solidworks-gives-designers-peace-of-mind-130244).

**CAD-to-ERP BOM comparison**
- CADLink (QBuild): a bidirectional SOLIDWORKS/EPDM-ERP link with "real time BOM comparison" that highlights discrepancies for the user to resolve — [CADLink brochure (SOLIDWORKS partner)](https://files.solidworks.com/partners/pdfs/cadlink-brochure_solidworks_2021653.pdf); [SAP partner listing](https://www.sap.com/sweden/products/scm/partners/qbuild-corporation-cadlink.html).
- BOM Creator (Servtronica): connects SOLIDWORKS/PDM to SAP Business One / S/4HANA, with a "Comparison of Machines" feature — [SOLIDWORKS partner page](https://www.solidworks.com/partner-product/bom-creator).
- Global Shop Solutions BOM Compare: side-by-side CAD vs ERP BOM with color-coded differences. It works only within the Global Shop ERP — [product sheet](https://www.globalshopsolutions.com/bom-compare-software-for-manufacturing).
- Open source: `bomcheck` (Python, PyPI) compares a SOLIDWORKS BOM exported to Excel with an ERP BOM exported to Excel, including unit conversion — [PyPI bomcheck](https://pypi.org/project/bomcheck). It is a direct precedent for the Korean team's approach.

**Autodesk AutoCAD CAD Standards**
- The CAD Standards checker compares a drawing against `.dws` standards files using plug-ins for layers, dimension styles, linetypes, text styles, and multileader styles. Multiple `.dws` files can be attached, with order setting priority — [Autodesk Help 2027, About CAD Standards](https://help.autodesk.com/cloudhelp/2027/ENG/AutoCAD-Core/files/GUID-D64F8076-4978-44B7-B056-D921C77FEA88.htm); [Autodesk blog](https://blogs.autodesk.com/autocad/?p=9863).
- The Batch Standards Checker audits many drawings and produces a violations report. Fixes are made per drawing with CHECKSTANDARDS. Third-party tips describe a `.chx` check file and HTML/XML reports — [NOVEDGE tip](https://novedge.com/blogs/design-news/autocad-tip-autocad-batch-standards-checker-enforce-project-wide-cad-standards) **[.chx/report format details UNVERIFIED against Autodesk docs]**.
- Limitation: it checks named-object styles (layers, styles), not dimension values, title-block content, or BOM consistency.

**PTC Creo ModelCHECK**
- ModelCHECK includes drawing checks such as BOUND_INFO (items outside the drawing boundary), FORMAT_NAME (approved drawing format), and PARAMS_EXIST (required parameters). Creo+ May 2026 added DRIVEN_DIMENSIONS, DRAFT_DIMENSIONS and ERASED_DIMENSIONS drawing checks, configurable as error or warning — [PTC Creo+ What's New May 2026](https://support.ptc.com/help/creo/creo_plus/usascii/whats_new/mdlchk_new_checks.html); [PARAMS_EXIST](https://support.ptc.com/help/creo/creo_plus/usascii/model_analysis/modelcheck/params_exist.html); [FORMAT_NAME](https://support.ptc.com/help/creo/creo_plus/usascii/model_analysis/modelcheck/format_name.html); [BOUND_INFO](https://support.ptc.com/help/creo/creo_pma/r12/usascii/model_analysis/modelcheck/bound_info.html).
- ModelCHECK can be configured for spell-checking drawing notes and title blocks — [PTC article CS162392](https://www.ptc.com/en/support/article/CS162392) (snippet).

**Siemens NX Check-Mate**
- Hundreds of standard checks across modeling, drafting, PMI, geometry, routing and welding. Results can be stored in Teamcenter and used in release/baseline workflows. Since June 2023 a Checker Editor lets users author custom checkers with a logic editor — [Siemens NX design validation](https://plm.sw.siemens.com/es-ES/nx/cad-online/mcad-software/design-validation); [Siemens blog June 2023](https://blogs.sw.siemens.com/nx-design/whats-new-in-nx-june-2023-validation/).
- A consultancy describes drawing checks for dimensions/tolerances, texts/tables, and out-of-date views — [DRWE](https://drwe.de/en/consulting/teamcenter/integration-tool-kit).
- Check-Mate is a separately licensed module (`ug_checkmate` license) — [Siemens support KB](https://support.sw.siemens.com/en-US/okba/PL8554510/What-does-ug_checkmate-do/index.html) (snippet).

**AI-based checkers / extractors (brief)**
- Werk24: an API that extracts PMI (dimensions, GD&T ISO/ASME, tolerances, threads, materials, surface finish, BOM, title block) from drawing images/PDFs to JSON, with per-field confidence. It offers REST, webhooks and a Python client, takes about 5–20 s per page, and is EU-hosted. Vendor claims 95%+ accuracy. Its ROI calculator assumes a €490 tier fee plus €0.85/page (Essential tier) — [Werk24 homepage](https://werk24.io/); [Werk24 llms.txt](https://werk24.io/llms.txt); [PMI extraction](https://Werk24.io/product/pmi-feature-extraction). All vendor claims.
- CoLab AutoReview: an AI "peer checker" for 2D drawings and 3D models that produces comments/redlines against team standards and past review feedback — [Design News](https://www.designnews.com/artificial-intelligence/ai-tools-for-engineers-autoreview); [CoLab 2026 guide](https://www.colabsoftware.com/guides/ai-tools-for-cad-standards-enforcement-your-complete-guide-to-automated-compliance-2026) (vendor-authored).
- Leo AI: marketed as a drawing checker that flags missing dimensions and tolerance issues against standards — [Leo AI blog](https://www.getleo.ai/blog/ai-engineering-drawing-review) (vendor marketing).
- CADDi Drawer: drawing image analysis and similarity search for centralizing and reusing 2D drawings. It is a drawing management/search tool, not a compliance checker — [Subscribed.fyi CADDi Drawer](https://subscribed.fyi/caddi-drawer/).
- NexCAD.ai: marketed as AI pre-release drawing review (missing dimensions, incorrect specs) — [Develop3D](https://develop3dlive.com/?p=17595) (vendor claim).
- "Drafter" and "Lido": no reliable sources found. See Gaps.

### Inferences
- **Custom-property checks**: Design Checker (existence/value via `.swstd`) and PDM transition conditions (data-card variable values at state change). **BOM checks**: Design Checker's BOM balloon check only. **CAD vs ERP BOM**: commercial ERP connectors or open-source bomcheck. None found does all three, which matches the user's custom-tool rationale.
- PDM transition conditions act as a gate (pass/fail per file at release) rather than a review report, and they need PDM Professional for BOM/Item object types.
- AI tools work on images/PDFs and are cloud/SaaS (data residency is a concern for semiconductor-equipment IP). They complement a deterministic API-based check rather than replace it.

### Gaps
- Pricing for Design Checker standalone (bundled only), Xarial commercial editions, CAD Booster Drew, CADLink, CoLab, Leo AI and CADDi was not found publicly.
- "Lido" as a drawing checker could not be identified. "Drafter" returned no relevant product. They may be niche or misnamed.
- No source confirmed whether CAD Booster Drew performs drawing validation.
- Korea-specific resellers or offerings (e.g. local SOLIDWORKS VARs offering drawing-check add-ins) were not researched.

## Q3. What gaps remain that justify a custom (Python) tool?

### Takeaway
The commercial and built-in tools mostly check styles and settings (fonts, layers, standards, dangling items) within one document, or they compare CAD BOM vs ERP BOM inside an ERP-specific connector. None of the sources show a tool that checks 3D model properties, the drawing's title block and BOM table, and an ERP/Excel BOM against each other in one pass and writes an Excel review sheet. Missing-dimension and tolerance-reasonableness checks are also weakly covered by rule-based tools. A standalone tool, possibly calling SOLIDWORKS/Design Checker via COM, fills these gaps.

### Cited Findings
- Design Checker reports go to report folders (HTML / summary) and Design Binder, not an Excel review sheet — [API RunDesignCheck4](https://help.solidworks.com/2010/english/api/dsgnchkapi/SolidWorks.Interop.dsgnchk~SolidWorks.Interop.dsgnchk.ISWDesignCheck~RunDesignCheck4.html); [Hawk Ridge](https://support.hawkridgesys.com/hc/en-us/articles/26745157214989-How-to-Use-the-Design-Checker-Task-in-SOLIDWORKS-Task-Scheduler).
- Users explicitly look for missing-dimension detection and pre-approval drawing checks beyond Design Checker — [3DSwym forum](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/check-drawing-for-missing-dimensions_DKMJvsGCQneUMYQj7-cdtw); [myCAD forum](https://forum.mycad.visiativ.com/t/controle-automatique-des-mises-en-plan-avant-validation-alternative-a-design-checker?locale=en).
- CAD vs ERP BOM comparison is tied to specific ERP connectors (CADLink, BOM Creator for SAP, Global Shop) — [CADLink brochure](https://files.solidworks.com/partners/pdfs/cadlink-brochure_solidworks_2021653.pdf); [BOM Creator](https://www.solidworks.com/partner-product/bom-creator); [Global Shop](https://www.globalshopsolutions.com/bom-compare-software-for-manufacturing).
- Open-source precedent: bomcheck compares Excel exports of a SOLIDWORKS BOM and an ERP BOM in Python — [PyPI bomcheck](https://pypi.org/project/bomcheck).
- An open-source SOLIDWORKS MCP server (Python) advertises a `validate_drawing_completeness` tool (missing dimensions/annotations/views). This is a developer-community example, not a commercial product — [Glama listing, SolidworksMCP-python](https://glama.ai/mcp/servers/andrewbartels1/SolidworksMCP-python/tools/validate_drawing_completeness).
- Automation harness options exist: Task Scheduler (Professional+), the Design Checker COM API, and Xarial Batch+ command line (free community edition) — sources above.

### Inferences
- Gaps that justify a custom tool:
  1. Three-way cross-check: model custom properties vs drawing title block/sheet properties vs drawing BOM table vs ERP/Excel BOM (part number, revision, material, quantity, description in Korean/English).
  2. Excel review-sheet output with per-item status, reviewer comments, and history, which fits Korean team review/approval practice. Design Checker's HTML reports do not fit this.
  3. Domain rules for semiconductor equipment (e.g. vendor part number formats, material/surface-treatment codes, cleanroom notes) that are awkward to express in `.swstd`.
  4. Independence from the SOLIDWORKS license tier for some checks: reading exported PDF/DXF/Excel BOMs offline, while parsing live documents still needs SOLIDWORKS + COM (or the Document Manager API **[licensing not researched here]**).
  5. Missing-dimension and tolerance-reasonableness heuristics, which no rule-based SOLIDWORKS tool was shown to cover.
- Reuse strategy: call `RunDesignCheck5` from Python (pywin32) for style/standards checks, and spend custom effort on cross-document and ERP consistency plus Excel output. Optionally use Werk24-style extraction only for legacy PDF drawings, but with IP/data-residency caution.

### Gaps
- No source confirmed whether any commercial SOLIDWORKS add-in (e.g. Visiativ, SolidKits, CAD Booster) already offers a combined model/drawing/ERP cross-check with Excel output. This should be verified with Korean resellers before building.
- SOLIDWORKS Document Manager API licensing (it allows reading properties/BOM without full SOLIDWORKS) was not researched here.
- No 2026 changes to Design Checker (e.g. in SOLIDWORKS 2026) were found or verified.
