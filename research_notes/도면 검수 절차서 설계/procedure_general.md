# 도면 검도·검수 절차서 구조와 공통(General) 체크리스트

> Research limitation (important for the report writer): WebFetch was blocked by the session network allowlist for every primary-source host tried: ntrs.nasa.gov, fiveflute.com, gdandtbasics.com, hii.com, wsdot.wa.gov and training.mts.com. Findings below therefore come mostly from search-result snippets and summaries of those pages, not from full-text reads. Clause numbers for ISO/ASME/KS standards are marked **[unverified]** wherever no fetched source confirmed them. Items marked **[background knowledge]** come from general domain knowledge, not from a source fetched in this session. Treat both as needing confirmation against the standard text.

## Q1. Well-known published drawing checklists and what they contain

### Takeaway
No single public "NASA drawing checker checklist" turned up. NASA publishes center drawing-practice manuals (KSC-GP-435 Vol. I Rev H, GSFC Engineering Drawing Standards Manual), and organizations write their own checklists on top of them. The most useful public checklists come from companies and consultancies (Five Flute part-drawing checklist, GD&T Basics checklist, HII/Navy approved-drawing submittal guide, the MTS Drawing & BOM Checking work instruction). Together they converge on the same item groups: title block, revision, views, dimensions, tolerances/GD&T, notes, materials and BOM.

