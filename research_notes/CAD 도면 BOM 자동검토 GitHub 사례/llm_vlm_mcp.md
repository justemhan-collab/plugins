# LLM/VLM-based Engineering Drawing Review and CAD MCP Servers (as of 2026-10-20)

Research method notes: arXiv.org, getleo.ai and buildcad.ai were unreachable (DNS failure) from the research environment, and the GitHub API was blocked. Paper figures below come from search-result snippets of the arXiv pages, not the full text. Repo facts come from fetching the GitHub HTML pages on 2026-10-20. Those pages did not show last-commit dates, so "last commit" is UNVERIFIED for every repo unless stated otherwise. Star counts are a snapshot from 2026-10-20.

## Q1. What accuracy do current VLMs achieve on engineering-drawing tasks, and which benchmarks exist (2024-2026)?

### Takeaway
Zero-shot frontier VLMs are unreliable on structured drawing extraction. In the most-cited controlled study (Nov 2024), GPT-4o reached only 35.5% F1 on GD&T/dimension extraction and Claude-3.5-Sonnet hallucinated about 56% of the time. A fine-tuned 0.23B Florence-2 beat both by a wide margin. Newer vendor blogs (2026) report about 84-92% field accuracy for the latest models on small tests, but these are unverified and have small samples. There is no standard GD&T or title-block benchmark. The nearest public ones are DesignQA (2024), Enginuity (2026) and a few paper-specific datasets.

