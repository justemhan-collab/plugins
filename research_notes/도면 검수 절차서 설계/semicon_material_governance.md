# Semiconductor Equipment Material/Finish Requirements and Material Master Governance (for a drawing review procedure)

Research date: 2026-10-28. Scope: a FOUP/EFEM purge & air-curtain equipment maker using SolidWorks and ERP.
Method note: these WebFetch hosts were blocked in the research session: steelmax.co.kr, intel.com and semi.org. Several findings therefore rest on search-result excerpts of those pages and are marked "(excerpt)". SEMI and KS full texts are paywalled and were not read. Items marked **[UNVERIFIED]** come from expert knowledge or a secondary source and need confirmation against the primary standard.

## Q1. Which SEMI/ISO requirements become concrete drawing review items for a FOUP/EFEM purge & air-curtain equipment maker?

### Takeaway
Few SEMI/ISO documents give drawing-level numbers for a non-wetted, near-wafer tool such as an EFEM, load port purge or air curtain. The review items mostly come from four places:
- SEMI S2-0724 (EHS), especially fire protection Section 14, which sets materials flammability rules: FM 4910 or UL 2360 plastics and limits on combustible mass.
- SEMI F19/F20, for any gas-wetted stainless path (purge N2/XCDA lines): 316L to F20 and electropolished surface to F19.
- ISO 14644-14/-15, which assess equipment suitability for particles and airborne chemicals.
- Customer- or fab-specific material restriction lists, typically covering zinc, cadmium, silicone, outgassing (ASTM E595-type screening) and ESD-dissipative contact materials.

A checker should verify that each drawing names the governing specification. Material grade alone is not enough. Specific numeric limits should come from the customer spec.

