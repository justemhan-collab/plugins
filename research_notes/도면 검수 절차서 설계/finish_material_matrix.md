# FINISH × MATERIAL Compatibility Matrix (Surface finish / post-treatment vs. base material, drawing callout practice)

> Research date: 2026-10-28. Research environment note: direct page fetches were blocked for most hosts (mono.ipros.com, misumi-ec.com, finishing.com, anodizing.org were not resolvable from this session), so findings below are from search-result extracts of the cited pages. Spec numeric values should be re-verified against the purchased current revision before they are written into a procedure. Items tagged **확인 필요** are interpretations of Korean shop terms that no source confirmed.

## Q1. What each FINISH is, and compatibility with each material family

### Takeaway
Most company FINISH codes map cleanly to a material family: anodize variants → aluminum only; E.P (전해연마)/passivation → stainless; black oxide / parkerizing / zinc → ferrous only; electroless Ni and hard chrome → broad metallic use with caveats. Several Korean shop codes (C.P, E.P/POLYMER COATING, SILVER, 투명, 무정전, 열처리,강화) are ambiguous and must be fixed in a company code dictionary before a matrix can be enforced automatically.

### Cited Findings

**Anodize (BLACK-ANOD., WHITE-ANOD., Hard ANOD., 투명?, SILVER?)**
- Anodic oxide coating standards are scoped to aluminium and aluminium alloys: JIS H 8601 "specifies the requirements for the anodic oxide coatings on aluminium and aluminium alloys" and excludes hard anodic coatings, which are covered by JIS H 8603; JIS H 8601:2025 (dated 2025-12-22) is the current edition — [JSA preview JIS H 8601:2025](https://webdesk.jsa.or.jp/preview/pre_jis_h_08601_000_000_2025_e_ed10_ch.pdf); [standards.ie listing](https://shop.standards.ie/en-ie/standards/jis-h-8601-2025-629543_saig_jsa_jsa_3793171)
- JIS H 8601:1999 was a modified adoption of ISO 7599:1983 — [JSA handbook index](https://webdesk.jsa.or.jp/link/lib/eng_hb2024/index/313749.pdf)
- KS D 8301 "Anodic oxidation coatings on aluminium and aluminium alloys" is listed as current (2021 confirmation, 2001 original) — [ndls.org.cn listing](https://ndls.org.cn/standard/detail/5dde45d38130caa1f698b2dfb386c362)
- 7xxx alloys anodize gray / blue-gray / brown-black and mottled, depending on alloy and process; 5052 anodizes yellowish in thicker coatings; high-Si alloys anodize poorly because Si-rich areas remain unanodized, so low-Si/low-Fe casting alloys are recommended — [AAC Alloy & Temper](https://www.anodizing.org/alloy-temper); [AAC Specifying Anodized Finishes (PDF)](https://anodizing.org/wp-content/uploads/2024/09/specifying-anodized-finishes.pdf)
- A practitioner who hard-anodized 6061, 6082 and 7075 reported none reached true jet black and suspected that consistently black parts were dyed; another 7075-T6 lot came back "light marbled" after a seal not on the drawing was added — [finishing.com 549/81](https://finishing.com/549/81.shtml)
- A380 die-cast (Si > 8%) gives a dark, blotchy gray anodize that does not suit cosmetic use — [PTSMake anodizing guide](https://www.ptsmake.com/?p=10426)
- White anodize does not come from ordinary dyeing. It relies on light-scattering structures, and the white tone depends on electrolysis conditions (AC gave a more uniform white than DC in one report) — [TechConnect World 2024](https://www.techconnectworld.com/World2024/a.php?i=266); [KoreaScience oxalic anodizing](https://koreascience.kr/article/JAKO202032860595638.page?lang=ko)

**E.P (전해연마, electropolishing) / passivation**
- ASTM B912 is "Passivation of Stainless Steels Using Electropolishing", scoped to 200, 300 and 400 series and PH stainless steels. Passivation occurs at the same time as electropolishing, which also removes heat tint and scale — [iTeh ASTM B912-02(2018)](https://standards.iteh.ai/catalog/standards/astm/c47c1db6-46fa-4eee-9ebf-b6639dca72f6/astm-b912-02-2018)
- ASTM B912-26 is listed as current (published 1 Apr 2026) — [BSI Knowledge](https://knowledge.bsigroup.com/products/standard-specification-for-passivation-of-stainless-steels-using-electropolishing-1)
- ASTM A967 (chemical passivation of SS) and ASTM A380 are the passivation references, and SEMI F19 covers the wetted-surface finish of electropolished 316L components — [Harrison EP Industry Standards](https://harrisonep.com/resources/industry-standards)

**Black oxide (흑색산화피막)**
- MIL-DTL-13924D scope is ferrous metals only (wrought iron, carbon, low-alloy and corrosion-resistant steels). Class 1 is alkaline oxidizing for carbon/low-alloy steel, Class 2 is alkaline-chromate for some CRES tempered below 900°F, Class 3 is fused salt for CRES tempered at 900°F or above, and Class 4 is alkaline oxidizing for other CRES — [Scribd MIL-DTL-13924D](https://www.scribd.com/document/515535470/MIL-DTL-13924D). Class numbering is described differently by other sources — [EngineersEdge black oxide](https://engineersedge.com/black_oxide.htm); [Phoenix Plating chart](https://standardaero.com/wp-content/uploads/2025/06/Phoenix-Plating-Chart.pdf)
- Copper blackening falls under a different spec (MIL-F-495), not 13924. Black oxide gives limited corrosion protection and needs a supplementary oil or wax — [Wikipedia Black oxide](https://en.wikipedia.org/wiki/Black_oxide); [Engineers Edge](https://engineersedge.com/black_oxide.htm)

**PARKERIZING (인산염 피막)**
- MIL-DTL-16232 (current revision H, 2020) covers heavy manganese- or zinc-phosphate coatings on ferrous metals only. Manganese phosphate is dark gray to black and porous; zinc phosphate is light to dark gray — [Hubbard-Hall MIL-DTL-16232G guide](https://hubbardhall.com/document/mil-dtl-16232g-specification-guide?dl=1); [Wikipedia Phosphate conversion coating](https://en.wikipedia.org/wiki/Phosphate_conversion_coating)
- The process cannot be used on aluminum, brass or copper, and is unsuitable for stainless and high-Ni steels — [HMSC parkerizing PDF](https://homemetalshopclub.org/news/14/parkerizing.pdf)

**아연도금 (zinc electroplate)**
- ASTM B633 thickness classes: Fe/Zn 5 (SC1) is 5 µm, Fe/Zn 8 (SC2) is 8 µm, Fe/Zn 12 (SC3) is 12 µm, and Fe/Zn 25 (SC4) is 25 µm. Type II (colored chromate) is rated for at least 96 h salt spray. "Fe/" denotes an iron or steel substrate — [Scribd ASTM B633](https://www.scribd.com/document/519888626/B633); [Gleco zinc](https://glecoplating.com/electroplating-company/electroplating-zinc/)
- One supplier states that B633 Types II and III are hexavalent chromates — [Gleco](https://glecoplating.com/?p=148)

**무전해니켈 (electroless Ni)**
- An ASTM B733 callout has three parts. Type sets phosphorus content (I none specified, II 1–3%, III 2–4%, IV 5–9%, V >10%). Service condition sets minimum thickness (SC0 0.1, SC1 5, SC2 13, SC3 25, SC4 75 µm). Class sets heat treatment (1 = none beyond HE relief, 2 = ≥450°F hardening, 3 = 375°F adhesion bake for non-heat-treatable Al and Be) — [Gleco spec reference](https://glecoplating.com/?p=992); [EngineersEdge AMS 2404](https://www.EngineersEdge.com/ams2404.htm)
- NASA PRC-5007 lists minimum EN thickness by substrate: 0.0010 in for aluminum, 0.0015 in for ferrous, and 0.0005 in for Cu/Ni/Co alloys. On Al, pitting and roughness become problems above 0.0030 in — [NASA PRC-5007](https://www.nasa.gov/wp-content/uploads/2023/03/prc-5007-current.pdf)

**Hard-CR / Cr 도금 (chromium plating)**
- AMS-QQ-C-320 was cancelled in 2007 and superseded by AMS 2460 (now Rev B). AMS 2406 covers hard chromium specifically and has no class or type — [US Chrome](https://www.uschrome.com/ams-qq-c-320-and-ams-2460/); [Armoloy](https://armoloy.com/?p=6036)
- NASA PRC-5003 defines Class 2 engineering chromium as >0.002 in, with a maximum final thickness of 0.0050 in. Its sample note reads "CHROMIUM PLATE TO A FINAL THICKNESS AFTER GRINDING OF 0.0020-0.0050 INCHES PER NASA/JSC PRC 5003, CLASS 2". It requires a 23 h hydrogen bake-out for steel — [NASA PRC-5003](https://www.nasa.gov/wp-content/uploads/2023/03/prc-5003-current.pdf)
- A QQ-C-320 Class 1 example cites an 8 h minimum bake at 191±14°C after plating; the related HE relief spec is AMS 2759/9 — [NMFRC Q&A](https://nmfrc.org/crarchive/oct07b.php); [standards.ie AMS2460A](https://shop.standards.ie/en-ie/standards/sae-ams2460a-1021075_saig_sae_sae_2377881)
- Decorative black chrome is typically electroplated over bright or matte nickel — [Arlington Plating](https://www.arlingtonplating.com/?p=40)

**RAYDENT**
- Raydent is marketed as a low-temperature (below 0°C) electrochemical black treatment. Vendor sources disagree on its structure: an "alloyed diffusion layer" about 1 µm deep, a "resident film … also diffusing into the uppermost part of the base material", or ceramic particles forming a 1 µm layer. Stated thickness is 1–2 µm with "no geometry change" in one source and 3–5 µm in another. One Japanese supplier says oiling is needed for 70–90 days until the film stabilizes — [IPROS Raydent Treatment](https://mono.ipros.com/en/cg1/Raydent%20Treatment/); [Chiaravalli (Italian licensee) doc](https://chiaravalli.com/assets/df624954-a711-47f9-94c0-b9e12f323180); [drivehq datasheet](https://drivehq.com/file/df.aspx/isGallarytrue/shareID6477490/parentID26713225/fileID5853327342?1=1)
- Applications listed are linear-guide rails, bearings and fitting parts. An A5052 aluminum Raydent example exists ("Aluminum Raydent treatment" category). A comparison table lists SUS440C and SUS316L. The company name appears as "Raydent" and "Raident Industry Co., Ltd." — [IPROS Aluminum Raydent](https://mono.ipros.com/en/cg2/Aluminum%20Raydent%20treatment/); [worldtradeshow SEMICON Japan 2022](https://www.worldtradeshow.tv/english/?p=43892)
- HOJITSU Vietnam markets it as "Raydent low-temperature black chrome plating" — [Yellowpages VN](https://www.yellowpages.com.vn/listings/1187921271/hojitsu-black-chrome-plating-hojitsu-vietnam-co-ltd.html)
- A platers' forum says the process "seems to have very little connection with actual chrome plating" — [finishing.com 555/18](https://www.finishing.com/555/18.shtml)

**Vibrator (진동/배럴 연마)**
- In vibratory finishing, parts tumble with abrasive media to deburr and polish them, which rounds corners — [Lee Valley](https://leevalley.com/en-gb/discover/veritas-tools-qa/vibrating-polisher)
- Mechanical finishing of this kind (including ball burnishing) is offered for Al and stainless parts — [Bartholet](https://www.bartholet.swiss/en/mechanical-surface-finishing)

**무정전**
- Antistatic coatings are electrically conductive coatings that reduce static charging and are recommended for cleanrooms and electronics plants — [Coherent Market Insights (KR)](https://www.coherentmarketinsights.com/ko/정전기방지코팅시장-2210). No source defined the shop term "무정전" itself.

### Inferences

**Term identification (Korean shop codes). Items tagged 확인 필요 are not source-confirmed.**
| Code | Most likely meaning | Alternatives / note |
|---|---|---|
| BLACK-ANOD. / (반광) | Black-dyed sulfuric (Type II) anodize; 반광 = semi-gloss | Al only |
| WHITE-ANOD. / (무광) | White anodize (special process, not dye); 무광 = matte, probably bead-blasted before anodize | Al only. Colour consistency is vendor-dependent |
| Hard ANOD. | Type III hard anodize (MIL-A-8625 Type III / JIS H 8603) | Al only |
| 투명 | Clear (undyed) anodize **확인 필요** | Could also be a clear lacquer/coating on another substrate, e.g. on BRASS. Interpret by base material |
| SILVER | Natural/clear anodize described by colour (silver) on Al **확인 필요** | On COPPER/BRASS/OFHC it more likely means silver plating (은도금, ASTM B700 / AMS 2410). Interpret by base material |
| C.P | Chrome plating **확인 필요** | Redundant with "Cr 도금". Alternatives: Chemical Polishing (화학연마) or Chemical Passivation. Ask the drawing owner |
| Cr 도금 | Decorative/bright chrome (Ni-Cr) | vs Hard-CR = functional thick chrome |
| E.P | Electropolishing (전해연마) on SUS | "E.P" for electroless plating is a non-standard use and should be rejected |
| E.P/POLYMER COATING(10um) | **확인 필요**: either EP followed by a 10 µm polymer topcoat, or electrophoretic/electrodeposition painting (전착도장, E-coat ~10–25 µm) | The 10 µm figure fits E-coat. Confirm with the vendor |
| RAYDENT | Raydent proprietary low-temp black treatment (Raydent/Raident Industry, Japan) | Spec = vendor proprietary. Callout should name the process and color |
| Vibrator | Vibratory (barrel) deburr/finish | Mechanical, any metal; avoid on precision edges |
| Hairline | Directional brushed finish (HL), usually SUS/Al sheet | Mechanical |
| SANDING | Sand/bead blast (샌딩) | Mechanical; often a pre-treatment for matte anodize |
| POLISHING | Mechanical buffing/polish | Mechanical; give a Ra target |
| 탈지 | Degreasing/cleaning only | Process step, not a coating. For vacuum parts, call out precision cleaning |
| 무정전 | Antistatic (대전방지) coating, likely on PC/ACRYL/PET covers **확인 필요** | Could be "무정전기" ESD-safe coating; surface resistivity should be specified |
| 열처리,강화 | Heat treatment (quench/temper, hardness HRC) for steels; 강화 = chemically/thermally toughened glass **확인 필요** | Split into two codes |
| PAINT(분체)/(소부) | Powder coat / baked liquid paint | Metals (mostly steel, Al) |
| 무전해니켈 | Electroless Ni (ASTM B733 / AMS 2404 / MIL-C-26074) | Broad |
| 아연도금 | Zinc electroplate (ASTM B633 / KS D 8304 / JIS H 8610) | Ferrous only in practice |
| 흑색산화피막 | Black oxide (MIL-DTL-13924; Cu uses MIL-F-495) | Ferrous (and Cu with a different chemistry) |
| PARKERIZING | Mn/Zn phosphate (MIL-DTL-16232) | Ferrous only |
| -, None | No finish (as-machined) | Distinguish "intentionally none" from "blank" |

**Compatibility matrix (engineering synthesis from the findings above plus standard practice)**
Legend: ✅ normal · ⚠ possible with caveats · ❌ error / not applicable · M = mechanical finish (substrate-agnostic for metals)

| FINISH ↓ / MATERIAL → | Austenitic SUS (304/316L/310S/301) | Martensitic SUS (440C) / Ferritic (430) | Al 6061 | Al 5052 | Al 7075 | Cu alloys (BRASS, COPPER, OFHC) | Carbon/alloy steel (SM45C, SS41, SPCC, SCM435, SUJ2, SKD-11) | Cast iron FC300 | Ti | W, Mo | NdFeB | Plastics (PC, ACRYL, PEEK, POM/ACETAL, MC NYLON, TEFLON, PVC, PET, VESPEL, BAKELITE) | Glass/ceramic (QUARTZ, Pyrex, TEMPERED GLASS, BaF2, CERAMIC, SiC, BN, SILICON, Graphite) | Elastomers (VITON, NBR, URETHANE, RUBBER) |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Anodize (BLACK/WHITE/투명/Hard) | ❌ | ❌ | ✅ | ✅ (Hard: yellowish) | ⚠ colour mottling | ❌ | ❌ | ❌ | ⚠ Ti anodize exists but is a different process/spec (e.g. AMS 2488); do not reuse the Al code | ❌ | ❌ | ❌ | ❌ | ❌ |
| E.P (전해연마) | ✅ (B912) | ⚠ (B912 covers 400 series; finish poorer) | ⚠ Al EP exists but is uncommon; usually a code mismatch | ⚠ | ⚠ | ⚠ Cu EP exists, uncommon | ❌ (no passivity; rusts) | ❌ | ⚠ | ⚠ | ❌ | ❌ | ❌ | ❌ |
| Passivation (A967/A380) (not a company code; usually implied with SUS) | ✅ | ✅ (440C needs a specific method) | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ⚠ | ❌ | ❌ | ❌ | ❌ | ❌ |
| 흑색산화피막 | ⚠ CRES classes | ✅ CRES classes | ❌ | ❌ | ❌ | ⚠ different chemistry/spec (MIL-F-495) | ✅ | ⚠ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| PARKERIZING | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| 아연도금 | ❌ (pointless) | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ (HE bake for SCM435/SUJ2/SKD-11 hardened) | ⚠ | ❌ | ❌ | ⚠ (magnets: Ni-Cu-Ni more usual) | ❌ | ❌ | ❌ |
| 무전해니켈 | ⚠ activation needed | ✅ | ✅ (Class 3 bake optional) | ✅ | ✅ | ✅ | ✅ | ⚠ | ⚠ special pretreat | ⚠ | ✅ common | ⚠ plating-on-plastic only | ❌ | ❌ |
| Hard-CR | ⚠ | ✅ | ⚠ needs zincate/Ni underlayer | ⚠ | ⚠ | ✅ | ✅ (HE bake) | ⚠ | ⚠ | ⚠ | ❌ | ❌ | ❌ | ❌ |
| Cr 도금 / C.P (decor) | ⚠ | ⚠ | ⚠ | ⚠ | ⚠ | ✅ (BRASS classic) | ✅ | ⚠ | ❌ | ❌ | ❌ | ⚠ POP only | ❌ | ❌ |
| RAYDENT | ⚠ vendor claims SUS440C/316L | ⚠ | ⚠ vendor A5052 example | ⚠ | ⚠ | ? no data | ✅ (rails/bearings, e.g. SUJ2) | ? | ? | ? | ? | ❌ | ❌ | ❌ |
| SILVER (as plating) | ⚠ | ⚠ | ⚠ | ⚠ | ⚠ | ✅ | ⚠ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| PAINT(분체/소부) | ⚠ | ⚠ | ✅ | ✅ | ✅ | ⚠ | ✅ | ✅ | ⚠ | ❌ | ⚠ | ❌ (bake temp) | ❌ | ❌ |
| E.P/POLYMER COATING | depends on interpretation **확인 필요** | | | | | | | | | | | | | |
| 무정전 (antistatic) | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ✅ (PC, ACRYL, PET, PVC) | ⚠ glass | ❌ |
| Hairline / SANDING / POLISHING / Vibrator (M) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ⚠ brittle (W) | ⚠ brittle, corrodes | ⚠ POLISHING (ACRYL flame/buff) OK; Vibrator ⚠ | ⚠ POLISHING only (optical); Vibrator ❌ | ❌ |
| 열처리,강화 | ⚠ (austenitic not hardenable; solution anneal only) | ✅ 440C | ⚠ (T-temper is a material callout, not a finish) | ❌ (non-heat-treatable) | ⚠ temper | ⚠ anneal | ✅ SM45C/SCM435/SUJ2/SKD-11; ❌ SS41/SPCC (low C, not through-hardenable) | ⚠ | ⚠ | ⚠ | ❌ | ❌ (anneal only) | ✅ glass toughening (TEMPERED GLASS / 강화) | ❌ |
| 탈지 | ✅ any material (process step) | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ (solvent compatibility check) | ✅ | ⚠ solvent swell |

**Clear-error rules (auto-reject candidates)**
- Anodize (any ANOD code, 투명 interpreted as clear anodize) on any non-aluminum material.
- PARKERIZING or 흑색산화피막 on Al, Cu alloys, Ti, W, Mo, plastics, glass. Black oxide on Cu is a different spec and needs explicit review.
- 아연도금 on SUS, Al or Cu alloys, and on any part for vacuum or cleanroom use (see Q3).
- Any chemical or electrochemical finish on plastics, glass, ceramics or elastomers, except 탈지, 무정전 (plastics) and 강화 (glass).
- E.P on carbon steel / SPCC / FC300. E.P on Al: flag for review, since it is likely a code error even though Al EP is technically possible.
- 열처리 (hardening) on SS41, SPCC, Al 5052 or austenitic SUS: flag, because these are not hardenable by quench.

### Gaps
- No official Raydent datasheet was retrievable (raydent.co.jp not fetched). Applicable materials, thickness (1–2 vs 3–5 µm) and the corrosion claims are vendor-reported and conflicting.
- No source defined the Korean shop terms C.P, 무정전, 투명, SILVER, or E.P/POLYMER COATING(10um). All are **확인 필요** with the company's 도면 작성자 or finishing vendor.
- MIL-A-8625 and MIL-DTL-5541 primary texts were not retrieved.
- Al and Cu electropolishing specs were not researched in depth.

## Q2. Dimensional impacts and drawing note templates

### Takeaway
Conversion coatings that grow from the surface (anodize, especially hard anodize) consume about half their thickness into the substrate. Deposited coatings (EN, chrome, zinc) add their full thickness per surface, and roughly 4× that on thread pitch diameter. Callouts must state spec + type/class + thickness (min/max) + color + seal + masking/significant surfaces + HE bake where applicable.

### Cited Findings
- Type III hard anodize under typical conditions (200 g/L H2SO4, 24–40 ASF, ~0°C) grows about 50% outward and 50% inward. Conventional anodize is often described as about ⅓ inward and ⅔ outward — [finishing.com 74/29 Q&A](https://www.finishing.com/74/29.shtml)
- The AAC reference says the coating penetrates as much as it builds up, and that "thickness" includes both buildup and penetration — [AAC Anodizing Reference Guide](https://www.anodizing.org/anodizing-reference-guide/)
- A 50 µm hard anodize adds about 25 µm per surface. Type III growth is about 0.001″ per surface at nominal thickness — [Gleco Type II vs III](https://glecoplating.com/type-ii-vs-type-iii-anodizing/); secondary marketing source [Alibaba seller blog](https://seller.alibaba.com/blogs/2026/southeast-asia/aluminum-products/hard-anodizing-type-iii-coating-guide-alibaba-b2b)
- ISO 7599-derived classes (AA10/AA15/AA20/AA25) designate minimum thickness in µm. This is unverified against the JIS/KS tables — [search summary of JSA preview](https://webdesk.jsa.or.jp/preview/pre_jis_h_08601_000_000_2025_e_ed10_ch.pdf) (**확인 필요**)
- EN thickness is set by service condition (SC1 5 µm … SC4 75 µm). B733 leaves several items to the purchaser, including significant surfaces, stress relief and embrittlement relief, so a bare spec number is an incomplete callout — [Gleco](https://glecoplating.com/?p=992); [finishing.com 357/37](https://finishing.com/357/37.shtml)
- Example EN callout from a practitioner: "ASTM B733, SC1, Type I, Class 1 or 2" (0.2 mil min) — [finishing.com 390/60](https://www.finishing.com/390/60.shtml)
- Hard chrome is typically plated oversize and ground. The NASA note template is "CHROMIUM PLATE TO A FINAL THICKNESS AFTER GRINDING OF 0.0020-0.0050 INCHES PER NASA/JSC PRC 5003, CLASS 2" — [NASA PRC-5003](https://www.nasa.gov/wp-content/uploads/2023/03/prc-5003-current.pdf)
- Raydent is marketed with "no geometry change", but 1–2 µm and 3–5 µm figures both appear — [drivehq datasheet](https://drivehq.com/file/df.aspx/isGallarytrue/shareID6477490/parentID26713225/fileID5853327342?1=1); [IPROS](https://mono.ipros.com/en/cg1/Raydent%20Treatment/)
- A 7075 anodize lot changed colour because the vendor added a seal not on the drawing, which shows why seal type should be explicit — [finishing.com 549/81](https://finishing.com/549/81.shtml)
- ASTM B633 does not address thread allowance in the retrieved material — [Scribd B633](https://www.scribd.com/document/519888626/B633)

### Inferences
- **Thread/fit rule (standard engineering practice, not source-retrieved):** for 60° threads, pitch-diameter change ≈ 4 × coating thickness per surface. 10 µm EN therefore changes PD by about 40 µm, which consumes most of a 6g/6H allowance. Mask threads or specify "dimensions apply after plating" / "before plating". For hard anodize, buildup is about half of the thickness, so a 50 µm hard anodize changes PD by about 100 µm. Threads must be masked or cut oversize.
- **Drawing note templates** (adapt the spec revision; company KS/JIS equivalents in brackets):
  - Type II black anodize: `ANODIZE PER MIL-A-8625 TYPE II, CLASS 2 (DYED), BLACK, 8–12 µm, HOT-WATER/NI-ACETATE SEAL. [KS D 8301 / JIS H 8601 AA10 동등] MASK: Ø__ BORE, THREADS, ELECTRICAL GROUND PADS. DIMENSIONS APPLY AFTER FINISH.`
  - Hard anodize: `HARD ANODIZE PER MIL-A-8625 TYPE III, CLASS 1, 50 ±10 µm, UNSEALED (or SEALED), [JIS H 8603]. ~50% BUILDUP. MASK THREADS AND ⌀__H7 FITS.`
  - Clear anodize (투명): `ANODIZE PER MIL-A-8625 TYPE II, CLASS 1 (NON-DYED), CLEAR, SEALED.`
  - Electroless Ni: `ELECTROLESS NICKEL PER ASTM B733 (or AMS 2404), TYPE IV (MID-P) / TYPE V (HIGH-P), SC2 (13 µm MIN), CLASS 1; HE RELIEF BAKE PER AMS 2759/9 FOR STEEL ≥ HRC 35 (SCM435/SUJ2/SKD-11 hardened). DIMENSIONS APPLY AFTER PLATING.`
  - Hard chrome: `HARD CHROMIUM PLATE PER AMS 2460 (or AMS 2406), FINAL THK 50–125 µm AFTER GRINDING, BAKE PER AMS 2759/9.`
  - Zinc: `ZINC PLATE PER ASTM B633, Fe/Zn 8, TYPE III (TRIVALENT CLEAR) [KS D 8304 / JIS H 8610 equivalent].` Specify trivalent for RoHS/hexavalent-Cr avoidance.
  - Black oxide: `BLACK OXIDE PER MIL-DTL-13924, CLASS 1 (carbon steel) / CLASS 4 (CRES) — verify class per current revision; SUPPLEMENTARY OIL.`
  - Phosphate: `MANGANESE PHOSPHATE PER MIL-DTL-16232, TYPE M, CLASS __ (supplementary treatment).`
  - EP/passivation: `ELECTROPOLISH PER ASTM B912; Ra ≤ __ µm (wetted surfaces per SEMI F19); PASSIVATE PER ASTM A967 (Citric/Nitric) if not EP.` Note that EP removes material (dimensions apply after EP).
  - Raydent: `RAYDENT TREATMENT (RAYDENT INDUSTRIAL / licensee), BLACK, NO DIMENSIONAL ALLOWANCE REQUIRED PER VENDOR — 확인 필요.`
- Notes should always state which surfaces are significant and which are masked, and whether dimensions apply before or after finishing.

### Gaps
- Exact MIL-A-8625 thickness ranges by Type/Class, and the JIS H 8601 / KS D 8301 class tables, were not retrieved.
- No source quantified EP material removal. Typical practice is a few to ~25 µm, but this is unsourced.
- The KS D 8304 / JIS H 8610 zinc equivalence is from general knowledge and was not verified.

## Q3. Semiconductor-equipment considerations (outgassing, particles, cleanroom)

### Takeaway
For vacuum or cleanroom parts, avoid zinc and cadmium entirely, treat anodize as a water-outgassing source (sealing helps but does not eliminate it), and specify EP + passivation with a Ra limit for 316L wetted parts. Mechanical finishes and degreasing alone need a precision-cleaning callout.

### Cited Findings
- NASA EEE parts guidance prohibits zinc because it sublimates in vacuum, especially at elevated temperature, leaving conductive deposits, and because of zinc whiskers. Cadmium likewise sublimates in hard vacuum, especially above 75°C, and can short circuits or contaminate optics — [NASA NEPP zinc prohibition](https://nepp.nasa.gov/npsl/Prohibited/zinc_prohibition.htm); [NASA NEPP cadmium prohibition](https://nepp.nasa.gov/npsl/Prohibited/cadmium_prohibition.htm)
- NASA EEE-INST-002 (as quoted by Glenair) says Cd, Zn, chemically coated Cd/Zn, or silver "shall not be used as a connector or contact finish"; EN for shells is the suggested alternative — [Glenair space-flight connectors](https://ww.glenair.com/powertrip-connectors/pdf/b/powertrip-connectors-for-space-flight.pdf)
- Anodized vacuum parts are discouraged because the oxide traps water that later outgasses — [Wikipedia Materials for use in vacuum](https://en.wikipedia.org/wiki/Materials_for_use_in_vacuum)
- Using O'Hanlon's data, unbaked anodized Al outgasses about 41× plain Al at 1 h and about 71× at 10 h — [Ideal Vacuum white paper](https://www.idealvac.com/files/manuals/White-Paper-Welded-Vac-Chamber.pdf)
- Mitigations for anodize outgassing include fine-grain substrate, nickel-acetate sealing, post-bake, and denser alternatives (EN, sol-gel). The porous oxide retains water, dyes and residues — [Finishing & Coating outgassing mitigation](https://finishingandcoating.com/index.php/anodizingcat/2875-outgassing-mitigation-strategies-in-anodized-aluminum)
- Sealing (boehmite) is debated: one view is that it is stable at room temperature, another is that porosity creates virtual leaks — [eng-tips](https://www.eng-tips.com/threads/outgassing-of-anodized-aluminum.73481); [PhysicsForums](https://www.physicsforums.com/threads/help-solving-a-uhv-problem-anodized-aluminum.1011917)
- Specialized sulfuric/oxalic anodizing has been developed for plasma process chambers to reduce cracks and particles at high temperature — [J-STAGE J. Vac. Soc. Jpn.](https://www.jstage.jst.go.jp/article/jvsj1958/45/5/45_5_438/_article/-char/en)
- SEMI F19 defines the wetted-surface finish of electropolished 316L components. EP thickens and improves the passive oxide — [Harrison EP standards](https://harrisonep.com/resources/industry-standards)
- Black oxide gives limited corrosion protection and relies on supplementary oil or wax. Raydent vendors recommend oiling for 70–90 days — [Wikipedia Black oxide](https://en.wikipedia.org/wiki/Black_oxide); [IPROS Raydent](https://mono.ipros.com/en/cg1/Raydent%20Treatment/)

### Inferences
- **Cleanroom / vacuum red-flags for the checklist:**
  1. 아연도금 (and any Cd) on parts inside the vacuum or process envelope: reject.
  2. 흑색산화피막 / PARKERIZING / RAYDENT: these depend on oil or porous films, which are a hydrocarbon and particle risk. Flag for vacuum or wafer-proximate parts.
  3. Dyed BLACK-ANOD. in vacuum: dyes and water outgas. Prefer undyed hard anodize with a defined seal, or EN, and require a bake.
  4. PAINT(분체/소부) inside the vacuum: flag for outgassing.
  5. Vibrator / SANDING: embedded media can generate particles. Require precision cleaning after the operation.
- For SUS 316L gas or wetted parts: `EP PER ASTM B912 + SEMI F19, Ra ≤ (customer value), PRECISION CLEAN & DOUBLE-BAG`. The Ra numbers must come from the customer spec (not retrieved).
- 탈지 alone is not a cleanliness specification. Vacuum parts need a precision-cleaning level (e.g. UHV cleaning), and the governing customer spec is **확인 필요**.

### Gaps
- SEMI F19 numeric Ra / surface-defect limits were not retrieved (paywalled).
- No peer-reviewed comparison of sealed vs unsealed anodize outgassing for semiconductor chambers was found.
- No cleanroom particle-generation data for Raydent or black oxide was found.