### Cited Findings
- **Khan et al., "Fine-Tuning Vision-Language Model for Automated Engineering Drawing Information Extraction" (arXiv 2411.03707, Nov 2024).**
  - Setup: Florence-2 (0.23B) was full-parameter fine-tuned on 400 expert-annotated drawings and compared with zero-shot GPT-4o and Claude-3.5-Sonnet on GD&T/dimension extraction. Metrics were precision, recall, F1 and hallucination rate. — [arXiv 2411.03707](https://arxiv.org/abs/2411.03707)
  - Results:
    - GPT-4o: precision 59.03%, recall 25.39%, F1 35.51%.
    - Claude-3.5: highest hallucination rate, 55.99%.
    - Fine-tuned Florence-2 against the best baseline on each metric: F1 +52.40%, hallucination −43.15%, precision +29.95%, recall +37.75%.
    - Florence-2's absolute values were not in the snippets. — [arXiv PDF 2411.03707](https://arxiv.org/pdf/2411.03707) (via search snippet); secondary summary: [Cognitive Future](https://cognitivefuture.ai/can-ai-read-gd-and-t-drawings/)
- **"From Drawings to Decisions: A Hybrid Vision-Language Framework" (arXiv 2506.17374, 2025).** YOLOv11-OBB detection is paired with a fine-tuned lightweight VLM. The dataset has 1,367 mechanical drawings across 9 annotation categories, including GD&T. — [arXiv 2506.17374](https://arxiv.org/pdf/2506.17374)
- **"A Multi-Stage Hybrid Framework for Automated Interpretation of Multi-View Engineering Drawings Using VLM" (arXiv 2510.21862, Oct 2025).** YOLO detectors feed two Donut-based VLMs that parse GD&T frames, measures and title blocks. — [arXiv 2510.21862](https://arxiv.org/abs/2510.21862)
- **"Context-Aware Mapping of 2D Drawing Annotations to 3D CAD Features Using LLM-Assisted Reasoning" (arXiv 2602.18296, Feb 2026).** Maps drawing annotations to CAD features. This is directly relevant to drawing-vs-model consistency. Accuracy numbers were not retrieved. — [arXiv 2602.18296](https://arxiv.org/pdf/2602.18296)
- **Enginuity (arXiv 2606.03410, Jun 2026).** A dataset and benchmark of 2,056 diagram–parts-table pairs. Four frontier VLMs were evaluated zero-shot. It reports a large gap between part identification and description fidelity. This is the closest public analogue to "drawing ↔ BOM/parts list" understanding. — [arXiv 2606.03410](https://arxiv.org/abs/2606.03410)
- **DesignQA (arXiv 2404.07917, 2024).** A multimodal benchmark on engineering documentation (rule compliance and dimension questions). It evaluates GPT-4o and others, including in retrieval-augmented setups. — [arXiv 2404.07917](https://arxiv.org/pdf/2404.07917)
- **"From Concept to Manufacturing: Evaluating VLMs for Engineering Design" (arXiv 2311.12668, 2023).** GPT-4V extracted all 5 dimensions in every trial but labelled them correctly only about two-thirds of the time. The typical error was mislabelling hole depth. — [arXiv 2311.12668](https://arxiv.org/pdf/2311.12668)
- **"Learning to Ground Before Reading: Unified PCB Engineering Drawing Parsing with Compact VLMs" (arXiv 2608.29268, Aug 2026).** Uses a compact grounding-first VLM. It cites "AECV-Bench" (2026) as a benchmark for multimodal models on architectural and engineering drawings. AECV-Bench details are UNVERIFIED. — [arXiv 2608.29268](https://arxiv.org/pdf/2608.29268)
- **Vendor and blog evaluations (not peer-reviewed, small samples, UNVERIFIED):**
  - SnapEDA (Feb 2026): Claude Opus 4.6 reached 92% overall accuracy on mechanical-drawing dimension extraction, against 84% for Gemini 3 Deep Think, in a 92-field test. — [SnapEDA blog](https://blog.snapeda.com/2026/02/16/evaluating-the-accuracy-of-ai-generated-cad-models/)
  - Businessware Technologies benchmark: GPT-4o struggled on tables and dimensions, Gemini ranked best, and Claude Opus sat in the middle tier (about 40% "extraction efficiency"). The post has labelling inconsistencies. — [Businessware blog 1](https://www.businesswaretech.com/blog/benchmarking-ai-on-tables-and-engineering-drawings-results-findings), [Businessware blog 2](https://www.businesswaretech.com/blog/benchmark-testing-ai-models-on-engineering-drawings)
- **eDOCr2 (MDPI Machines 13(3):254, 2025).** Deterministic OCR plus image processing segments a drawing into the information block, dimensions and feature control frames. It reports 93.75% text recall and a CER below 1% on its own benchmark. VLMs (Qwen2-VL-7B, GPT-4o) are then used after segmentation to check, filter or retrieve content. Code is on GitHub. — [MDPI eDOCr2](https://www.mdpi.com/2075-1702/13/3/254)
- **Commercial claim:** Werk24 claims 95% extraction accuracy on standard annotations, and 99% with a custom quality gate. This is a vendor claim. — [werk24.io](https://werk24.io/)

### Inferences
- Off-the-shelf zero-shot VLMs on whole-sheet images are not acceptable as the sole source of truth for dimension or tolerance values. Recall is low and hallucination is high in the controlled study. Newer models (2026) appear much better on small tests, but no independent benchmark confirms this.
- Fine-tuned small models (Florence-2, Donut, Qwen2-VL-7B) beat zero-shot frontier models on narrow extraction. They can also be run on-premise, which matters for customer-drawing confidentiality.
- For native CAD drawings (SLDDRW/DWG/DXF), image-based extraction is avoidable. Dimensions, notes and title-block attributes can be read as structured entities (see Q3), which makes the VLM accuracy problem mostly irrelevant for in-house drawings. VLMs are needed mainly for scanned or PDF-only customer drawings.

### Gaps
- Absolute Florence-2 F1 and hallucination values, and per-category (GD&T vs title block) breakdowns: arXiv full text was not reachable.
- I found no public benchmark dedicated to title-block extraction or GD&T semantic interpretation (for example, datum-reference correctness).
- No independent 2026 benchmark covers GPT-5-class, Claude 4.x or Gemini 3 models on drawings. Only vendor blogs exist.
- AECV-Bench details are unverified.

## Q2. Which architectures are used (deterministic extraction + LLM judgment vs pure VLM), and how is hallucination mitigated?

### Takeaway
The dominant research pattern from 2025-2026 is hybrid. Detection or segmentation (YOLO/OBB, or classic OCR region splitting) localizes annotations first. A small fine-tuned VLM or OCR then reads each crop, and an LLM or VLM is used only for verification, filtering or reasoning. Pure whole-image VLM prompting is used mainly as a baseline. The SolidWorks MCP SolidPilot uses the same principle on the CAD side: "deterministic compiler, no LLM," with the LLM planning only.

### Cited Findings
- **Detect-then-read pipelines:**
  - YOLOv11-OBB plus a fine-tuned VLM. — [arXiv 2506.17374](https://arxiv.org/pdf/2506.17374)
  - YOLO plus two Donut VLMs for GD&T, measures and title blocks. — [arXiv 2510.21862](https://arxiv.org/abs/2510.21862)
  - Ground-before-reading for PCB drawings. — [arXiv 2608.29268](https://arxiv.org/pdf/2608.29268)
- **Deterministic OCR plus VLM post-check:** eDOCr2 segments the drawing, runs OCR, then uses Qwen2-VL-7B or GPT-4o to check and filter. — [MDPI eDOCr2](https://www.mdpi.com/2075-1702/13/3/254)
- **Fine-tuning:** fine-tuning a small VLM cut hallucination by about 43% relative to the best zero-shot model. — [arXiv 2411.03707](https://arxiv.org/abs/2411.03707)
- **Plain OCR is not enough on its own:** it misreads Ø and ±, and GD&T symbols are beyond plain OCR. Domain lexicons, pattern constraints (thread, fit and unit templates) and validators are common mitigations. — [Werk24 blog on OCR limits](https://werk24.io/blog/the-limitations-of-traditional-ocr-in-processing-technical-drawings), [gndctl guide](https://gndctl.com/resources/a-guide-to-ocr-on-pdf-drawings)
- **Annotation-to-feature mapping** with LLM reasoning gives a drawing-vs-model consistency check. — [arXiv 2602.18296](https://arxiv.org/pdf/2602.18296)
- **CAD-side determinism:** SolidPilot uses Planner (IR) → deterministic Compiler (no LLM) → C# COM execution. Its `verify_state`, `compare_parts` and `compare_assemblies` tools support verification loops. — [eyfel/mcp-server-solidworks](https://github.com/eyfel/mcp-server-solidworks)

### Inferences
Recommended architecture for a semiconductor-equipment team (inference):
1. **Deterministic extraction first.** Read BOM, custom properties (title block), dimensions and notes via the SolidWorks API, PDM or ezdxf/LibreDWG.
2. **Rule engine for hard checks.** Examples: title-block field completeness, revision match between drawing and model, BOM quantity versus assembly component count, material/finish codes against a company standard list, and tolerance presence.
3. **LLM for judgment and reporting only.** Examples: DFM comments, note wording, standard-compliance explanation. Every LLM claim should cite a source entity ID or handle so a person can verify it.
4. **VLM path only for raster or PDF customer drawings.** Use crop-level reading (detect → read), and cross-check numbers against any available model data.

This matches the "LLM never produces the authoritative number" pattern used across the literature.

### Gaps
- No quantitative comparison of hybrid versus pure-VLM on the same dataset was retrieved (full papers inaccessible).
- No published open-source end-to-end "drawing QA checker" with a measured false-positive and false-negative rate was found.

## Q3. Which CAD MCP servers can support a read-only review workflow, and what are their limits?

### Takeaway
Many SOLIDWORKS and AutoCAD MCP servers exist (at least nine SolidWorks repos alone). Almost all are hobby or alpha projects that drive the desktop app through Windows COM, and they expose write or arbitrary-execution tools alongside reads. Few expose BOM or custom-property reads explicitly.

The best fits for read-only review:
- **dimitrovakulenko/dwg-mcp-server:** headless LibreDWG, read-only source, allowed-roots sandbox, GPL-3.0.
- **Codemod3/cad-mcp:** ezdxf, explicit read tools including `dxf_get_dimensions`, copy-on-write, MIT, very new.
- **SolidPilot (eyfel):** `analyze_model`, `analyze_assembly` and `analyze_drawing`; AGPL-3.0, alpha, no BOM yet.

Autodesk shipped official Fusion MCP servers in April 2026 (secondary sources).

### Cited Findings — SOLIDWORKS
- **eyfel/mcp-server-solidworks ("SolidPilot")**
  - Maturity: 405 stars, 77 forks, 56 commits. Self-described as a "working prototype / early alpha."
  - License: AGPL-3.0 (network clause), plus a commercial license and a CLA.
  - Platform: SolidWorks 2026 SP3, Windows.
  - Architecture: Python MCP adapter → REST → C# .NET 4.8 COM executor on `localhost:5000`. No authentication is described.
  - Read tools: `analyze_model` (mass properties, bodies, features, topology map), `analyze_assembly` (component tree, transforms, mates), `analyze_drawing` (reads DXF/DWG views, dimensions and title-block parameters), `get_selection`, `verify_state`, `compare_parts`, `compare_assemblies`.
  - Not present: a BOM tool (on the roadmap), a custom-properties reader, or GD&T.
  - The README gives inconsistent tool counts (38, 47 and "40-odd").
  - Write and delete tools exist. No input sandboxing is described. — [GitHub](https://github.com/eyfel/mcp-server-solidworks)
- **tylerstoltz/SW_MCP**
  - Maturity: 3 stars, 2 commits. No LICENSE file (informal "as-is" statement only).
  - Platform: SolidWorks 2020, .NET 8.
  - Read tools: `get_model_info`, feature-tree resource, `access_solidworks_property` (get and set; "works on some stuff").
  - Execution tool: `execute_solidworks_api` (arbitrary API calls; "barely works").
  - Not present: BOM, drawing or dimension tools.
  - The README suggests running Claude Desktop as Administrator. Experimental. — [GitHub](https://github.com/tylerstoltz/sw_mcp)
- **Other SolidWorks MCPs found (2026; stars, license and dates UNVERIFIED):**
  - **jaylamping/solidworks-mcp:** TS front end with a .NET COM worker. 100+ tools including measure/inspect, plus a low-level API invoke. — [GitHub](https://github.com/jaylamping/solidworks-mcp)
  - **andrewbartels1/SolidworksMCP-python:** about 134 tools. Self-described as a hobby project "under active construction." — [GitHub](https://github.com/andrewbartels1/SolidworksMCP-python)
  - **MatteoBaisini/solidworks-mcp:** tested on SW 2026; sketch, extrude, measure, export. — [GitHub](https://github.com/MatteoBaisini/solidworks-mcp)
  - **just1step/solidworks-mcp:** tray app that shares one COM session. — [GitHub](https://github.com/just1step/solidworks-mcp)
  - **tkhosravi/solidworks-mcp:** C#/.NET 8, SW 2025, 40+ tools. — [GitHub](https://github.com/tkhosravi/solidworks-mcp)
  - **tmkaio/solidworks-mcp:** sheet-metal part analysis, SW 2022+. — [GitHub](https://github.com/tmkaio/solidworks-mcp)
  - **haunchen/solidworks-mcp:** SW 2021; creates drawings and PDFs; optional Streamable HTTP over LAN, which is a network-exposure risk. — [GitHub](https://github.com/haunchen/solidworks-mcp)
  - **vespo92/SolidworksMCP-TS:** alpha. — [GitHub](https://github.com/vespo92/SolidworksMCP-TS)
  - **kilwizac/solidworks-api-mcp:** API-documentation search only (14,000+ methods). Useful as a coding aid for writing your own deterministic extractor; it does not touch models. — [GitHub](https://github.com/kilwizac/solidworks-api-mcp)

### Cited Findings — DWG/DXF/AutoCAD
- **dimitrovakulenko/dwg-mcp-server**
  - Maturity: 20 stars, 60 commits, CI on 4 platforms, listed in the official MCP Registry, no releases shown.
  - License: GPL-3.0.
  - Architecture: Python MCP host plus a Rust worker per file, built on vendored LibreDWG. No AutoCAD or ODA needed.
  - Tools: `dwg.list_roots`, `dwg.open_file`, `dwg.close_file`, `dwg.list_types`, `dwg.list_file_types`, `dwg.describe_type`, `dwg.get_objects`, `dwg.query_objects`, `dwg.set_entity_properties` (in-memory only), `dwg.list_render_views`, `dwg.render_view` (SVG/PNG).
  - Security: files are opened only under allowed roots (MCP roots or `DWG_MCP_ALLOWED_ROOTS`); the source is never saved; there is process isolation per document.
  - Limits: rendering skips some entity types. — [GitHub](https://github.com/dimitrovakulenko/dwg-mcp-server)
- **Codemod3/CAD-MCP**
  - Maturity: 1 star, 4 commits, has pytest tests.
  - License: MIT.
  - Engine: ezdxf. DWG requires the free ODA File Converter.
  - Read tools: `dxf_summary`, `dxf_list_layers`, `dxf_get_layer_entities`, `dxf_get_entities_by_type`, `dxf_get_text`, `dxf_get_dimensions`, `dxf_get_blocks`, `dxf_get_block_detail`.
  - Write tools: these act only on a copy (`cad_create_editable_copy`, and so on).
  - Security: local stdio. It claims "files never leave your machine" (the server's own claim; an LLM client may still send content to the cloud). — [GitHub](https://github.com/Codemod3/cad-mcp)
- **puran-water/autocad-mcp:** AutoCAD LT. v3.1 has 8 consolidated tools, "freehand AutoLISP execution," File IPC and ezdxf backends, and list/count/get reads. — [GitHub](https://github.com/puran-water/autocad-mcp) (license, stars and date UNVERIFIED)
- **U-C4N/Autocad-MCP:** 240+ tools. Live COM or headless ezdxf. Reads DWG/DXF and handles ISO GD&T and tolerance sheets. MIT per search snippet. — [GitHub](https://github.com/U-C4N/Autocad-MCP)
- **bimwright/dwg-mcp:** .NET 8, AutoCAD 2022-2027. Tools `dwg_get_entity_properties` and `dwg_query_entities`. Model space only, plus code execution. — [GitHub](https://github.com/bimwright/dwg-mcp)
- **Other AutoCAD MCPs:**
  - **C48H74/autocad-mcp:** COM, read/modify in AutoCAD and Plant 3D. — [GitHub](https://github.com/C48H74/autocad-mcp)
  - **id0o0bi/autocad-mcp:** ezdxf plus an accoreconsole backend; "audit" drawings. — [GitHub](https://github.com/id0o0bi/autocad-mcp)
  - **beiming183-cloud/AutoCAD-MCP:** headless DXF. — [GitHub](https://github.com/beiming183-cloud/AutoCAD-MCP)

### Cited Findings — FreeCAD / Fusion / Onshape / others
- **neka-nat/freecad-mcp**
  - Maturity: about 3.0k stars, 355 forks; the most popular open-source engineering CAD MCP.
  - License: MIT.
  - Capabilities: create and edit models, run Python scripts, inspect documents, run FEM.
  - Security: the RPC server binds to localhost by default; a remote-connection config exists; no authentication is described. Arbitrary Python execution means any connected client can run code. — [GitHub](https://github.com/neka-nat/freecad-mcp)
- **Other FreeCAD MCPs:** bonninr/freecad_mcp (MIT, last updated Mar 2025) and contextform/freecad-mcp (noted as unmaintained). — via aggregator search summaries ([mcpexplorer](https://mcpexplorer.com/servers/freecad-mcp), [ChatForest](https://chatforest.com/reviews/cad-3d-modeling-mcp-servers/)); UNVERIFIED
- **Fusion:** Autodesk released official MCP support in April 2026 as two servers. The Fusion MCP server works in a design session for modeling. The Fusion Data MCP works at the project/hub layer, which is relevant for metadata and BOM-like data. Community: JustusBraitinger's server (27 tools); sockcymbal/autodesk-fusion-mcp-python on Glama. — [Leo AI blog "MCP for CAD in 2026"](https://www.getleo.ai/blog/cad-mcp-server-comparison-2026) (page unreachable; from a search summary), [Glama](https://glama.ai/mcp/servers/sockcymbal/autodesk-fusion-mcp-python); UNVERIFIED details
- **Onshape:** BLamy/onshape-mcp (community, few stars). Onshape is cloud-based, so it uses API-key or OAuth access to the customer's cloud documents. — via [Snyk "9 MCP servers for CAD"](https://snyk.io/articles/9-mcp-servers-for-computer-aided-drafting-cad-with-ai/) and [TechDraw AI](https://techdrawai.com/blog/cad-mcp-servers) search summaries; UNVERIFIED. Onshape's REST API natively exposes BOM and metadata endpoints (background knowledge, not verified here).
- **Aggregator comparisons:**
  - [Leo AI blog](https://www.getleo.ai/blog/cad-mcp-server-comparison-2026)
  - [buildcad.ai guide](https://buildcad.ai/guides/cad-mcp-servers)
  - [TechDraw AI blog](https://techdrawai.com/blog/cad-mcp-servers)
  - [forgemcp.ai MCAD list](https://forgemcp.ai/platforms/mcad-3d-cad)
  - [ChatForest review](https://chatforest.com/reviews/cad-3d-modeling-mcp-servers/)
  - [mcpserver.so CAD tag](https://mcpserver.so/list?tags=CAD)
- **CadQuery MCPs:** not verified in this round. Search did not reach dedicated repos.

### Inferences
- **No SolidWorks MCP found offers a clean, read-only "BOM + custom properties + drawing dimensions" surface.** For a SOLIDWORKS-centric team, the most robust option is a small in-house read-only MCP (or plain script). It would use the SolidWorks API (or the Document Manager API, which reads properties and BOM without launching SolidWorks — background knowledge) and expose only `get_bom`, `get_custom_properties`, `get_drawing_dimensions/notes` and `get_title_block`. SolidPilot's `analyze_*` design and kilwizac's API-docs MCP are good references for this.
- **Most reusable components:**
  - LibreDWG-based dwg-mcp-server: read-only DWG without an AutoCAD license, with a good sandbox model. GPL-3.0 is fine for internal use but matters if redistributed.
  - Codemod3's `dxf_get_dimensions`/`dxf_get_text` pattern (MIT; easy to fork or rewrite with ezdxf).
  - SolidPilot's compare and verify tools (AGPL: internal use is OK, but avoid offering a modified version as a network service without releasing source).
- **Security risks for confidential customer drawings:**
  1. Many servers expose arbitrary code or API execution: `execute_solidworks_api`, FreeCAD Python, AutoLISP, bimwright code exec. Prompt injection hidden in drawing text (notes, title block) could trigger actions. Disable or strip these tools for review use.
  2. Local HTTP or RPC endpoints have no authentication: SolidPilot on localhost:5000, FreeCAD RPC, haunchen's LAN HTTP mode.
  3. `uvx` from a git URL runs unpinned code, so pin commits and vendor the code.
  4. The MCP server being local does not keep data local: extracted content goes to whatever LLM API the client uses. On-premise or VPC LLM hosting, or a zero-data-retention enterprise contract, is needed for customer drawings.
  5. Cloud CAD (Onshape, Fusion Data) MCPs move access tokens to the agent, so use read-only scopes.

### Gaps
- Last-commit dates for all repos: the GitHub API was blocked and the HTML did not show dates.
- Licenses and stars for jaylamping, andrewbartels1, puran-water, U-C4N and bimwright were not individually verified.
- Official Autodesk Fusion MCP tool list and auth model: the primary source was not reached.
- Onshape MCP details (hedless/onshape-mcp vs BLamy/onshape-mcp) were not verified.
- No MCP specifically for SOLIDWORKS PDM or BOM tables was found.
- No PTC Creo, Siemens NX or CATIA specifics were retrieved. forgemcp.ai lists NX and Inventor but was not fetched.

## Q4. Commercial AI drawing-check offerings (brief comparison)

### Takeaway
Commercial tools split into three groups:
- **Extraction/RFQ/DFM screening APIs:** Werk24.
- **Drawing search/management:** CADDi Drawer.
- **CAD copilots:** Leo AI.

None that I verified publishes independent accuracy benchmarks. All are cloud services, which raises customer-drawing confidentiality questions.

### Cited Findings
- **Werk24:**
  - Capabilities: AI feature extraction from technical drawings, covering PMI, GD&T, tolerances and threads; RFQ feasibility screening against a shop's machine limits (fit/no-fit with reasons); DFM rule checks.
  - Access: Python SDK, REST API, webhooks.
  - Accuracy claims: 95% on standard annotations, 99% with a custom quality gate (vendor claim). — [werk24.io](https://werk24.io/)
- **CADDi Drawer:** a drawing data-management platform. Search similar drawings by shape, text, dimensions or part name; links procurement, quality and CAD/CAM data. Rated 4.8/5 from 43 reviews on Capterra. Users report imperfect photo-based similarity search and limited integrations. It is not a DFM checker. — [Capterra](https://www.capterra.com/p/10014799/CADDi-Drawer/), [CADDi news](https://us.caddi.com/resources/news/caddi-drawer-generates-searchable-2d-drawings-saving-design-time-and-procurement-costs)
- **Drawer AI:** an electrical estimating tool on PDF drawings, not mechanical DFM. — [CB Insights](https://www.cbinsights.com/company/werk24/alternatives-competitors)
- **Leo AI:** an engineering CAD copilot vendor that publishes CAD-MCP comparison content (2026). — [Leo AI blog](https://www.getleo.ai/blog/cad-mcp-server-comparison-2026) (unreachable; title only)
- **InspectionXpert:** an OCR-based ballooning and inspection-report tool with OCR extraction modes. It is a reference point for the deterministic-OCR approach. — [InspectionXpert help](https://help.inspectionxpert.com/ocr-extraction-modes)

### Inferences
- For a Korean semiconductor-equipment team, the commercial SaaS options imply uploading customer drawings to third-party clouds (Werk24 is Germany-based; CADDi is Japan/US). This likely conflicts with customer NDAs unless an on-prem or private deployment is negotiated.
- An in-house pipeline is likely more acceptable: deterministic CAD extraction plus a rule engine plus an LLM via an enterprise or zero-retention channel or a local model.
- CADDi is the closest commercial analogue for "find similar past drawings / reuse" use cases in the Asia region.

### Gaps
- I could not find "Drafter" or a dedicated "AI drawing checker" product for mechanical drawing QA in this round.
- I found no independent accuracy evaluations of Werk24, CADDi or Leo AI.
- On-prem deployment availability for each vendor was not verified.