### Cited Findings
**SEMI S2 (EHS)**
- The current edition is SEMI S2-0724. The July 2024 release folded in more than 9 delayed revisions, including changes to Fire Protection. A redline against S2-0821 is sold. The next edition is expected in July 2027. — [SEMI Standards Watch: new version S2-0724](https://www.semi.org/en/standards-watch-2024-sep/new-version-of-semi-s2-0724-published) (excerpt); [SEMI store S2](https://store-us.semi.org/products/s00200-semi-s2-environmental-health-and-safety-guideline-for-semiconductor-manufacturing-equipment)
- Fire Protection is Section 14 of S2. S2 has 28 sections and 5 appendices. — [Intertek SEMI S2](https://www.intertek.com/semiconductor-equipment/semi-s2/) (excerpt)

**Flammability of plastics (FM 4910 / UL 2360)**
- FM 4910 is FM Global's Cleanroom Materials Flammability Test Protocol, released in 1997. Tools built from FM 4910-compliant materials do not need on-board fire suppression when no flammable or combustible liquids are used. — [Semiconductor Digest: Cleanroom fire safety](https://sst.semiconductor-digest.com/?p=44804) (excerpt)
- Fire-resistant plastics used in tools include Halar ECTFE, CPVC, PVDF and FR polypropylene. PVDF, PFA, PEEK and PTFE pass FM 4910 but are expensive. — [Semiconductor Digest](https://sst.semiconductor-digest.com/?p=44804) (excerpt)
- Intel's supplier "Combustible Materials Test Report" form accepts FM 4910 or UL 2360 Class 1/2 listings. It states that UL 94 is not acceptable for rating cleanroom equipment plastics. It also sets a limit of less than 1 lb/ft² (5 kg/m²) of combustible plastic relative to tool footprint. — [Intel combustible materials test report](https://www.intel.com/content/dam/www/public/us/en/documents/supplier/combustible-materials-test-report.pdf) (excerpt; fetch blocked)

**Wetted stainless paths (SEMI F19 / F20)**
- SEMI F19 defines wetted-surface characterization and finish acceptance criteria for stainless tubing and components made per SEMI F20. The current F19 edition was approved 2015-05-19. — [SEMI store F19](https://store-us.semi.org/products/f01900-semi-f19-specification-for-the-surface-condition-of-the-wetted-surfaces-of-stainless-steel-components)
- SEMI F20 is the specification for 316L stainless bar, forgings, extruded shapes, plate and tubing. F20 governs the material and F19 governs the surface. — [SEMI store listing via search](https://store-us.semi.org/collections/all?page=33) (excerpt)
- Ra ≤ 0.25 µm (10 µin) electropolished is the commonly cited UHP target. Non-EP stainless typically reaches about 0.5 µm Ra. These are seller blogs, and one wrongly attributes the Ra limit to F20, so they are **[UNVERIFIED vs. F19 text]**. — [Alibaba seller blog](https://seller.alibaba.com/blogs/2026/southeast-asia/semiconductor-materials/316l-ep-stainless-steel-semiconductor-guide-alibaba-b2b)
- An older F19-95 acceptance criterion counts defects at 3,500× SEM: no more than 40 flaws. — [Semiconductor Digest: EP surface morphology](https://sst.semiconductor-digest.com/?p=43099) (excerpt; 1995 edition, may have changed)

**Cleanroom suitability (ISO 14644-14 / -15)**
- ISO 14644-14 gives a method to assess whether equipment (machinery, process equipment, components, tools) suits a cleanroom by airborne particle concentration, for particles from 0.1 µm to ≥5 µm. A 2nd edition, ISO 14644-14:2026, replaced the 2016 edition. — [SCC ISO 14644-14:2026](https://scc-ccn.ca/standardsdb/standards/8192131); [ANSI preview 2026](https://webstore.ansi.org/preview-pages/ISO/preview_ISO+14644-14-2026.pdf)
- ISO 14644-15 (2017) assesses the suitability of equipment and materials by airborne chemical concentration, the chemical counterpart to -14. Whether it explicitly prescribes an outgassing test was not confirmed. — search excerpt, [DIN ISO 14644-14 page](https://din.de/en/wdc-beuth:din21:263246408)

**Outgassing (ASTM E595)**
- ASTM E595 tests at 125 °C and < 7×10⁻³ Pa for 24 h and reports TML, CVCM and optionally WVR. TML 1.00% and CVCM 0.10% "have been used as screening levels for rejection of spacecraft materials". The standard says acceptance criteria "shall be determined by the user". It is a screening test and not valid for computing actual system contamination. The current revision is E595-15(2021). — [ANSI webstore E595-15](https://webstore.ansi.org/standards/astm/astme59515); [NASA outgassing data](https://data.amerigeoss.org/en/dataset/spacecraft-material-outgassing-data) (excerpts)

**Restricted materials**
- University cleanroom and vacuum material lists ban zinc and exclude cadmium. A cleanroom-equipment vendor advises avoiding zinc and zinc-plated parts in favour of anodized Al, stainless or glass. Most silicones, such as some Sylgard grades, are not vacuum compatible. Approved lists are tool-specific. — [Waterloo QNFCF authorized materials](https://qnfcf.uwaterloo.ca/equipment/main-cleanroom/deposition-1/sputter-nb-superconducting-films-plassys-sputter/authorized-materials); [Purdue BNC material compatibility](https://purdue.atlassian.net/wiki/spaces/BNCWiki/pages/6240554); [U-Mich LNF material restrictions](https://lnf-wiki.eecs.umich.edu/wiki/Material_Restrictions); [item24 semiconductor](https://www.item24.com/en-de/company/industry-solutions/semiconductor-industry)
- A Sandia hardware spec prohibits zinc, cadmium, electroplated tin and uncoated silver. It bans sulfur- or silicon-containing machining fluids and cites ASTM E595. This is aerospace practice, used here only as an analogue. — [Sandia JAS toolbox: materials](https://jastoolbox.sandia.gov/topic/mechanical-specification/design-constraints/materials/)

**ESD / antistatic materials**
- Commercial wafer-handling ESD materials are specified at 10⁶–10⁹ Ω. Examples are Bernstein ESD wafer tweezers and Perlast G90DM elastomer at 2×10⁶–1×10⁹ Ω volume resistance. The broader "dissipative" range is often quoted as 10⁴–10¹¹ Ω. Dissipative parts must be grounded to bleed charge. — [Simpex ESD wafer tweezer](https://www.simpex.ch/en/shop/production-technology/hand-tools/tweezers/esd-wafer-pinzette-130mm-form-6-dw-ohne-zaehne-ableitfaehig/); [Precision Polymer Engineering dissipative end-effector pads](https://www.prepol.com/downloads/dissipative-end-effector-pads); [Semiconductor Digest ESD](https://sst.semiconductor-digest.com/?p=62793)
- SEMI E163 is titled as a guide for handling reticles and other extremely electrostatic sensitive (EES) items in designated areas. No source tied it to a 10⁶–10⁹ Ω requirement for FOUP-contact parts. — [SEMI store equipment automation hardware](https://store-us.semi.org/collections/latest-standards/stdvol-equipment-automation-hardware?page=7) (excerpt)
- The Entegris A300 FOUP datasheet offers an optional ESD shell for charge-prone applications but gives no resistivity figure. — [Entegris A300 FOUP datasheet](https://www.entegris.com/content/dam/product-assets/a300foups/datasheet-a300foups.pdf) (excerpt)

**RoHS / REACH**
- Semiconductor and PV manufacturing equipment has relied on the RoHS 2 exclusion for "large-scale stationary industrial tools" (LSIT/LSSIT). Advantest, for example, declares its testers and handlers LSSIT-exempt. Exclusions are to be interpreted narrowly. — [Circuits Assembly: SEMI fights RoHS proposal](https://circuitsassembly.com/ca/editorial/menu-news/18998-semi-fights-eu-rohs-proposal-worst-case-scenario.html); [Advantest RoHS](https://www3.advantest.com/ko/sustainability/rohs); [IBF RoHS developments](https://www.ibf-solutions.com/en/seminars-and-news/news/current-developments-on-substance-restrictions-under-rohs)
- The REACH SVHC Candidate List is updated about twice a year, and each update triggers Article 33 communication duties. One vendor reports substances added on 2026-02-04. RoHS exemption 6(a), lead in free-machining steel, is reported to expire on 2026-12-11 for categories 8, 9 and 11. This is a secondary source; verify with ECHA and the EU. — [osapiens REACH & RoHS](https://osapiens.com/en/regulations/reach-and-rohs)

### Inferences
Proposed drawing-review checklist items. These are the researcher's synthesis, not quotes from standards.
1. **Specification callout present.** Every part has material = standard + grade + temper or condition, for example "KS D 6701 A6061-T6" or "ASTM A240 316L". Gas-wetted purge parts additionally state "SEMI F20 316L" and the surface finish, e.g. "SEMI F19, EP, Ra ≤ 0.25 µm" (number per customer spec).
2. **Wetted vs. non-wetted flag.** Purge N2/XCDA flow-path parts (nozzles, diffusers, manifolds, fittings) are marked wetted. They need cleanliness, packaging (double-bag, cleanroom-assembled) and leak or particle test notes. Non-wetted structural parts do not need EP.
3. **Plastics flammability.** Any plastic in the tool body or air-curtain duct carries an FM 4910 or UL 2360 listed grade, or is justified against the S2 §14 combustible-mass limit. Generic "PC", "PVC" or "ACRYL" without an FM-approved grade is flagged. UL 94 alone is not accepted by at least one major fab (Intel form).
4. **Restricted materials and finishes.** Flag zinc and zinc plating (electro-galvanized / Zn-Ni fasteners), cadmium, brass with Zn or Pb, and exposed copper or bare brass in the air path near wafers. Also flag uncontrolled silicone (grease, sealant, rubber), PVC or halogenated plastics where the customer restricts them, and adhesives or tapes inside the minienvironment. Prefer anodized Al, passivated or EP stainless, and PEEK/PTFE/PVDF/PFA.
5. **Outgassing.** Elastomers, adhesives, lubricants and coatings inside the EFEM or purge path need a low-outgassing grade or test evidence (E595-type TML/CVCM, or ISO 14644-15 / customer AMC test).
6. **ESD.** Parts touching FOUP or wafer (load-port kinematic pins, FOUP clamp pads, end-effector pads, purge nozzle seals contacting the FOUP port) are specified as static-dissipative with a resistivity range (commonly 10⁶–10⁹ Ω per vendor practice; confirm with the customer) and a ground path.
7. **Surface treatment callouts.** Anodize type and sealing (e.g. hard anodize, sealed or unsealed per customer), passivation (ASTM A967 / A380 [UNVERIFIED spec choice]), plating thickness. No ambiguous "도장" (paint) on cleanroom-internal parts without a particle-shedding check.
8. **RoHS/REACH declaration field.** The BOM or ERP item carries a compliance status even if the tool is LSIT-excluded, because customers commonly request declarations.

### Gaps
- Exact SEMI S2-0724 §14 text and any numeric material limits were not read (paywalled; semi.org fetch blocked).
- Official F19 Ra and defect acceptance numbers for the current (2015) edition were not verified. The Ra ≤ 0.25 µm figure is secondary.
- No SEMI standard was found that mandates 10⁶–10⁹ Ω for FOUP-contact parts. The range is industry or vendor practice and may come from customer specs or ANSI/ESD documents [UNVERIFIED].
- SEMI E-series cleanliness standards for FOUP or minienvironment parts were not located. SEMI E47.1 (FOUP mechanics) does not address materials. Customer specs (Samsung/SK hynix/TSMC supplier standards) are likely the real source and are not public.
- No authoritative semiconductor-industry "prohibited materials" list was found publicly. The zinc/cadmium/silicone bans come from university fab and aerospace analogues.

## Q2. Best practices for a controlled material/finish vocabulary between SolidWorks custom properties and ERP (property lists/dropdowns, sldmat library management, PDM data cards)

### Takeaway
Use a single canonical material master. Each record carries a canonical code, standard, grade and temper, the SW material name and the ERP code. Publish it in three forms:
- One read-only shared .sldmat custom library.
- PDM data-card lists sourced from SQL or Excel, or Property Tab Builder dropdowns.
- The ERP item master.

Do not let users type material text. The SolidWorks material should drive the "Material" property through a link, and other systems should read it.

### Cited Findings
- Custom material libraries are .sldmat files. Add their folder under System Options > File Locations > Material Databases. Avoid C:\Program Files. A shared server or PDM vault location keeps everyone on one version. — [CADimensions custom material library](https://knowledge.cadimensions.com/knowledge/how-can-i-create-a-custom-material-database-in-solidworks); [Solid Solutions custom material libraries](https://solidsolutions.co.uk/Blog/2021/09/Creating-custom-material-libraries-in-SOLIDWORKS)
- Reported pitfalls: editing a library pointed at the vault can create a new local .sldmat instead of updating the vault copy. Network shares cause latency and uncontrolled edits, so some companies copy the file locally at login. Read access for users and write access only for the maintainer is common. — [SOLIDWORKS forum thread 84337](https://forum.solidworks.com/thread/84337); [myCAD/Visiativ forum](https://forum.mycad.visiativ.com/t/comment-partager-une-bibliotheque-de-matieres-personnalises-entre-collegues/105316); [3DSwym vault material library](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/how-do-i-create-a-custom-materials-library-in-my-vault-for-all-editors-to-reference-one-library_T8V_Ao5RQPiEamvds5dJCA)
- SOLIDWORKS PDM card lists can be populated by a T-SQL query against an external database; the help cites a materials database as the example. Only the first result column is used. Lists are refreshed by the PDM database server service. — [SOLIDWORKS PDM help: card list from SQL](https://help.solidworks.com/2017/English/EnterprisePDM/Admin/t_create_SQL_list.htm); [Javelin: SQL card list (2024)](https://www.javelin-tech.com/blog/2024/09/creating-a-card-list-from-an-sql-database-in-solidworks-pdm/)
- Dynamically controlled SQL lists can make one list's choices depend on another, e.g. material category → grade. — [CADimensions dynamic SQL lists](https://knowledge.cadimensions.com/knowledge/how-to-set-up-dynamic-controlled-sql-card-lists-in-pdm)
- A Material property can be read from the model using `[SW-Material]` / `"SW-Material@Part.SLDPRT"`. Adding a drop-down list on that property breaks the link to the SOLIDWORKS material. Driving the SW material from a property value needs a third-party add-in or the API. — [myCAD forum: control material from properties form](https://forum.mycad.visiativ.com/t/piloter-la-matiere-piece-depuis-un-formulaire-de-proprietes-personnalisees/98053); [3DSwym linking to material custom property](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/linking-to-material-custom-property_I8hP3AT9TT2X2i-4h0d5Xw)
- In Toolbox, only one list property can be linked to the SOLIDWORKS Materials library. The mapping table pairs display values with SW material names, e.g. "Plain Carbon Steel = PCarSteel". — [SOLIDWORKS help: Creating Material Properties (Toolbox)](https://help.solidworks.com/2023/english/SolidWorks/toolbox/t_Creating_Material_Properties.htm)

### Inferences
Recommended governance model (researcher's synthesis):
- **Canonical master table.** Columns: `MAT_CODE` (ERP key, immutable), `CANONICAL_NAME` (e.g. `STS304`), `STANDARD` (`KS D 3698`), `GRADE` (`STS304`), `TEMPER/CONDITION` (`-T6`, `2B`, `annealed`), `EQUIV_JIS/ASTM/EN` (informational), `SW_MATERIAL_NAME` (exact .sldmat name), `CATEGORY` (metal/plastic/ceramic/elastomer), `WETTED_OK`, `FM4910`, `ESD_CLASS`, `STATUS` (active/obsolete/replaced-by).
- **Finish as a separate controlled field** (`SURFACE_TREATMENT`: e.g. `ANODIZE-BLACK`, `ANODIZE-CLEAR`, `HARD-ANODIZE`, `EP`, `PASSIVATE`, `NI-PLATE`, `NONE`). Never concatenate finish into the material string; for example, "NIKEL" plating belongs in finish, not in material.
- **Placeholder policy.** Use a single explicit token such as `N/A` (purchased or standard part) for parts without material. Ban `-`, `None`, empty strings and spaces. A validator treats blank or `-` as an error on manufactured parts and as allowed only when `PART_TYPE = PURCHASED`.
- **The .sldmat library is generated from the master.** Do not hand-edit it. One custom category per material family. Version-control it (PDM or git). Read-only for designers.
- **Property Tab Builder / PDM card.** The Material field is display-only and linked to `SW-Material`. Finish, heat treatment and compliance fields are dropdowns sourced from the same master via an SQL list.

### Gaps
- Official SOLIDWORKS help on Property Tab Builder dropdown sourcing and its interaction with SW-Material linking was not fetched. Forum evidence only.
- No neutral PLM/ERP master-data article with quantitative evidence on error rates was found.

## Q3. Was SS41 renamed (KS D 3503 SS275), and what is SM45C's status?

### Takeaway
Yes, with nuance. KS D 3503 was revised (sources cite December 2016, with editions labelled ":2018"). SS400 (formerly SS41 in the pre-SI era) became SS275 in a new yield-strength-based series: SS235, SS275, SS315, SS410, SS450, SS550. JIS G 3101 still uses SS400. So "SS41" is two generations obsolete and "SS400" is obsolete in KS but valid in JIS.

SM45C remains a current KS D 3752 grade; one list shows a revision dated 2019-10-29. JIS G 4051 calls it S45C.

### Cited Findings
- KS D 3503 has six grades, SS235, SS275, SS315, SS410, SS450 and SS550, under a revision dated 2016-12. SS275 replaces old SS400 and adds limits on C, Mn and Si; old SS400 specified only P and S. One seller reports SS275 tensile strength of 410/415–550 MPa and yield ≥ 275 MPa. JIS keeps SS400. — [steelmax: KS D 3503](https://steelmax.co.kr/?p=6814); [steelmax SS450 (old SS590)](https://steelmax.co.kr/?p=6824) (search excerpts; distributor source)
- Another listing cites "KS D 3503:2018 Rolled steels for general structure". The 2016 vs 2018 date conflict is unresolved: most likely a 2016 revision with a 2018 re-issue or confirmation [UNVERIFIED]. — [steelestores KS SS400](https://steelestores.com/grade/ks-ss400-ks-d-3503.html) (excerpt)
- SM45C is a KS D 3752 (machine structural carbon steel) grade, and JIS G 4051 calls it S45C. One standards list marks KS D 3752 current with a revision date of 2019-10-29. Chemistry: C 0.42–0.48, Si 0.15–0.35, Mn 0.60–0.90, P ≤ 0.030, S ≤ 0.035. KS and JIS specify chemistry only, not mechanical properties. — [steelmax S45C/SM45C](https://steelmax.co.kr/?p=136); [ndls.org.cn standard listing](https://ndls.org.cn/standard/detail/bfbc7a163f528b125ecfc318e11489b2) (excerpts)
- Similar renames are reported for welded structural steel, e.g. SM490 → SM355 under KS D 3515 [UNVERIFIED; only implied by a Total Materia "KS SM275D" entry]. — [Total Materia KS SM275D](https://www.totalmateria.com/en-us/material/1641163)

### Inferences
- SS41 → SS400 was the SI-unit conversion (kgf/mm² → N/mm²). This is general knowledge; no source was retrieved for it [UNVERIFIED date].
- Drawing rule: write the standard explicitly, e.g. "KS D 3503 SS275" or "JIS G 3101 SS400". Do not leave bare "SS400". Map legacy entries `SS41` and `SS400 (KS)` to `SS275` with an alias table, and keep `SS400` only when the JIS standard is stated.
- SM45C/S45C: keep one canonical name (e.g. `SM45C`, standard KS D 3752) and treat S45C as an alias. Heat treatment (e.g. H&T / 조질) goes in a separate field.
- Aluminium: KS D 6701 (plate) uses "A6061-T6" / "A6061P-T651" style designations [UNVERIFIED exact KS form]. T6 vs T651 (stress-relieved by stretching, typical for machined plate) matters for machining distortion. The master should store grade and temper separately and allow T651 for plate stock.
- Stainless: KS uses the STS prefix (KS D 3698/3705), JIS uses SUS, ASTM A240 uses "304/316L", and EN uses 1.4301/1.4404. In a KS-based company "SUS304" is a de-facto alias. Pick one canonical form and alias the rest.

### Gaps
- KS D 3503 original text and its exact revision dates (2016-12-xx vs 2018) were not verified on KSA/e-나라표준인증. The steelmax fetch was blocked.
- The KS D 3752 grade count (sources say 20 or 22 grades) and the latest revision were not confirmed.
- KS D 6701 exact temper designations (e.g. A6061P-T6 vs A6061-T651) were not confirmed.

## Q4. How to check consistency automatically (SW material vs custom property 'Material' vs BOM column vs ERP), and typical pitfalls

### Takeaway
Run a scripted validator (SolidWorks API / PDM task / Document Manager API) over each part and each configuration. It reads (1) the assigned SW material, (2) the file-level and configuration-specific "Material" custom properties (raw expression and evaluated value), (3) the drawing BOM cell, and (4) the ERP code via the mapping table. It flags any mismatch.

Common failure modes:
- Material assigned per configuration while the property is file-level.
- Property text pasted instead of a `"SW-Material@…"` link.
- Stale cached values in BOMs.
- Excel BOMs showing the literal "SW-Material".
- Library typos and duplicates producing names that don't map to ERP.

### Cited Findings
- Custom properties referencing SW-xxx values can show the same value for all configurations in a BOM. The BOM may read cached values that update only after the part is opened and the BOM refreshed. — [eng-tips: BOM showing incorrect info](https://www.eng-tips.com/threads/bom-showing-incorrect-info.333177); [SOLIDWORKS forum thread 63530](https://forum.solidworks.com/thread/63530) (excerpts)
- A BOM-column equation can read the material directly, but the result may carry a "$PRP:" prefix and reflect only the active configuration. — [3DSwym: BOM direct part property access](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/possibly-undocumented-feature-of-boms-direct-part-property-access-without-setting-up-document-properties_21yEx7QSSvuOHlQ_gSCi3w)
- Excel-based BOMs can display the literal text "SW-Material" (with quotes) instead of the evaluated material. — [SOLIDWORKS forum: Excel BOM shows SW-Material](https://forum.solidworks.com/forum-solidworks/ghuvLx8_S92gTC5TKjXtPA/excel-bom-table-shows-sw-material-instead-of-material-text)
- Configuration-specific link syntax is `"SW-Material@@Config@Part.SLDPRT"`, and the quotation marks are required. Related variants (e.g. SW-Treatment) did not work for one user. — [3DSwym: linking to material custom property](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/linking-to-material-custom-property_I8hP3AT9TT2X2i-4h0d5Xw) (2015, verify on current version)
- Quantity and other fields can be overridden manually in File > Properties > Custom, so the BOM shows wrong data. — [Solid Solutions: BOM quantities wrong](https://www.solidsolutions.co.uk/tech-support-blog-why-are-my-bom-quantities-wrong/)

### Inferences
**Validator rules** (proposed):
1. `SW_material` must exist in the master (`SW_MATERIAL_NAME`) and be non-empty for manufactured parts. Flag the default "Material <not specified>".
2. The `Material` property's raw expression must equal `"SW-Material@<file>"` (or the config-specific form). Flag any hard-typed value.
3. The evaluated `Material` must equal the SW material for every configuration. Flag configurations that differ from the file-level value.
4. The drawing BOM material cell must equal the evaluated property, and the title-block material must equal the BOM material for single-part drawings.
5. `MAT_CODE` from the mapping must exist and be active in ERP. Flag obsolete materials (e.g. legacy SS400 KS → SS275).
6. Finish must be in the finish vocabulary. Cross-rules: EP only on stainless; anodize only on Al; FOUP-contact parts must have `ESD_CLASS ≠ none`; plastics in tool body must have `FM4910 = Y` or a waiver.
7. Normalize before comparing: uppercase, trim, collapse spaces, remove dots ("P.C" → "PC"), then apply the alias table. Report fuzzy-match suggestions (Levenshtein ≤ 2) for unknown names.

**Normalization suggestions for the company list** (researcher judgement):

| As found | Issue | Suggested canonical |
|---|---|---|
| TITANUM | typo | `TITANIUM` + grade (e.g. `Ti Gr.2` / `Ti-6Al-4V (Gr.5)`) |
| NIKEL | typo; usually a plating, not a base material | `NICKEL` (if solid Ni 200/201), else move to finish `NI-PLATE` (electroless/electrolytic) |
| SILICON CARBIRD | typo | `SILICON CARBIDE (SiC)`; specify sintered/RB grade if relevant |
| PET (duplicate) | duplicate entry | single `PET`; if two variants exist, split by grade (e.g. `PET-ESD`) |
| SUS 310S (duplicate) | duplicate; space and prefix inconsistency | single `STS310S` (KS D 3698/3705) with aliases `SUS310S`, `SUS 310S`, `310S` |
| P.C | punctuation | `PC` (polycarbonate); add an ESD/FM-grade variant if used in the tool body |
| `-`, None, blank | placeholder | `N/A` only for purchased/standard parts; error otherwise |
| SS400 / SS41 | obsolete KS | `SS275 (KS D 3503)`; keep `SS400` only with JIS G 3101 |
| S45C / SM45C | alias pair | canonical `SM45C (KS D 3752)`, alias `S45C` |
| AL6061 / A6061 / 6061-T6 | form variants | `A6061-T6` or `A6061-T651` with temper mandatory |

**Pitfalls to list in the procedure:**
- Material set per configuration while the property is file-level.
- Derived or mirrored parts and weldment cut-lists that carry their own material properties.
- Toolbox and purchased parts with no material.
- Renaming a .sldmat entry orphans existing parts: the material name is stored in the part, so renames must go through an alias or migration.
- Local .sldmat copies diverging from the master.
- Drawings that use `$PRPSHEET:"Material"` while the BOM uses a different property name, e.g. "재질" vs "Material".

### Gaps
- Official SOLIDWORKS documentation on config-specific SW-Material evaluation in BOMs (current 2025/2026 behaviour) was not retrieved. Forum evidence only, some of it old.
- No published case study quantifying CAD↔ERP material mismatch rates was found.
