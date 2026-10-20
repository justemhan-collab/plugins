# Open-source parsing and checking of 2D engineering drawings from DXF/DWG and PDF (excluding native SOLIDWORKS)

Researched 2026-10-20. Method notes:
- Repo metadata (stars, `updated_at`) comes from the GitHub search API on 2026-10-20. `updated_at` is not the same as the last commit date. Direct per-repo API calls (`pushed_at`) and fetches from arxiv.org, mdpi.com, ezdxf docs and help.solidworks.com were blocked in this environment (DNS/proxy). Some facts therefore come from search-engine summaries of those pages, and each one is marked where it applies.
- "UNVERIFIED" means the claim could not be confirmed against the primary page.

## Q1. Which repos read DXF/DWG for drawing QA, and how do they handle DIMENSION, ATTRIB, ACAD_TABLE, TOLERANCE and paperspace layouts?

### Takeaway
There is no mature open-source DXF/DWG drawing-QA checker. The repos that exist are small (0–10 stars) and mostly from 2026. The reusable foundation is ezdxf plus a DWG→DXF converter: ODA File Converter through the `ezdxf.addons.odafc` wrapper, or LibreDWG. The rule engine (dimension override checks, title-block attribute checks, layer checks) would have to be built in-house.

### Cited Findings
**Core libraries**
- **mozman/ezdxf**: "Python interface to DXF". About 1,482 stars and 290 forks. MIT license per long-standing PyPI/README (not re-verified today). `updated_at` 2026-10-19, so it is actively maintained. Docs are at version 1.4.x (1.4.4 stable, 1.4.5b1 dev). — [GitHub search API result](https://github.com/mozman/ezdxf); [ezdxf docs](https://ezdxf.readthedocs.io/en/stable/tutorials/linear_dimension.html)
- How ezdxf handles DIMENSION entities:
  - `dimension.get_measurement()` returns the true value computed from the definition points.
  - `dimension.dxf.text` holds the text as it was rendered or overridden when the dimension was created.
  - In `dxf.text`, `""` or `"<>"` means "show the real measurement", and `" "` (one space) suppresses it. Any other string is an override, which can embed `<>` to keep the measurement together with a prefix or suffix.
  - Comparing `get_measurement()` against the parsed `dxf.text` is therefore the standard way to detect overridden or "faked" dimension text. — [ezdxf: Get Content From DXF Entities](https://ezdxf.readthedocs.io/en/stable/tasks/get_entity_content.html); [ezdxf linear dimension tutorial](https://ezdxf.readthedocs.io/en/stable/tutorials/linear_dimension.html) (via search summary)
- Tolerances on DIMENSION entities in ezdxf are stored in DIMSTYLE variables or in per-entity XDATA overrides (`dimtol`, `dimtp`, `dimtm`, plus `dimlim` for limits). They are read with `dimension.override()` and the resulting DimStyleOverride's `dimtol` / `dimtp` / `dimtm`. "dimtm defines the lower tolerance value if present, else the lower tolerance value is the same as the upper." — [ezdxf DimStyle docs](https://ezdxf.readthedocs.io/en/stable/tables/dimstyle_table_entry.html); [DXF Content howto](https://ezdxf.mozman.at/docs/howto/content.html) (via search summary)
- ACAD_TABLE support in ezdxf is read-only. `ezdxf.entities.acad_table.read_acad_table_content()` returns the cells as a list of rows of strings. "If the row or column count is missing, the full content lands in the first row." ACAD_TABLE "has only limited support to preserve the entity. There is no support for adding a new ACAD_TABLE entity or modifying its content", and full support was judged "very unlikely due to the enormous complexity". The TablePainter add-on is unrelated: it draws tables out of lines and text. — [ezdxf get_entity_content](https://ezdxf.readthedocs.io/en/stable/tasks/get_entity_content.html); [ezdxf TablePainter](https://ezdxf.readthedocs.io/en/stable/addons/tablepainter.html) (via search summary)
- DWG input in ezdxf goes through the `odafc` add-on. ezdxf "can not convert between different DXF versions" and recommends the free ODA File Converter. The add-on wraps a user-installed ODA binary, "converts a DWG/DXB/DXF file into a temporary DXF file and loads this file by ezdxf". `unix_exec_path` supports a Linux or macOS binary or AppImage. The docs warn about the security risk of an external executable whose path can be changed. — [ezdxf odafc add-on](https://ezdxf.readthedocs.io/en/stable/addons/odafc.html); [ezdxf introduction](https://ezdxf.readthedocs.io/en/stable/introduction.html)
- **LibreDWG/libredwg** is a GPL C library ("Official mirror… CI hooks and nightly releases") with about 1,631 stars and `updated_at` 2026-10-19. OSArch describes it as "stable for the most common entities and objects", and FreeCAD says it lacks "support for some DWG entities". No source found addresses DIMENSION fidelity specifically. — [GitHub](https://github.com/LibreDWG/libredwg); [OSArch wiki](https://wiki.osarch.org/index.php/Drawing_(DWG)); [FreeCAD wiki](https://wiki.freecad.org/Translations:Installing_additional_components/74/en)

**Small repos that apply these libraries**
- **aeewws/ocrx-engineering-drawings**: "Windows-first OCR and CAD extraction workflow". 10 stars, MIT, `updated_at` 2026-10-12, created 2026-04.
  - DXF path: direct text extraction of `TEXT`, `MTEXT`, `ATTRIB`, `ATTDEF`, `DIMENSION` and `MLEADER`, then OCR (PaddleOCR GPU, with RapidOCR as fallback) on the remaining visuals, merging the two text sources "conservatively".
  - DWG is converted via ODA File Converter, which is not bundled.
  - PDF follows "native PDF parse first, OCR second".
  - Outputs are `.ocr.txt` and `.ocr.json`.
  - It does no rule checking. — [GitHub](https://github.com/aeewws/ocrx-engineering-drawings)
- **MatheusMeM/dwg-interpreter-claude-skill**: a Claude Code skill that reads DWG on Windows without AutoCAD (LibreDWG + ezdxf). It extracts JSON covering "layouts, viewports with scale, layers, blocks, title block, texts and dimensions". 0 stars, created 2026-08. This is a good reference for paperspace/layout and viewport-scale inventory. — [GitHub](https://github.com/MatheusMeM/dwg-interpreter-claude-skill)
- **JMcQB/CAD-Title-Block-Extractor**: "Extract Title Block information from DXF CAD Files". 0 stars, created 2026-03. Content was not inspected (UNVERIFIED). — [GitHub](https://github.com/JMcQB/CAD-Title-Block-Extractor)
- **darcreeper1-collab/autocad-dxf-pdf-batch-exporter**: DXF/DWG→PDF batch export through AutoCAD COM, with "frame detection, title-block ordering, and verification". 1 star. Requires AutoCAD, so it is not a pure-OSS parser. — [GitHub](https://github.com/darcreeper1-collab/autocad-dxf-pdf-batch-exporter)
- **Eklavya-San/CTQ-extraction**: an AI-agent skill that extracts CTQ parameters, GD&T FCFs and "verbatim tolerances" from drawings or CAD files into JSON (ISO 1101 topic). 3 stars, created 2026-08. It is LLM-driven. — [GitHub](https://github.com/Eklavya-San/CTQ-extraction)
- GitHub repository searches for "dxf drawing checker layer standard", "ezdxf dimension check drawing" and "dxf title block" returned no dedicated QA or checker projects beyond the ones listed above. — GitHub search API, 2026-10-20

### Inferences
- **Reusable component list for the team:**
  1. `ezdxf` for reading. Use `doc.layouts` and `doc.paperspace(name)` per sheet, then INSERT with `.attribs` for title-block ATTRIB values, DIMENSION with `get_measurement()`, `dxf.text` and `override()`, TOLERANCE entities (`dxf.content` holds the FCF in a coded string format), MTEXT `plain_text()`, and `read_acad_table_content()`.
  2. `ezdxf.addons.odafc` with ODA File Converter for DWG, which is the most robust route. LibreDWG's `dwg2dxf` is the GPL alternative.
  3. ocrx's "vector text first, OCR fallback, merge" routing is a sensible pattern to copy.
- Title-block checks in DXF depend on whether the exporter wrote ATTRIBs (block attributes) or plain TEXT. SOLIDWORKS sheet-format notes linked to custom properties typically arrive as TEXT/MTEXT, not ATTRIB (UNVERIFIED; test on a sample export). Expect position-based (zone) parsing.
- Overridden-dimension detection works only if the DXF keeps DIMENSION entities. If the export explodes them into lines and text, this check is impossible from the DXF alone.

### Gaps
- Could not fetch ezdxf docs pages directly. TOLERANCE-entity parsing helpers and MLEADER content in ezdxf were not verified.
- No source on LibreDWG vs ODA fidelity for DIMENSION/ACAD_TABLE.
- Last-commit dates (`pushed_at`) and licenses of the small repos are unverified, apart from those quoted above.
- No open-source "layer standard / CAD standards checker" (in the style of AutoCAD's CHECKSTANDARDS) was found for Python.

## Q2. Known pitfalls of SOLIDWORKS → DWG/DXF export

### Takeaway
There is documented evidence of four problems:
- a single scale factor for all views
- text turned into polylines or geometry, depending on font/export settings
- BOM and other tables exported as blocks with clashing names rather than as usable tables
- hatches that are non-associative

No direct forum evidence was found on DIMENSION entities being exploded. DXF should be treated as a lossy secondary input, and the SOLIDWORKS API should stay the source of truth for dimensions and BOM.

### Cited Findings
- On export, every view gets one common scale factor. "If a SOLIDWORKS drawing includes views of different scales, some of them will inevitably measure incorrectly when converted." Enabling 1:1 output is not enough on its own, because the result also depends on the selected base scale. — [Javelin: Selecting Base Scale for Exporting SOLIDWORKS Drawings to DXF/DWG](https://www.javelin-tech.com/blog/2015/03/selecting-base-scale-for-exporting-solidworks-drawings-to-dxf-dwg/)
- The Export Options dialog can write all sheets to one file in paper space or to separate files (each with a number prefix). Crosshatch is exported as non-associative hatch. — [SOLIDWORKS help: DXF/DWG Files](https://help.solidworks.com/2023/English/Solidworks/sldworks/c_DXF-DWG_Files_dxf_dwg-Files.htm); [Export options 2016](https://help.solidworks.com/2016/english/SolidWorks/sldworks/HIDD_EXPORT_OPTIONS_DXF.htm) (via search summary)
- There is an export option to put drawing-view geometry into blocks. — [Javelin 2026: How to Export SOLIDWORKS Drawings to DXF/DWG](https://www.javelin-tech.com/blog/2026/04/how-to-export-solidworks-drawings-to-dxf-dwg/) (via search summary)
- BOM and other tables are exported as block references, and "the block names for the BOM Table and other tables are the same", so copying one DWG into another overwrites table contents. The workaround was to convert them manually to plain text and lines. — [SOLIDWORKS forum thread 225774](https://forum.solidworks.com/thread/225774) (via search summary)
- A note created as sketch text was exported as polyline segments, and a CAM tool that reads only TEXT entities could not use it. — [SOLIDWORKS forum thread 185434](https://forum.solidworks.com/thread/185434)
- Changing the export font setting from "AutoCAD Standard" to TrueType keeps text as text with the font preserved. — [CamBam forum topic 8104](https://cambamcnc.com/forum/index.php?topic=8104.0)
- Spline→polyline options, R2013 DXF version, open versus closed polylines, and polyline-merge failures are documented in several threads. — [GrabCAD](https://grabcad.com/groups/solidworks-tutorials-on-demand/discussions/best-way-to-export-dxf-for-cnc-lasercut); [OpenBuilds](https://builds.openbuilds.com/threads/open-lines-solved-solidworks-how-to-closed-polyline-dxf.21079/); [Autodesk forum](https://forums.autodesk.com/t5/autocad-forum/polyline-issues-when-exporting-dxf-from-solidworks/td-p/6988578)
- The BOM data itself can be saved from the native table as .xlsx, .csv or .txt, which avoids the DWG table problem. — [cadshift blog](https://cadshift.com/blog/solidworks-bom-to-excel-native-macro-pdm/) (via search summary)

### Inferences
- Treat DXF BOM as "block containing TEXT plus LINE grid", not ACAD_TABLE. Parse it by geometric cell reconstruction (line grid to cells, then text-in-cell), or skip it in favour of SOLIDWORKS API BOM export.
- If the team needs DXF dimension checks, standardize export settings (TrueType fonts, paper space, one sheet per file, a known base scale) and validate on samples whether DIMENSION entities survive with `<>` intact.
- Multi-scale sheets mean that coordinates measured in model space or paperspace may not equal true lengths. Rely on `get_measurement()` with the DIMLFAC scale factor, or on the SOLIDWORKS API instead.

### Gaps
- No primary documentation found on whether SOLIDWORKS writes associative DIMENSION entities, or how it handles dual dimensions, tolerance stacks and GD&T FCFs (TOLERANCE entity versus exploded geometry) in DXF. This must be tested empirically.
- Mapping of custom properties to ATTRIB versus TEXT is unverified.

## Q3. PDF drawing parsing: eDOCr/eDOCr2, werk24 and other OCR pipelines; reported accuracy

### Takeaway
eDOCr2 (MIT) is the strongest open-source baseline for raster or PDF mechanical drawings:
- dimension recall 93.75%
- FCF recall 100%
- dimension CER 0.73%
- FCF CER 5.7%

Its tables and title block are only bulk-OCR'd with Tesseract, with no label/value semantics. werk24 is the most complete extractor (title block, BOM, revision table, GD&T), but it is commercial and pay-as-you-go. Newer academic work uses YOLOv11-OBB plus Donut/Florence-2 VLMs and reports F1 of about 97%, but on in-house datasets, and title blocks remain weak.

### Cited Findings
- **javvi51/edocr2** (eDOCr2, Javier Villena Toro): "segmentation and OCR on engineering drawings", mainly mechanical and production drawings. Includes training scripts for synthetic recognizers and detectors, and `test_all.py` for benchmarking. Models are downloaded from Releases. MIT license, 81 stars, 19 forks, 4 open issues, 19 commits, `updated_at` 2026-10-14, created 2024-09. Paper preprint: SSRN 10.2139/ssrn.5045921. — [GitHub](https://github.com/javvi51/edocr2)
- **eDOCr2 paper**: Toro & Tarkian, "Optimizing Text Recognition in Mechanical Drawings: A Comprehensive Approach", *Machines* 2025, 13, 254. Key points:
  - It segments drawings into information blocks, dimensions and feature control frames.
  - It reports "text recall of 93.75% and CER below 1%" on drawings from different sources.
  - Results table: eDOCr2 has 93.75% dimension recall, 100% FCF recall, 5.70% FCF CER and 0.73% dimension CER. The GPT-4o variant has 87.80%* dimension recall ("conditioned, since the model excludes repeated dimensions") and 1.83% dimension CER.
  - GD&T is handled by a CRNN trained to output UTF-8 GD&T symbols "as another character".
  - Tables are handled by Pytesseract, which "returns all textual elements without distinguishing between labels, title blocks, and the actual important information".
  - It also integrates Qwen2-VL-7B and GPT-4o for verification. — [MDPI](https://www.mdpi.com/2075-1702/13/3/254); [DiVA full text](https://www.diva-portal.org/smash/get/diva2:1950437/FULLTEXT01.pdf) (via search summary)
- **eDOCr v1**: Frontiers in Manufacturing Technology, 2023. Results: detection 90% precision and 90% recall, recognition F1 94%, CER 8%. It uses separate pipelines for the information block, GD&T (a dual recognition model) and dimensions (including a tolerance checker). — [Frontiers 10.3389/fmtec.2023.1154132](https://frontiersin.org/journals/manufacturing-technology/articles/10.3389/fmtec.2023.1154132/text)
- eDOCr v1 is "a packaged version of keras-ocr" for mechanical drawings. It installs with `pip install eDOCr` (Python ≥3.6, TF ≥2.0), comes from javvi51/eDOCr, and runs as `ocr_it.py drawing.pdf`. A fork lists Python 3.9 and TF 2.13. Current repo status is UNVERIFIED: the repo did not appear in GitHub search today. — [PyPI eDOCr](https://pypi.org/project/eDOCr); [chrisyucode/eDOCr fork](https://github.com/chrisyucode/eDOCr)
- **gsdali/coreml-edocr2**: eDOCr2 converted to CoreML for Apple Silicon. Swift, MIT, 2 stars, created 2026-04. — [GitHub](https://github.com/gsdali/coreml-edocr2)
- **werk24** (W24-Service-GmbH/werk24-python): "Automated Processing of Technical Drawings". 93 stars, `updated_at` 2026-10-16.
  - Extracts drawing ID, part ID, designation, general tolerances and roughness, material, weight, BOM (including fasteners), revision table, notes, dimensions with tolerances, threads, bores, chamfers, roughness, GD&T, radii and more, as well as manufacturing-method insights and redaction.
  - Uses an API token and is billed pay-as-you-go. The client requires a commercial license (LICENSE.txt), and it is not open source.
  - The Node.js and Java clients are archived. — [GitHub](https://github.com/W24-Service-GmbH/werk24-python); [archive org](https://github.com/W24-Service-GmbH-Archive/werk24-java)
- **arXiv 2505.01530**: YOLOv11 OBB detector plus Donut transformer. Reported GD&T precision 94.77%, recall 100% for most categories and F1 97.3%, on an in-house dataset that is not comparable to eDOCr. — [arXiv 2505.01530](https://arxiv.org/abs/2505.01530v3) (via search summary)
- **arXiv 2506.17374** ("From Drawings to Decisions", M. T. Khan et al.): YOLOv11-OBB, then Donut/Florence-2. Uses 1,367 mechanical drawings annotated in nine categories. Title-block figures were not retrieved. — [arXiv PDF](https://arxiv.org/pdf/2506.17374)
- **arXiv 2510.21862** (multi-stage VLM framework, overlapping authors): zero-shot Donut has F1 0.533 on title blocks and 0.810 on notes, with hallucination rates of 0.478 and 0.319. This shows title-block extraction remains hard. — [arXiv 2510.21862](https://arxiv.org/pdf/2510.21862) (via search summary)
- Open-source VLM title-block extraction with Qwen-VL appears in the EC3 2026 paper "Open-Source VLMs for Engineering Drawing Metadata: QWEN-VL Title-Block Detection and Information Extraction". Accuracy was not retrieved. — [EC3 2026](https://ec-3.org/publication/ec32026_252/)
- **Bakkopi/engineering-drawing-extractor**: OpenCV + pytesseract + openpyxl. Separates the drawing region from tables, removes lines and borders, OCRs title-block fields (drawing no., author, title) and writes them to Excel. 96 stars, 16 forks, 4 commits, no license shown, no accuracy reported, fixed file-naming conventions, `updated_at` 2026-09-21. — [GitHub](https://github.com/Bakkopi/engineering-drawing-extractor)
- Other small OCR repos (each 1–8 stars, created 2022–2026):
  - Tesseract region and table extraction — [saurabhkovoor/EngineeringDrawingAndTextExtraction](https://github.com/saurabhkovoor/EngineeringDrawingAndTextExtraction)
  - [thedatasense/PaddleOCR_Engineering_Drawings](https://github.com/thedatasense/PaddleOCR_Engineering_Drawings)
  - YOLO + PaddleOCR/Tesseract/Gemini — [SHYam1025/DrawVision-AI](https://github.com/SHYam1025/DrawVision-AI)
  - pdfplumber coordinate-based title-block fields (part name, material, drawing no.) from CAD-exported vector PDFs — [Night-E-ye/Drawing_ocr_extractor](https://github.com/Night-E-ye/Drawing_ocr_extractor)
  - [kookeetong/gdt-parser](https://github.com/kookeetong/gdt-parser)
  - AS9102 FAIR generator that extracts dimensions and GD&T — [SaddasSL/openfair](https://github.com/SaddasSL/openfair)
  - revision change detection — [ramugopal92/engineering-drawing-change-detection](https://github.com/ramugopal92/engineering-drawing-change-detection)
  - PDF + OCR + VLM + deterministic rule validation against specs (local Ollama) — [1378/Construction-PDF-Validation-AI](https://github.com/1378/Construction-PDF-Validation-AI)
- General-purpose engines:
  - PaddleOCR: about 91.4k stars, active 2026-10-19, with PP-Structure and PaddleOCR-VL for document/table parsing — [GitHub](https://github.com/PaddlePaddle/PaddleOCR)
  - PyMuPDF: about 10.9k stars — [GitHub](https://github.com/pymupdf/PyMuPDF)
  - pdfplumber: about 10.8k stars, extracts each char, line and rect plus tables — [GitHub](https://github.com/jsvine/pdfplumber)

### Inferences
- For PDFs exported from SOLIDWORKS (vector, with text kept as text), the best route is PyMuPDF or pdfplumber word and line extraction with coordinates. Then apply zone templates for the title block, revision table and BOM (line-grid cell reconstruction, which pdfplumber's table finder does), plus regexes for dimension, tolerance and GD&T strings. eDOCr2 or PaddleOCR serve as the fallback for scanned or outlined-text PDFs.
- eDOCr2's FCF CRNN and dimension/tolerance parsing are the most reusable open components for GD&T reading on raster input. Its table and title-block stage is weak and should be replaced with template or zone logic or a VLM.
- Korean text in title blocks: PaddleOCR supports Korean. eDOCr2 table OCR uses Tesseract "with a preselected language", so the `kor` traineddata would be needed. Neither has been verified on Korean drawings.
- All accuracy numbers are on small, non-standard benchmarks. Expect to build an in-house validation set from the team's own drawings.

### Gaps
- eDOCr2 table and title-block accuracy, GD&T symbol-class coverage, and dataset size were not obtained (the MDPI page was blocked).
- werk24 accuracy claims and pricing figures were not found.
- The exact eDOCr v1 repo status, stars and last commit are unverified.
- arXiv 2506.17374's per-category metrics were not retrieved.

## Q4. Vector PDF vs DXF vs raster for automated checks

### Takeaway
DXF with intact entities is semantically richest: true measured value versus override text, tolerance variables, ATTRIBs, layers and layouts. Vector PDF keeps exact text strings and coordinates but loses semantics: no measured value, no dimension association, no layers unless optional content groups exist. Raster requires OCR with recall of about 90–94% and CER of 1–8% in the best open tools. For SOLIDWORKS output, however, DXF semantics are often degraded on export (Q2), so the SOLIDWORKS API remains the authoritative source.

### Cited Findings
- DXF DIMENSION keeps both the measurement (`get_measurement()`) and the displayed or override text (`dxf.text`), which enables override detection. — [ezdxf get_entity_content](https://ezdxf.readthedocs.io/en/stable/tasks/get_entity_content.html)
- Raster OCR baselines are 90% precision/recall and 8% CER for eDOCr v1, and 93.75% dimension recall with 0.73% dimension CER for eDOCr2. — [Frontiers 2023](https://frontiersin.org/journals/manufacturing-technology/articles/10.3389/fmtec.2023.1154132/text); [MDPI 2025](https://www.mdpi.com/2075-1702/13/3/254)
- werk24 argues that traditional OCR struggles with technical drawings (blog series). This is a vendor source. — [werk24 blog](https://werk24.io/blog/the-limitations-of-traditional-ocr-in-processing-technical-drawings)
- pdfplumber exposes each char, rectangle and line for vector PDFs, which supports grid-based table reconstruction. — [pdfplumber](https://github.com/jsvine/pdfplumber)
- Practical tools route inputs in this order: native PDF text first, OCR second, and for DXF, CAD text first with OCR fallback. — [ocrx-engineering-drawings](https://github.com/aeewws/ocrx-engineering-drawings)
- Text-as-polyline exports (DXF) defeat entity-based text extraction. — [SOLIDWORKS forum 185434](https://forum.solidworks.com/thread/185434)

### Inferences
Recommended priority for the Korean team:
1. SOLIDWORKS API for dimensions (`IDisplayDimension` gives value versus text override), tolerances, GD&T, BOM and custom properties.
2. DXF via ezdxf for drawings received without the source model. Run override and tolerance checks only if DIMENSION entities survive.
3. Vector PDF via PyMuPDF/pdfplumber for title block, revision and BOM text with zone templates, and for string-level dimension, tolerance and note rules.
4. Raster through eDOCr2 or PaddleOCR only for scans or legacy drawings, with human review.

A cross-check between sources (SOLIDWORKS API BOM versus the PDF BOM table text) is a cheap, high-value rule.

### Gaps
- No published head-to-head benchmark comparing DXF, vector-PDF and raster extraction on the same drawings was found.
- How SOLIDWORKS PDF export handles fonts (text kept versus outlined, especially for Korean/CJK fonts) was not researched.