### Cited Findings
- KSC-GP-435 Vol. I, Rev H (Aug 2020), "Engineering Drawing Practices – Aerospace and Ground Support Equipment", sets the minimum requirements for engineering drawings / digital product definition data sets at KSC. It is based on the ASME Y14 series and sets the conventions drafting personnel follow in preparing, revising and completing drawings — [NTRS citation](https://ntrs.nasa.gov/citations/20205010487); [PDF](https://ntrs.nasa.gov/api/citations/20205010487/downloads/KSC-GP-435_Vol1_RevH-Final-Signed.pdf) (full text not fetched; summary from search).
- The GSFC Engineering Drawing Standards Manual sets the minimum requirements acceptable at GSFC for drawings of flight hardware and ground support systems — [EngineersEdge listing](https://engineersedge.com/drafting/nasa_engineering_drawing_standards_manual_15265.htm).
- NASA ENG-I-SU03 Rev 3 (2026) is a review instruction in which "the Checker shall use Form ENG-F-SU01 … Review Checklist as the official guideline". It shows the NASA pattern of a procedure (instruction) plus a separately numbered checklist form — [NASA PDF](https://www.nasa.gov/wp-content/uploads/2026/06/eng-i-su03-rev-3-space-utilization-quality-review-instruction.pdf) (summary from search; the form itself was not found).
- Five Flute Part Drawing Checklist (2022-12-03): the reviewer confirms that the datum structure matches the part's function, reviews each feature control frame (FCF) for design intent, and checks tolerances on critical interfaces between mating parts — [Five Flute PDF](https://www.fiveflute.com/assets/Five-Flute-Part-Drawing-Checklist-2022-12-03.pdf) (search summary).
- GD&T Basics checklist: Is the revision year of the referenced ASME standard stated? Are datums established and applied correctly? Is each geometric control legal and functional? — [GD&T Basics](https://www.gdandtbasics.com/?p=12985) (search summary).
- HII "Navy Approved Drawing Submittal Guide": the title block must carry the drawing number, size and latest revision letter. The revision block must describe each change. Tolerances must be shown, including fractional dimensions, out-of-roundness, run-out, and parallelism/squareness of surfaces and axes — [HII PDF](https://hii.com/wp-content/uploads/2025/06/Navy_Approved_Drawing_Submittal_Guide.pdf) (search summary).
- A practising checker on Eng-Tips describes the order of a check: formatting and borders first, then tolerances. The same checker simplified an over-complicated datum scheme to A|B|C and confirmed that tolerance stack-ups do not cause assembly interference — [Eng-Tips "checker checklist" thread](https://www.eng-tips.com/threads/quot-checker-quot-checklist.151962/page-2).
- A user-uploaded "Engineering Drawings Review Checklist" (provenance unverified) has 20+ items grouped as *Engineering Checks / General Checks / Part Drawings*, with pass/fail marks and a sign-off — [Scribd](https://www.scribd.com/document/343077239/Engineering-Drawings-Review-Checklist-example2-pdf).
- An ISO-oriented mechanical drawing checklist (scraped copy, low reliability) covers title-block completeness, projection method, FCFs, ISO 2768 general tolerances and ISO 1101 geometric tolerancing — [stats.tinkerine.com copy](https://stats.tinkerine.com/fetch.php/u2E27A/243758/Iso%20Drawing%20Checklist%20Mechanical%20Engineering.pdf).
- Korean academic source (한국CDE학회, 1996), "CAD시스템을 위한 컴퓨터원용 설계도면검도 – 기계부품도의 치수검도방법": drawings made in CAD contain the same errors as hand drawings, so 검도 is still needed. Errors found during fabrication or assembly hurt delivery dates. The paper proposes systematic detection of **missing (누락) and redundant (중복) dimensions**, because dimensions are the most basic definition of size and shape — [KoreaScience](https://koreascience.or.kr/article/JAKO199611920957341.page?lang=ko).
- Korean public checklist examples come mostly from construction: 비즈폼 "도면검토 체크리스트" uses the columns 공종 / 세부공종 / 검측일자 / 검사항목, and there is a 건축 실시설계도면 체크리스트 improvement study. No mechanical/equipment-company public checklist was found — [bizforms 109297](https://www.bizforms.co.kr/form_view/form_109297.asp); [KoreaScience JAKO201030159715821](https://koreascience.kr/article/JAKO201030159715821.page?lang=ko).
- Generic review criteria seen in a lower-quality web article: completeness (all required information present), clarity (layout, lines, symbols and notes are understandable) and conformance (follows industry standards) — [pink-ribbon.be article](https://ftp.pink-ribbon.be/?p=47528) (low-authority source).

### Inferences
- Published checklists converge on these sections: (1) format/title block, (2) revision/change, (3) views and projection, (4) dimensioning (complete, not redundant, functional), (5) tolerances (general and specific), (6) GD&T (datum scheme fits function, FCFs legal), (7) surface texture, material and finish, (8) notes, (9) BOM/parts list and balloons (assemblies), (10) engineering checks (fit, stack-up, manufacturability). For JUSTEM, a good GENERAL checklist would cover 1–8 for every drawing, with 9 applying to every assembly drawing.
- A common split separates "drafting/format" checks (mostly automatable) from "engineering" checks (human judgment). The Scribd checklist's Engineering vs General grouping reflects this split.

### Gaps
- Full text of KSC-GP-435, the GSFC manual, the Five Flute and GD&T Basics checklists, and the HII guide could not be fetched, so item lists are partial. Allow these hosts to get verbatim items.
- No Boeing-, GoEngineer- or Hawk Ridge-branded public drawing checklist was found in the searches run.
- No public Korean semiconductor-equipment company 검도 checklist was found. Korean public material is almost entirely construction-sector.

## Q2. Procedure structure (ISO 9001 8.3 alignment, RACI) and the general / project / category checklist layers

### Takeaway
A typical engineering QMS uses a short procedure plus separately controlled checklist forms and work instructions. The procedure defines roles (originator/designer → checker → approver/release) and the redline–backcheck loop. Checklists are layered: a standard (general) list, supplemented by project-specific items, and reviewed periodically (at least annually is one recommendation). ISO 9001 requires only that verification and review activities be planned and controlled and that documented information be retained. It does not prescribe a checklist format.

### Cited Findings
- ISO 9001:2015 secondary guidance: **verification** asks whether design outputs meet design inputs. Evidence includes calculations, checklist reviews, comparison tests and **drawing sign-offs**. Validation checks the product against its intended use — [open-exam-prep study guide](https://open-exam-prep.com/study-guides/iso-9001-lead-auditor/chapter-2b/section-2-3); [Advisera](https://advisera.com/9001academy/knowledgebase/iso9001-design-verifizierung-im-vergleich-zu-design-validierung/).
- Documented information on review, verification and validation activities must be retained — [Presencis ISO 9001 article 8.3 summary](https://cdn.presencis.com/regulations/iso-9001/article-8.3/).
- Audit checklists ask whether the procedures cover revision control, document numbering, and checking and approval of in-house design documents — [SafetyCulture design audit checklist](https://public-library.safetyculture.io/products/audit-checklist-design-and-pe-s-r-ped-nnz7fscxwlulre8q).
- ISO 9001:2015 clause map **[background knowledge; clause numbers believed correct but not fetched]**:
  - 8.3.2 design and development planning: stages, required reviews/verification/validation, responsibilities and authorities, and the documented information needed to show requirements were met.
  - 8.3.3 inputs.
  - 8.3.4 controls: reviews, verification and validation, with action on problems and retained documented information.
  - 8.3.5 outputs: must reference monitoring/measuring requirements and acceptance criteria, and define the characteristics essential for safe and proper use.
  - 8.3.6 design changes: identify, review and control changes, and retain information on changes, review results, authorization and actions.
  - 7.5 documented information: identification, review and approval for suitability and adequacy, version control, retention.
- Real-company example (MTS Systems, "Drawing and BOM Checking Work Instruction" Rev G/H): the reviewer checks each document or redline markup against the appropriate checklist, highlights checked items, and marks corrections in red. The reviewer records an action of Update, Release, or Update & Release. An **ENGR** box is used when the reviewer is the responsible engineer, and a **CHECK** box when the reviewer is someone else. The later revision adds a check by the lab-office owner against the appropriate checklist before release. The WI itself is revision-controlled (Rev G → Rev H, with changes highlighted) — [MTS WI Rev H](https://training.mts.com/Engineering/Drawing_and_BOM_Checking_WI_RevH%20(changes%20highlighted).docx); [MTS WI Rev G](https://training.mts.com/Engineering/Drawing_and_BOM_Checking_WI_RevG%20-%20ENGR015_OLT.pdf) (search summary).
- A civil/structural QMS work instruction requires checks to cover project-specific design information, not only general codes, and lets the standard checklists be **supplemented with additional items** as needed — [Scribd Drawing Checking Guide](https://www.scribd.com/document/708937622/Drawing-Checking-Guide) (provenance unverified).
- Saudi EXPRO "Checklist – General Notes Drawing" template groups questions into sections (preparation and checking, co-related documents, coordination and review). The **originator and the checker both answer** each item Y/N/NA — [EXPRO template](https://www.expro.gov.sa/api/uploads/Checklist%20%20General%20Notes%20Drawing%20Template.docx).
- Guidance seen in search: a generic checklist is a good base, but it should be customized to project requirements and company standards and reviewed at least annually — (snippet surfaced in the same search; source page among [pmis.udsm.ac.tz](https://pmis.udsm.ac.tz/35752982/mchargeh/bfindi/ftackley/checklist+for+structural+engineers+drawing.pdf) and the results above; attribution uncertain).
- STRUCTURE magazine: informal senior-engineer review has largely been replaced by documented QA programs, but senior review remains at the heart of most engineering quality systems — [STRUCTURE mag PDF](https://www.structuremag.org/wp-content/uploads/2015/09/C-StrucDesign-Domalik-Oct151.pdf).
- Independence of checking: there should always be a different set of eyes, ideally someone who did not work on the drawing, before release — [lx.iriss.org.uk "Drawing Checking"](https://www.lx.iriss.org.uk/read/drawing-checking.html) (low-authority site).
- Signatures and ECOs: Eng-Tips practitioners point to ASME Y14.100 and Y14.35 as the governing documents. One practitioner reports that at firms that took this seriously, only the **checker's** initials go on the revised drawing, while the ECO carries the approvals. Another says nothing in the Y14 series supports not signing an ECO-approved drawing. These are anecdotes, not normative text — [Eng-Tips change authorization thread](https://www.eng-tips.com/threads/change-authorization-documents.417108).
- Y14.100-2017 is titled "Engineering Drawing Practices". Revision-letter rules sit in Y14.35 — [Eng-Tips](https://www.eng-tips.com/threads/change-authorization-documents.417108) (edition and title from search; clause content unverified).
- A NASA center pattern pairs a procedure (ENG-I-SU03) with a separately numbered checklist form (ENG-F-SU01) that the checker "shall use" — [NASA PDF](https://www.nasa.gov/wp-content/uploads/2026/06/eng-i-su03-rev-3-space-utilization-quality-review-instruction.pdf).

### Inferences (recommended structure for the JUSTEM procedure; synthesis, not sourced as-is)
- **Document set**:
  - Procedure (절차서, e.g. QP-ENG-0xx): purpose, scope, definitions, R&R, flow, records, references.
  - Work instruction for the check method: marking colors, back-check, automated-check use.
  - Checklist forms, each with its own form number and revision:
    - F-01 General (every drawing).
    - F-02 category-specific, e.g. 가공품 machined part, 판금 sheet metal, 용접 weldment, 조립도 assembly, 배관/가스라인 piping, 전장/하네스 harness, 구매품 purchased-part SCD.
    - F-03 project/customer-specific: customer drawing standard, title-block template, approved materials/finishes, cleanroom/UHV/outgassing restrictions, customer-mandated notes, export markings.
  - This mirrors the NASA procedure-plus-form pattern and the MTS "appropriate checklist" approach.
- **R&R / RACI**:
  - Designer (작성): R for a self-check against the general checklist, running the automated checks, and fixing redlines.
  - Checker (검도): R for an independent check, redlining, the back-check, and signing the checklist. Must not be the author.
  - Approver (승인, team lead): A for release.
  - Document control/PDM admin: R for release, numbering and distribution.
  - Manufacturing/QC/purchasing: C at design review.
  - Customer: C/I where the contract requires it.
- **Flow**: create → self-check plus automated check (attach report) → submit for check → checker redline (paper or PDF markup) → designer corrects → back-check (the checker verifies each redline) → approval → release in PDM with revision → distribution. A change follows ECR → ECO → revised drawing with a revision-block entry → re-check limited to the changed areas plus their effects.
- **Map to ISO 9001**:
  - Drawing check is verification (8.3.4), with the drawing as a design output (8.3.5).
  - Phase design reviews (DR minutes) are reviews (8.3.4).
  - ECOs fall under 8.3.6.
  - Checklist and procedure revision control falls under 7.5.3.
  - Clause numbers [background knowledge].
- **Layering logic**: General = format and standards conformance applicable everywhere. Category = process-driven rules such as bend relief, weld symbols and thread callouts. Project = contract and customer rules. Version each layer independently, record the checklist revision used on each completed check, and review the checklists periodically (e.g. annually and after recurring-defect analysis).

### Gaps
- Exact ISO 9001:2015 wording of 8.3.4 / 8.3.5 / 8.3.6 / 7.5.3 was not retrieved; confirm against the standard.
- No source fetched specifying Y14.100 / Y14.35 clauses on approval signatures or checker role.
- No published Korean mechanical-company 검도 절차서 found.

## Q3. Content of a good GENERAL drawing checklist

### Takeaway
Published checklists and the relevant standards support a general list covering: title block, revision/ECO, sheet format and views/projection, dimension completeness and non-redundancy, general tolerance callout (ISO 2768 / KS equivalent), GD&T legality and datum logic, surface texture, material and finish, notes, and (for assemblies) BOM and balloons. The checklist should also require that the standards and their editions be cited on the drawing.

### Cited Findings
- **Title block**: drawing number, size and latest revision letter (HII/Navy). Title-block completeness (ISO checklist) — [HII](https://hii.com/wp-content/uploads/2025/06/Navy_Approved_Drawing_Submittal_Guide.pdf); [ISO checklist copy](https://stats.tinkerine.com/fetch.php/u2E27A/243758/Iso%20Drawing%20Checklist%20Mechanical%20Engineering.pdf).
- **Revision block**: each change must be described in the revision block — [HII](https://hii.com/wp-content/uploads/2025/06/Navy_Approved_Drawing_Submittal_Guide.pdf). Revision rules: ASME Y14.35 — [Eng-Tips](https://www.eng-tips.com/threads/change-authorization-documents.417108).
- **Projection method** must be indicated (1st vs 3rd angle) — [ISO checklist copy](https://stats.tinkerine.com/fetch.php/u2E27A/243758/Iso%20Drawing%20Checklist%20Mechanical%20Engineering.pdf).
- **Dimensions**: check for missing (누락) and redundant (중복) dimensions — [KoreaScience 1996](https://koreascience.or.kr/article/JAKO199611920957341.page?lang=ko).
- **Tolerances**: specific tolerances shown where needed, including form, runout and orientation — [HII](https://hii.com/wp-content/uploads/2025/06/Navy_Approved_Drawing_Submittal_Guide.pdf). Critical-interface tolerances must be checked against mating parts — [Five Flute](https://www.fiveflute.com/assets/Five-Flute-Part-Drawing-Checklist-2022-12-03.pdf). Stack-up must be checked so the assembly does not interfere — [Eng-Tips](https://www.eng-tips.com/threads/quot-checker-quot-checklist.151962/page-2).
- **General tolerance**: ISO 2768-1 has classes f/m/c/v (linear and angular). ISO 2768-2 has classes H/K/L (geometrical tolerances without individual indication) — [Xometry ISO 2768](https://www.xometry.com/resources/certifications/iso-2768); [3ERP](https://www.3erp.com/blog/iso-2768-standard/). The Korean national list shows KS B ISO 2768-2 as current (2019 confirmed) — search summary of a KS listing. KS B 0412 as the Korean 보통공차 standard is **[unverified]**: possibly superseded or adopted as KS B ISO 2768-1; confirm on e-ks.kr / standard.go.kr.
- **GD&T**: state the edition of the referenced standard (e.g. ASME Y14.5-2018). Datums must be established correctly and suit the function. Each geometric control must be legal and functional — [GD&T Basics](https://www.gdandtbasics.com/?p=12985); [Five Flute](https://www.fiveflute.com/assets/Five-Flute-Part-Drawing-Checklist-2022-12-03.pdf). For ISO-based drawings the reference is ISO 1101 — [ISO checklist copy](https://stats.tinkerine.com/fetch.php/u2E27A/243758/Iso%20Drawing%20Checklist%20Mechanical%20Engineering.pdf).
- **KS B 0001** is the base Korean mechanical drawing standard. For example, KS B 0005 (spring drawing) defers to KS B 0001 for anything it does not cover — search summary of KoreaScience/KS listing (exact clause content on title block and parts list not retrieved).
- **Korean education syllabus**: 주서 (notes) and 기계 재료 표시 방법 (material designation) are taught as distinct 제도 topics — [KOCW syllabus](https://www.kocw.net:444/home/common/contents/syllabus/1460540_20230403142651316.pdf).

### Proposed general checklist items (synthesis for JUSTEM; standard numbers marked where unverified)
1. **Title block / properties**:
   - Drawing number matches the part number and PDM.
   - Title/description, material, finish/treatment, mass.
   - Scale, sheet size, sheet x of y.
   - Projection symbol.
   - Units.
   - General-tolerance callout (e.g. "ISO 2768-mK" or the company table).
   - Drawn/checked/approved names and dates.
   - Customer/project field.
   - Confidentiality/export notice.
2. **Revision**:
   - Revision letter consistent across the title block, revision table, file and PDM.
   - Each revision has an ECO number, a description, a date and an approver.
   - Changed areas flagged (delta/cloud).
   - First release convention (e.g. "-" or "A") follows the company rule. Y14.35 conventions [clause numbers unverified].
3. **Views**:
   - Correct projection.
   - Enough views, sections and details to define every feature, with no superfluous views.
   - Section and detail labels match.
   - Views are not out of date relative to the model (no "dangling" annotations).
4. **Dimensions**:
   - Every feature fully defined. No redundant or double dimensions, chain-vs-baseline used per function.
   - Reference dimensions in parentheses.
   - No overridden (text-replaced) driven values.
   - Units and decimals consistent with the tolerance.
   - Hole and thread callouts are complete (size, depth, pitch, class, counterbore/countersink).
5. **Tolerances**:
   - The general tolerance covers everything without a specific tolerance.
   - Functional fits carry ISO 286 fit classes [standard number from background knowledge].
   - Stack-up is checked for critical interfaces.
6. **GD&T**:
   - The standard and edition are stated (ASME Y14.5-2018 or ISO 1101/GPS).
   - The datum reference frame is functional and datums are on real features.
   - FCF syntax is legal and modifiers are valid.
   - Basic dimensions exist for every position/profile.
   - Each datum feature is identified only once.
7. **Surface texture**:
   - Symbol per ISO 1302 (now ISO 21920-1:2021 for indication) [edition relationship from background knowledge — verify].
   - Default roughness given in the title block or notes.
   - Sealing, vacuum and wafer-contact faces carry explicit values.
8. **Material and finish**:
   - Material spec and grade (e.g. A6061-T6, SUS316L).
   - Finish/coating spec with thickness, color and masking.
   - Heat treatment and hardness.
   - Cleanroom/outgassing constraints (a project layer item for semiconductor equipment).
9. **Notes**:
   - Numbered general notes using company standard wording.
   - No conflicts with dimensions.
   - Deburring, edge break, flags and cleanliness requirements.
   - Every referenced standard is listed.
10. **BOM / parts list and balloons (assemblies)**:
    - Every component ballooned exactly once. Item numbers match the BOM.
    - Quantities match the model.
    - Part numbers, descriptions and purchased-part vendor/PN present.
    - Torque, adhesive and assembly notes present.
    - Parts-list format follows ASME Y14.34 [scope from background knowledge — verify] or the company/KS form.
11. **Standards and format**:
    - Line types, text height and arrowheads per ISO 128/129 [ISO 129-1 covers dimension indication — background knowledge] or KS B 0001.
    - Company drawing template version is current.

### Gaps
- Verbatim KS B 0001 title-block / parts-list requirements and the current status of KS B 0412 were not retrieved.
- The ISO 1302 → ISO 21920-1 supersession and the ASME Y14.34 scope come from background knowledge only.

## Q4. Checker practices (color marking, sign-off, severity, metrics) and automatable vs human-judgment checks

### Takeaway
The de-facto marking convention (formalized by WSDOT and piping-industry guides) is **yellow = verified correct, red = add or correct, green = delete or instructions to the corrector**, with offices free to set their own scheme. Sign-off is a checklist with Y/N/NA per item, completed by both originator and checker, plus checker initials on the drawing. Drawing-quality metrics found in sources are FPY (first-pass yield), defects per sheet and rework cycle time, with defect categorization to target recurring causes. Format, property and annotation-consistency checks can be automated (e.g. SOLIDWORKS Design Checker or a custom API). Fit, function, datum choice, tolerance adequacy and manufacturability need human judgment.

### Cited Findings
- WSDOT terminal design manual (Appendix E): **red** = additions/corrections, **green** = deletions, **yellow** = verified correct. Blue/pink are used for concurrence and reviewer notes. The purpose is an auditable trail that each item was checked — [WSDOT M3082 App. E](https://wsdot.Wa.Gov/publications/manuals/fulltext/M3082/AppendixE.pdf) (search summary).
- Piping checking color code: yellow = correct, no change. Red = information to add (described as universal). Green = instructions to the person making corrections — [Scribd CheckingColorCode](https://www.scribd.com/document/361812111/CheckingColorCode-pdf).
- Drafting forums: red for additions, blue (or green) for deletions, yellow for approved. "Whatever the office prefers" governs — [AUGI forum](https://forums.augi.com/archive/index.php/t-113565.html).
- MTS WI: the reviewer highlights checked items and marks corrections in red. ENGR vs CHECK sign-off boxes — [MTS WI Rev H](https://training.mts.com/Engineering/Drawing_and_BOM_Checking_WI_RevH%20(changes%20highlighted).docx).
- EXPRO template: the originator and the checker each answer every item Y/N/NA — [EXPRO](https://www.expro.gov.sa/api/uploads/Checklist%20%20General%20Notes%20Drawing%20Template.docx).
- Metrics: defect density per sheet, first-pass yield and rework cycle time are the primary drawing-QA metrics. Checklist completion and standards exceptions are leading indicators. Recurring defects should feed back into templates and symbol libraries — [geosurge.ai](https://www.geosurge.ai/corpus/gen-4384/engineering-drawing/drawing-quality-assurance.html) (low-authority, marketing-corpus site).
- FPY definition: units passing the first time ÷ units started. Reworked units count as failures. Define process boundaries consistently — [6sigma.us FPY](https://www.6sigma.us/manufacturing/first-pass-yield-fpy/); [Guidewheel](https://www.guidewheel.com/blog/first-pass-yield-scrap-rate). "Errors per x opportunities" is an alternative framing — [Cirris](https://cirris.com/first-pass-yield-part-4/).
- An MIT thesis on FPY at a semiconductor equipment maker proposed categorizing error reports to target recurring failure modes — [MIT DSpace 1721.1/107027](https://dspace.mit.edu/handle/1721.1/107027?show=full).
- SOLIDWORKS Design Checker (Professional/Premium):
  - Checks title blocks, custom properties, layers, fonts, units, overridden dimensions, arrow styles and sheet formats.
  - Can "learn" checks from a reference document, auto-correct, and run in batch.
  - Produces HTML reports.
  - Sources: [Javelin](https://www.javelin-tech.com/3d/solidworks-design-checker/); [Javelin blog](https://www.javelin-tech.com/blog/2019/10/solidworks-standards-compliance-design-checker/); [SOLIDWORKS Help 2019 – Build Checks](https://help.solidworks.com/2019/English/SolidWorks/solidworks_design_checker/c_build_checks.htm).
- Vendor claim: manual checklist comments are hard to audit unless tied to evidence in the drawing — [Nomic](https://www.nomic.ai/use-cases/automated-drawing-review/ai-project-requirements-design-criteria-review) (vendor marketing).

### Inferences
- **Severity classes** (no authoritative source found; proposed):
  - Critical/A: fit, function or safety error, wrong tolerance, wrong material. The drawing cannot be released.
  - Major/B: missing information that forces a supplier RFI or creates an ambiguity.
  - Minor/C: format or style issues, typos.
  - Optional Observation: suggestions.
  - Release gate: zero A/B open and C dispositioned.
- **Metrics for JUSTEM**:
  - FPY = drawings approved at the first check ÷ drawings submitted.
  - Defects per drawing (or per sheet) by severity and category.
  - Check lead time.
  - Post-release defects: ECOs caused by drawing errors, supplier RFIs, NCRs traced to drawings — the "escape rate" of the check.
  - Automated-check pass rate at submission.
- **Automatable from CAD/SOLIDWORKS API (largely deterministic)**:
  - Title-block and custom-property presence and format; PN ↔ file name ↔ PDM match.
  - Revision consistency (property vs revision table).
  - Template/sheet-format version.
  - Projection setting and units.
  - General-tolerance note presence.
  - Overridden or dangling dimensions and annotations.
  - Model fully dimensioned / under-defined features: partly possible via the DimXpert or "missing dimension" heuristics.
  - Duplicate dimensions across views.
  - Text font/height and layer.
  - Material property set and allowed-material list.
  - Finish property set.
  - Hole callouts generated from Hole Wizard.
  - BOM ↔ balloon completeness and quantities. Purchased-part fields filled.
  - Drawing/model out-of-date or rebuild errors.
  - Datum/FCF syntax: partly checkable.
  - Surface finish symbol present on required faces: needs face tagging.
- **Requires human judgment**:
  - Whether dimensioning and the datum scheme reflect function and inspection.
  - Whether tolerances are adequate or too tight (cost), and stack-up results.
  - Manufacturability for the chosen process.
  - Material and finish suitability (vacuum, plasma, chemical compatibility, particles).
  - Note correctness for the application.
  - View sufficiency and clarity.
  - Conformance to customer/project intent.
  - Safety and regulatory (CE/SEMI S2) implications.
- **Records**:
  - Completed checklists with the checklist revision and signatures.
  - Marked-up check prints or PDF markups, kept until release (or longer per the retention policy).
  - Automated-check report attached to the PDM workflow.
  - DR minutes with action items and closure.
  - ECR/ECO with approvals.
  - Release record (who, when, revision).
  - These satisfy "retain documented information" for 8.3.4/8.3.6 (clause numbers [background knowledge]).

### Gaps
- No authoritative published severity-classification scheme for drawing defects was found.
- No benchmark values for drawing FPY or defects per drawing in the semiconductor-equipment industry were found.
- No public example of a SOLIDWORKS-API (non-Design-Checker) drawing check suite with item lists was retrieved.
