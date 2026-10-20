# 성숙한 오픈소스 도면검사기는 없음: JUSTEM이 직접 구축할 영역

조사 기준일: 2026-10-20 · 대상: SolidWorks/CAD 도면·3D 모델·BOM 자동 판독 및 검토 시스템의 GitHub·공개 자료 사례 · 용도: JUSTEM 반도체 장비 설계팀 독립 Python 도구 계획 검토

---

## 1. 결론 요약 (BLUF)

- **결론**: GitHub에는 표제란·치수·공차·GD&T·Dangling·Override·BOM 교차검증을 한 번에 수행하는 성숙한 오픈소스 도면검사기가 **없음**. 기존 사례는 (a) 대형 예제 라이브러리 1개(xarial/codestack, 385★), (b) 0~5★ 수준의 단일 목적 BOM 추출·속성 도구, (c) 2026년 급증한 Python/COM "에이전트·MCP·설계검토" 시제품으로 파편화되어 있음 ([smart_SW](https://github.com/coleas33/smart_SW), [codestack](https://github.com/xarial/codestack)).
- **JUSTEM 계획(SolidWorks API 추출 → JSON → Python 규칙검사 → Excel 검토표)은 업계에서 가장 검증된 패턴과 일치함**. 가장 가까운 선례는 coleas33/smart_SW(C# 추출기 → Python 결정론적 검사 → JSON/Markdown 보고서, "숫자는 테스트된 코드가 내고 모델은 판정하지 않음")와 kcarlton55/bomcheck(SolidWorks BOM Excel ↔ ERP Excel 비교, PyPI 2.7, 2026-08-15)임 ([smart_SW](https://github.com/coleas33/smart_SW), [PyPI bomcheck](https://pypi.org/project/bomcheck/)).
- **원천 데이터 우선순위**: SolidWorks API(진실 원천) > Document Manager API(대량·경량 속성/참조/표 읽기) > DXF(ezdxf, 보조) > 벡터 PDF(pdfplumber 등) > 래스터 OCR/VLM(외부·레거시 도면 한정). DXF 내보내기는 축척·텍스트·BOM 표가 손상되는 손실 경로임 ([Javelin](https://www.javelin-tech.com/blog/2015/03/selecting-base-scale-for-exporting-solidworks-drawings-to-dxf-dwg/), [SOLIDWORKS 포럼 225774](https://forum.solidworks.com/thread/225774)).
- **LLM/VLM은 판정 주체가 될 수 없음**: 통제 실험에서 GPT-4o의 GD&T/치수 추출 F1은 35.51%, Claude-3.5-Sonnet 환각률은 55.99%임 ([arXiv 2411.03707](https://arxiv.org/abs/2411.03707)). 결정론적 추출 + 규칙엔진이 판정하고 LLM은 설명·보고서 문장화에만 쓰는 하이브리드가 연구·실무 공통 결론임.
- **상용 도구 공백이 자체 개발 근거임**: SOLIDWORKS Design Checker(Professional/Premium 포함)는 서식·Dangling·BOM 풍선 등 단일 문서 검사에 강하나, 모델↔도면↔ERP 3자 교차검증과 Excel 검토표는 제공하지 않음 ([Hawk Ridge](https://support.hawkridgesys.com/hc/en-us/articles/26745157214989-How-to-Use-the-Design-Checker-Task-in-SOLIDWORKS-Task-Scheduler)).
- **주의 사항**: 참고 가치가 큰 다수 저장소가 AGPL/GPL(smart_SW, SolidPilot, bomcheck, LibreDWG, PyMuPDF)이므로 코드 복사 대신 아이디어 재구현 권장. 임의 코드 실행 MCP 도구와 클라우드 LLM 전송은 고객 도면 기밀 리스크가 큼.

---

## 2. 조사 범위·방법·한계

- **범위**: SolidWorks API(pywin32/C#/VBA)·Document Manager(DM) API, ezdxf 기반 DXF/DWG, PDF 판독(벡터·OCR·VLM), STEP 파싱 BOM, BOM 교차검증, CAD MCP 서버, LLM/VLM 도면검토, 상용 도구.
- **방법**: GitHub 검색 API(스타·라이선스·`pushed_at`/`updated_at`), README·소스 직접 확인, PyPI JSON API, 웹 검색.
- **한계(반드시 감안)**
  - • `updated_at`은 스타만 늘어도 바뀌므로 **최근 커밋일이 아님**. 표에 "(updated_at)"으로 구분 표기함.
  - • **접속 차단으로 검색 요약(snippet)에만 의존한 출처**: help.solidworks.com, codestack.net, javelin-tech.com, cadbooster.com, arxiv.org, mdpi.com, opencascade.com, openbom.com, getleo.ai, ezdxf 문서 일부. 해당 출처의 수치·메서드명은 "(검색 요약)"으로 표기함.
  - • 일부 소형 저장소의 라이선스·최근 커밋·내부 코드는 미확인이며 "확인 필요"로 표기함.
  - • 스타 수는 2026-10-20 스냅샷임.

---

## 3. 유형별 구현 사례 지형

### 3.1 SolidWorks API·Document Manager 기반 도구 — 아키텍처 선례는 있으나 전체 규칙셋은 없음

| 프로젝트 | 접근법 | 기술스택 | 성숙도(2026-10-20) | 라이선스 | 한계 |
|---|---|---|---|---|---|
| [coleas33/smart_SW](https://github.com/coleas33/smart_SW) | 읽기전용 가드 추출 애드인 → IR → Python 결정론적 검사(YAML 표준 프로파일) → session/check/attention.json + report.md | C# .NET 4.8 x64(SW2024 API) + Python 3.11(uv) | 2★, 생성 2026-09-13, 831 커밋 | AGPL-3.0-or-later(PyMuPDF 의존) | 단일 SW2024 좌석 파일럿, 추출 범위·JSON 스키마 README 미기재 |
| [THERMOPAC/ThermopacCommunication-R1](https://github.com/THERMOPAC/ThermopacCommunication-R1/blob/HEAD/local-agent/extractor/extract_dimensions.py) `extract_dimensions.py` | 치수·공차 추출, 다중 메서드명 시도(`get_com_value`), 소프트 실패, dict 반환 | Python/pywin32 | 확인 필요 | 확인 필요 | 루프 상한(뷰 100, 치수 500) |
| [rd3n-ai/iso7200-cad-standard](https://github.com/rd3n-ai/iso7200-cad-standard) | ISO 7200 표제란·SSOT 속성 스키마·기계판독 감사 룰북 | Python | 1★, 2026-08-22 1일 업로드 | Apache-2.0 | 참조용 룰북 수준 |
| [easyhard01/EdmBOM_CSharp](https://github.com/easyhard01/EdmBOM_CSharp) | CMP 장비 BOM 비교(SolidWorks EPDM + Excel) | C# | 1★, 생성 2026-09-20 | 확인 필요 | 내용 미검토, 반도체 장비 도메인 유사 |
| [Yanru1004/bom_check](https://github.com/Yanru1004/bom_check) | BOM ↔ SolidWorks 부품목록 검사 | Python | 0★ (updated_at 2026-05-24) | 확인 필요 | 내용 미검토 |
| [matt454357/sw-BOM-Scan](https://github.com/matt454357/sw-BOM-Scan) | 어셈블리 순회 → SQLite BOM | C# | 2★, 2023-07경 | 확인 필요 | 정체 |
| [reikus/xlBOM…Community-Edition](https://github.com/reikus/xlBOM-with-thumbnails-Community-Edition) | BOM → Excel(썸네일 포함) 애드인 | 확인 필요 | 4★ (updated_at 2026-03-26) | 확인 필요 | 검사 기능 없음 |
| [jrobelia/solidworks-inventree-addin](https://github.com/jrobelia/solidworks-inventree-addin) | 속성·BOM ↔ InvenTree 비교·반영 | C# | 3★, 오픈 이슈 25 | 확인 필요 | 속성 diff UX 참고용 |
| [yzlin286-wq/sw_drawing_studio](https://github.com/yzlin286-wq/sw_drawing_studio/blob/HEAD/app/services/docmgr_service.py) | DM API Python 호출, 라이선스 키 env/config 분리, DLL 미존재 시 경고 | Python/pywin32 | 확인 필요 | 확인 필요 | DM 키 관리 패턴 참고 |
| [stefansterk/SWhereUsed](https://github.com/stefansterk/SWhereUsed) | DM으로 Where-used 인덱싱(SW 미실행) | Python | 1★, 생성 2026-10-04 | 확인 필요 | 신생 |
| [buianhtuan2903/autoexport-solidwork](https://github.com/buianhtuan2903/autoexport-solidwork) | 도면 → PDF/DWG 일괄 내보내기 | C# | 29★ (updated_at 2026-10-07) | 확인 필요 | 검사 없음, 배치 패턴 참고 |

- **시사점**
  - • "추출(IR) → 결정론적 규칙 → 다중 산출물" 구조는 smart_SW가, "버전차 흡수용 다중 메서드명 + 소프트 실패" 추출 패턴은 THERMOPAC이 가장 직접적인 참고임.
  - • 전체 QA 규칙셋(공차 유무, Override, Dangling, 표제란 완결성, BOM↔어셈블리, 리비전표)을 구현한 공개 사례는 확인되지 않음. JUSTEM 도구는 범위 면에서 신규임.

### 3.2 프레임워크·래퍼 — .NET은 xCAD.NET, Python은 얇은 래퍼뿐

| 프로젝트 | 언어 | ★ | 최근 push | 라이선스 | 비고 |
|---|---|---|---|---|---|
| [xarial/xcad](https://github.com/xarial/xcad) | C# | 217 | 2026-09-28 | MIT | 1,010 커밋, SOLIDWORKS와 DM에 동일 순회 코드 적용. 도면 주석·표 커버리지는 확인 필요 |
| [xarial/codestack](https://github.com/xarial/codestack) | VBA/C# | 385 | 2026-05-07 | MIT | 최대 예제·매크로 라이브러리, 오픈 이슈 460 |
| [CAD-Booster/SolidDNA](https://github.com/CAD-Booster/SolidDNA) | C# | 160 | (updated_at 2026-10-15) | MIT | 애드인 중심, .NET 4.8 |
| [weianweigan/SolidWorksLookup](https://github.com/weianweigan/SolidWorksLookup) | C# | 110 | (updated_at 2026-10-19) | 확인 필요 | API 객체 탐색기(뷰·주석 속성 탐색용) |
| [deloarts/pyswx](https://github.com/deloarts/pyswx) | Python | 22 | 2026-01-26 | MIT | SW2024+ 전용 타입 래퍼, 베타 v0.6.0, ByRef를 튜플로 반환 |
| [Glutenberg/swtoolkit](https://github.com/Glutenberg/swtoolkit) | Python | 74 | 2021-03-29 | MIT | 정체 |
| [mklanac/pySolidWorks](https://github.com/mklanac/pySolidWorks) | Python | 78 | 2023-03-09 | GPL-3.0 | 정체, GPL |
| [SeanYeatts/SolidWrap](https://github.com/SeanYeatts/SolidWrap) | Python | 14 | 2025-02-03 | MIT | SW + PDM 래퍼 |
| [schwitters/openswx](https://github.com/schwitters/openswx) | C++20 | 31 | 2026-10-09 | MIT | **SolidWorks 미설치로** SLDPRT/SLDASM/SLDDRW 읽기(속성·참조·시트·뷰 참조·재귀 BOM JSON). 주석·치수 추출은 미문서화, 실도면 신뢰성 미검증 |
| [kilwizac/solidworks-api-mcp](https://github.com/kilwizac/solidworks-api-mcp) 외 | TS/Py | 9 등 | 2026 | 확인 필요 | API 문서 검색 DB(14,000+ 메서드) — 개발 보조용 |

- **판단**: Python 우선 팀이라면 래퍼 채택보다 **pywin32 직접 호출 + 사내 헬퍼 계층(THERMOPAC식 다중 메서드명 getattr)** 이 견고함. Python 래퍼는 모두 정체 또는 베타임 ([pyswx](https://github.com/deloarts/pyswx), [swtoolkit](https://github.com/Glutenberg/swtoolkit)).
- **대안**: C# 헤드리스 콘솔 추출기를 허용하면 xCAD.NET(MIT)이 보일러플레이트를 줄이고 DM 백엔드 전환이 쉬움. 규칙·Excel 단계는 Python 유지.

### 3.3 ezdxf 기반 DXF/DWG 검사 — 라이브러리는 성숙, 검사기는 부재

- **핵심 라이브러리**
  - • [mozman/ezdxf](https://github.com/mozman/ezdxf): 약 1,482★, MIT(PyPI/README 기준, 당일 재확인 안 함), updated_at 2026-10-19, 문서 1.4.x.
  - • [LibreDWG/libredwg](https://github.com/LibreDWG/libredwg): 약 1,631★, **GPL**, DWG 직접 판독. 일부 엔티티 미지원 보고 ([FreeCAD wiki](https://wiki.freecad.org/Translations:Installing_additional_components/74/en)).
  - • DWG는 ezdxf `odafc` 애드온 + 무료 ODA File Converter 경유가 표준 경로임. 외부 실행파일 경로 변경에 따른 보안 경고가 문서에 있음 ([ezdxf odafc](https://ezdxf.readthedocs.io/en/stable/addons/odafc.html)).
- **ezdxf 검사 기법(문서 일부는 검색 요약)**
  - • Override 판별: `dimension.get_measurement()`(정의점 기반 실측값) vs `dimension.dxf.text`(`""`/`"<>"`=실측 표시, `" "`=숨김, 그 외=Override) 비교 ([ezdxf get_entity_content](https://ezdxf.readthedocs.io/en/stable/tasks/get_entity_content.html)).
  - • 공차: DIMSTYLE 또는 엔티티 XDATA의 `dimtol`/`dimtp`/`dimtm`/`dimlim`을 `dimension.override()`로 읽음 ([ezdxf DimStyle](https://ezdxf.readthedocs.io/en/stable/tables/dimstyle_table_entry.html), 검색 요약).
  - • ACAD_TABLE: `read_acad_table_content()` **읽기 전용**, 행/열 수 누락 시 전체가 첫 행에 들어감 ([ezdxf](https://ezdxf.readthedocs.io/en/stable/tasks/get_entity_content.html), 검색 요약).
- **적용 사례(모두 소형)**
  - • [aeewws/ocrx-engineering-drawings](https://github.com/aeewws/ocrx-engineering-drawings): 10★, MIT. DXF의 TEXT/MTEXT/ATTRIB/ATTDEF/DIMENSION/MLEADER 직접 추출 후 잔여 시각요소 OCR(PaddleOCR, RapidOCR 폴백), "벡터 우선·OCR 보조·보수적 병합". 규칙검사 없음.
  - • [MatheusMeM/dwg-interpreter-claude-skill](https://github.com/MatheusMeM/dwg-interpreter-claude-skill): 0★, LibreDWG + ezdxf로 레이아웃·뷰포트 축척·레이어·블록·표제란·치수 JSON화.
  - • [JMcQB/CAD-Title-Block-Extractor](https://github.com/JMcQB/CAD-Title-Block-Extractor): 0★, 내용 확인 필요.
- **SolidWorks → DXF/DWG 내보내기 함정(문서화된 4건)**
  - • 모든 뷰에 단일 축척 적용 → 다축척 시트에서 일부 뷰 측정값 왜곡 ([Javelin 2015](https://www.javelin-tech.com/blog/2015/03/selecting-base-scale-for-exporting-solidworks-drawings-to-dxf-dwg/)).
  - • BOM 등 표가 동일 이름 블록으로 내보내져 다른 DWG로 복사 시 내용 덮어씀 ([SOLIDWORKS 포럼 225774](https://forum.solidworks.com/thread/225774), 검색 요약).
  - • 스케치 텍스트가 폴리라인으로 변환 ([포럼 185434](https://forum.solidworks.com/thread/185434)), TrueType 설정 시 텍스트 유지 ([CamBam 포럼](https://cambamcnc.com/forum/index.php?topic=8104.0)).
  - • 해치는 비연관 해치로 내보내짐 ([SOLIDWORKS help DXF/DWG](https://help.solidworks.com/2023/English/Solidworks/sldworks/c_DXF-DWG_Files_dxf_dwg-Files.htm), 검색 요약).
  - • DIMENSION 엔티티 유지 여부, 표제란 속성의 ATTRIB vs TEXT 매핑은 **확인 필요**(샘플 내보내기 실험 필요).
- **판단**: JUSTEM 자체 도면은 DXF를 검사 원천으로 쓰지 말 것. DXF 경로는 원본 모델 없이 수신한 외부 도면 전용.

### 3.4 PDF 도면 판독(벡터·OCR·VLM) — 오픈소스 최고 기준선은 eDOCr2

| 프로젝트 | 접근 | 성능·성숙도 | 라이선스 | 한계 |
|---|---|---|---|---|
| [javvi51/edocr2](https://github.com/javvi51/edocr2) (eDOCr2) | 정보블록·치수·FCF 분할 후 OCR, GD&T 기호를 CRNN 문자로 인식, Qwen2-VL-7B/GPT-4o로 사후검증 | 치수 재현율 93.75%, FCF 재현율 100%, 치수 CER 0.73%, FCF CER 5.70% ([MDPI Machines 2025](https://www.mdpi.com/2075-1702/13/3/254), 검색 요약) · 81★, 19 커밋 | MIT | 표·표제란은 Tesseract 일괄 OCR로 라벨/값 구분 없음, 자체 소규모 벤치마크 |
| eDOCr v1 | keras-ocr 패키지 | 검출 P/R 90%, F1 94%, CER 8% ([Frontiers 2023](https://frontiersin.org/journals/manufacturing-technology/articles/10.3389/fmtec.2023.1154132/text)) | 확인 필요 | 저장소 현황 확인 필요 |
| [W24-Service-GmbH/werk24-python](https://github.com/W24-Service-GmbH/werk24-python) | 상용 API: 표제란·BOM·리비전표·치수·GD&T 등 추출 | 93★, 벤더 주장 95%(품질게이트 시 99%) ([werk24.io](https://werk24.io/)) | 상용(클라이언트도 상용 라이선스) | 클라우드 업로드, 종량제 |
| [Bakkopi/engineering-drawing-extractor](https://github.com/Bakkopi/engineering-drawing-extractor) | OpenCV + pytesseract + openpyxl로 표제란 → Excel | 96★, 4 커밋 | 미표시 | 정확도 미보고, 고정 명명규칙 |
| [Night-E-ye/Drawing_ocr_extractor](https://github.com/Night-E-ye/Drawing_ocr_extractor) | pdfplumber 좌표 기반 표제란 필드 추출(벡터 PDF) | 소형 | 확인 필요 | — |
| [1378/Construction-PDF-Validation-AI](https://github.com/1378/Construction-PDF-Validation-AI) | PDF + OCR + VLM + 결정론적 규칙(로컬 Ollama) | 소형 | 확인 필요 | 건설 도메인 |

- **범용 엔진**: [PaddleOCR](https://github.com/PaddlePaddle/PaddleOCR)(약 91.4k★, 한국어 지원, PP-Structure), [pdfplumber](https://github.com/jsvine/pdfplumber)(약 10.8k★, 문자·선·사각형 좌표 및 표 재구성, 라이선스 확인 필요), [PyMuPDF](https://github.com/pymupdf/PyMuPDF)(약 10.9k★, smart_SW가 AGPL을 채택한 원인 의존성).
- **연구 동향**: YOLOv11-OBB 검출 + Donut/Florence-2 판독으로 GD&T F1 97.3% 보고(사내 데이터셋, 비교 불가) ([arXiv 2505.01530](https://arxiv.org/abs/2505.01530v3), 검색 요약). 제로샷 Donut 표제란 F1 0.533, 환각률 0.478로 **표제란 판독은 여전히 난제** ([arXiv 2510.21862](https://arxiv.org/pdf/2510.21862), 검색 요약).
- **판단**: SolidWorks에서 내보낸 벡터 PDF는 좌표 기반 텍스트 + 영역 템플릿(표제란·리비전표·BOM)으로 처리하고, OCR/VLM은 스캔·레거시·고객 PDF에만 사용. 한글 표제란 OCR 성능은 확인 필요(eDOCr2는 Tesseract `kor` 데이터 필요).

### 3.5 STEP 파싱 기반 BOM — 독립 교차검증용 2차 원천으로 적합

- **공통 메커니즘**: OCCT XDE/XCAF — `STEPCAFControl_Reader` → `XCAFDoc_ShapeTool` → `GetComponents` → `IsReference`/`GetReferredShape`. 수량은 동일 원형(prototype)을 가리키는 참조 라벨 수로 **수신 측이 재계산**해야 함(STEP에는 수량 필드 없음) ([OCCT xde.md](https://raw.githubusercontent.com/Open-Cascade-SAS/OCCT/master/dox/user_guides/xde/xde.md)).

| 프로젝트 | 용도 | 성숙도 | 라이선스 |
|---|---|---|---|
| [Open-Cascade-SAS/OCCT](https://github.com/Open-Cascade-SAS/OCCT) | STEP AP203/214/242 기준 구현, PMI(`XCAFDoc_DimTolTool`) | 약 3.0k★ | LGPL-2.1 + 예외(재확인 안 함) |
| [CadQuery/cadquery](https://github.com/CadQuery/cadquery) `importers/assembly.py` | XCAF 순회·이름·재질·색상 추출의 간결한 Python 참조 구현 | 약 6.0k★, PyPI 2.8.0(2026-06-21) | Apache-2.0 |
| [CadQuery/OCP](https://github.com/CadQuery/OCP) | pip 설치 OCCT 바인딩(`cadquery-ocp`) | 약 205★ | Apache-2.0 |
| [tpaviot/pythonocc-core](https://github.com/tpaviot/pythonocc-core) | OCCT 전체 래퍼(conda) | 약 2.0k★ | LGPL-3.0(재확인 안 함) |
| [usnistgov/SFA](https://github.com/usnistgov/SFA) | STEP → 스프레드시트, PMI·검증속성 적합성 검사 | 198★ | 미국 정부 저작물(저장소 확인 필요) |
| [mozman/steputils](https://github.com/mozman/steputils) | 순수 Python Part 21 파서 | 53★, "PLANNING STATE" | MIT |

- **함정**: 인스턴스명 접미사(`<n>`, `:n`, `_n`) 가변 ([PTC 커뮤니티](https://community.ptc.com/modeling-323/index10.html)), atomic STEP 내보내기 시 부품별 파일 분할 ([SOLIDWORKS 2026 STEP 옵션](https://help.solidworks.com/2026/English/SolidWorks/Sldworks/HIDD_EXPORT_OPTIONS_STEP.htm)), 구성(Configuration)·"BOM에서 제외" 플래그 소실.
- **기회**: SOLIDWORKS 2024+ MBD "Publish to STEP242"로 사용자 정의 속성 내보내기 가능, API `IMBDSTEP242Data.CustomPropertiesToPublish` ([SW 2024 What's New](https://help.solidworks.com/2024/English/WhatsNew/c_wn2024_mbd_exp_cust_prop_step_242.htm)). OCCT의 UDA 읽기(`GetNamedProperties`)는 7.8~7.9 이후로 보고되나 정확한 버전 확인 필요 ([OCCT 포럼](https://dev.opencascade.org/content/new-feature-step-import-user-defined-attributes), 검색 요약).
- **판단**: STEP은 진실 원천이 아니라 **SolidWorks BOM 표 설정이 부품을 숨기지 않았는지 확인하는 독립 대조(golden check)** 용도로 Phase 3 이후 선택 적용.

### 3.6 BOM 교차검증 도구 — 직접 선례는 bomcheck 1개

| 프로젝트 | 매칭 로직 | 스택 | 성숙도 | 라이선스 | 한계 |
|---|---|---|---|---|---|
| [kcarlton55/bomcheck](https://github.com/kcarlton55/bomcheck) (+bomcheckgui) | `PartNo_sw.xlsx` ↔ `PartNo_sl.xlsx` 파일쌍, 다단 BOM(ERP `Level`, CAD `3.1.2` 항목번호)을 조립품별 단단 BOM으로 분해 후 (assy, PN) 비교, **IQDU 플래그**(Item·Qty·Description·UoM), 단위 환산, `bomcheck.cfg` 컬럼 설정 | Python/pandas | PyPI 2.7(2026-08-15), 2★, 2018~ | GPL-3.0-or-later | 단일 저자, 파일명 규칙 강제 |
| [Luis-123-long/BomCadCompare](https://github.com/Luis-123-long/BomCadCompare) | CAD 좌표·BOM·실장기 프로그램 **3자 일치 검사** | C# WPF | 1★ | 확인 필요 | SMT 전용, 3자 대조 패턴 참고 |
| [L-A-Marchetti/BOMulus](https://github.com/L-A-Marchetti/BOMulus) | BOM 버전 간 행 단위 diff | Go | 14★ | 확인 필요 | 전자 BOM |
| [OmniaGit/odooplm](https://github.com/OmniaGit/odooplm) | Odoo PLM, SolidWorks 연동·BOM 리비전 | Python | 164★ | 확인 필요 | Odoo 도입 필요 |

- **업계 공통 점검항목**(대부분 벤더 블로그 근거): PN 존재·완결성, 수량·단위(UoM) 불일치, 리비전 불일치, 품명 불일치, 구조 차이(팬텀·구매 하위조립), 속성 누락 ([OpenBOM](https://www.openbom.com/blog/plm-and-bom-management/bom-review-mistakes-quality-cost-supply-chain), 검색 요약; [Industrios](https://industrios.com/news/read/reduce-bom-errors-cad-erp/)).
- **공백**: 도면 BOM 표(SLDDRW/PDF) ↔ 모델 ↔ ERP를 대조하는 오픈소스 도구는 발견되지 않음.

### 3.7 CAD MCP 서버 — 대부분 쓰기·임의실행 도구 포함, 읽기전용 검토용으로는 부적합

| 서버 | 읽기 도구 | 성숙도 | 라이선스 | 보안·한계 |
|---|---|---|---|---|
| [eyfel/mcp-server-solidworks](https://github.com/eyfel/mcp-server-solidworks) (SolidPilot) | `analyze_model`/`analyze_assembly`/`analyze_drawing`, `compare_parts`, `verify_state` | 405★, 56 커밋, 초기 알파, SW2026 SP3 | AGPL-3.0 + 상용·CLA | localhost:5000 REST **인증 없음**, 쓰기·삭제 도구 존재, BOM·사용자 속성·GD&T 도구 없음 |
| [tylerstoltz/SW_MCP](https://github.com/tylerstoltz/sw_mcp) | `get_model_info`, 속성 get/set | 3★, 2 커밋 | LICENSE 없음 | `execute_solidworks_api` 임의 API 실행, 관리자 권한 실행 권장 |
| [andrewbartels1/SolidworksMCP-python](https://github.com/andrewbartels1/SolidworksMCP-python) | 약 134 도구, `validate_drawing_completeness` 광고 ([Glama](https://glama.ai/mcp/servers/andrewbartels1/SolidworksMCP-python/tools/validate_drawing_completeness)) | 취미 프로젝트 | 확인 필요 | 실효성 확인 필요 |
| [dimitrovakulenko/dwg-mcp-server](https://github.com/dimitrovakulenko/dwg-mcp-server) | `dwg.get_objects`/`query_objects`/`render_view` | 20★, 60 커밋, 4개 플랫폼 CI, 공식 MCP Registry 등재 | GPL-3.0 | **허용 루트 샌드박스, 원본 미저장, 문서별 프로세스 격리** — 모범 보안 설계 |
| [Codemod3/cad-mcp](https://github.com/Codemod3/cad-mcp) | `dxf_get_dimensions`/`dxf_get_text`/`dxf_get_blocks` 등 | 1★, 4 커밋, pytest | MIT | 쓰기는 사본에만, 로컬 stdio |
| [neka-nat/freecad-mcp](https://github.com/neka-nat/freecad-mcp) | 문서 검사·FEM | 약 3.0k★ | MIT | **임의 Python 실행**, 인증 없음 |
| [haunchen/solidworks-mcp](https://github.com/haunchen/solidworks-mcp) | 도면·PDF 생성 | 7★ | 확인 필요 | LAN Streamable HTTP 옵션 — 네트워크 노출 |

- 그 외 jaylamping, MatteoBaisini, just1step, tkhosravi, tmkaio, vespo92 SolidWorks MCP와 puran-water(AutoLISP 자유 실행), U-C4N, bimwright(코드 실행) AutoCAD MCP 존재. 스타·라이선스는 확인 필요 ([목록 출처: 각 GitHub](https://github.com/jaylamping/solidworks-mcp)).
- **판단**: 현재 SolidWorks MCP 중 **"BOM + 사용자 속성 + 도면 치수"를 읽기전용으로 깔끔히 노출하는 서버는 없음**. 필요 시 사내 추출기 위에 읽기전용 도구 4~5개만 노출하는 소형 MCP를 Phase 4에 자체 구축.

### 3.8 LLM/VLM 기반 도면 검토 — 판정용으로는 정확도 미달, 설명·보조용

| 근거 | 결과 |
|---|---|
| [arXiv 2411.03707](https://arxiv.org/abs/2411.03707) (2024-11) | 제로샷 GPT-4o P 59.03%·R 25.39%·F1 35.51%, Claude-3.5 환각률 55.99%. 400장으로 미세조정한 Florence-2(0.23B)가 F1 +52.40%p, 환각 −43.15%p 우위 (검색 요약) |
| [arXiv 2311.12668](https://arxiv.org/pdf/2311.12668) | GPT-4V가 치수 5개는 모두 읽었으나 라벨 정확도 약 2/3 |
| [Enginuity, arXiv 2606.03410](https://arxiv.org/abs/2606.03410) (2026-06) | 도면–부품표 2,056쌍 벤치마크, 부품 식별과 설명 정확도 간 큰 격차 — 도면↔BOM 이해의 가장 가까운 공개 기준 |
| [arXiv 2602.18296](https://arxiv.org/pdf/2602.18296) (2026-02) | LLM 보조로 도면 주석 → 3D 특징 매핑(정확도 수치 확인 필요) |
| [SnapEDA 블로그](https://blog.snapeda.com/2026/02/16/evaluating-the-accuracy-of-ai-generated-cad-models/) (2026-02) | Claude Opus 4.6 92%, Gemini 3 Deep Think 84%(92개 필드, 벤더 블로그·미검증) |

- **지배적 구조**: 검출(YOLO-OBB/OCR 영역분할) → 크롭 단위 판독(소형 미세조정 VLM/OCR) → LLM은 검증·필터·추론만. SolidPilot도 "Planner(IR) → 결정론적 Compiler(LLM 없음) → C# COM 실행" 구조 ([SolidPilot](https://github.com/eyfel/mcp-server-solidworks)).
- **판단**: JUSTEM 자체 도면(SLDDRW)은 구조화 엔티티로 읽으므로 VLM 정확도 문제를 회피 가능. VLM은 고객 PDF·스캔 도면, 주기(Note) 문구 검토, 결과 설명문 생성에 한정.

### 3.9 상용 도구 대비 — 서식·단일문서 검사는 강하나 3자 교차검증·Excel 검토표는 공백

| 도구 | 검사 범위 | 자동화 | 라이선스·제약 | JUSTEM 관점 공백 |
|---|---|---|---|---|
| SOLIDWORKS Design Checker | 문서 설정(치수 표준·폰트·단위), 표준 템플릿·레이어·표제란, **Dangling 치수/주석**, **BOM 풍선 누락**, 겹친 치수, 사용자 속성, 자동수정 ([CATI](https://www.cati.com/blog/solidworksdesign-checker/), [Help 2020 Dangling](https://help.solidworks.com/2020/English/SolidWorks/solidworks_design_checker/r_dangling_dimension_annotation.htm), 검색 요약) | Task Scheduler "Design Checker" 작업, COM `ISWDesignCheck.RunDesignCheck4/5` ([API 2015](https://help.solidworks.com/2015/English/api/dsgnchkapi/SOLIDWORKS.Interop.dsgnchk~SOLIDWORKS.Interop.dsgnchk.ISWDesignCheck~RunDesignCheck5.html)) | Professional/Premium 포함, SolidWorks 실행 필요 | HTML 보고서, ERP 대조·누락 치수·공차 값 검증 없음(공차 검사 존재 여부 확인 필요) |
| SOLIDWORKS PDM 전이 조건 | 데이터카드 변수·리비전 조건으로 상태 전이 차단 ([PDM Help 2018](https://help.solidworks.com/2018/English/EnterprisePDM/Admin/t_workflow_transition_conditions.htm)) | 워크플로 게이트 | BOM/Item 조건은 PDM Professional | 검토표가 아닌 통과/차단 |
| CADLink, BOM Creator, Global Shop BOM Compare | CAD BOM ↔ ERP BOM 비교 ([CADLink](https://files.solidworks.com/partners/pdfs/cadlink-brochure_solidworks_2021653.pdf), [Global Shop](https://www.globalshopsolutions.com/bom-compare-software-for-manufacturing)) | ERP 연동 | 특정 ERP 종속 | 도면 표제란·BOM 표 대조 없음 |
| Xarial CAD+ (Batch+) | 배치 매크로·속성 편집 ([SOLIDWORKS 파트너](https://solidworks.com/partner-product/cad-toolset)) | CLI 배치 실행 | 커뮤니티판 무료(오픈소스) | 검사기 아님, 실행 하네스로 활용 가능 |
| AutoCAD CAD Standards / Creo ModelCHECK / NX Check-Mate | 스타일·파라미터·드로잉 검사, Creo+ 2026-05 DRIVEN/DRAFT/ERASED_DIMENSIONS 추가 ([PTC](https://support.ptc.com/help/creo/creo_plus/usascii/whats_new/mdlchk_new_checks.html)) | 배치 | 각 CAD 종속 | 규칙파일 방식 참고용 |
| Werk24, CoLab AutoReview, Leo AI, CADDi Drawer | 이미지·PDF 추출/AI 검토/유사도면 검색 ([werk24.io](https://werk24.io/), [Design News](https://www.designnews.com/artificial-intelligence/ai-tools-for-engineers-autoreview)) | API/SaaS | 클라우드, 독립 정확도 검증 없음 | 고객 도면 NDA 충돌 소지 |

- **사용자 불만 근거**: 누락 치수 검출 가능 여부 문의에 명확한 답 없음 ([3DSwym](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/check-drawing-for-missing-dimensions_DKMJvsGCQneUMYQj7-cdtw)), 결재 전 자동검사용 "Design Checker 대안" 요청 ([myCAD 포럼](https://forum.mycad.visiativ.com/t/controle-automatique-des-mises-en-plan-avant-validation-alternative-a-design-checker?locale=en)).
- **판단**: Design Checker를 대체하지 말고 **서식·Dangling 검사는 `RunDesignCheck5`를 pywin32로 호출해 재사용**, 자체 개발 노력은 3자 교차검증·도메인 규칙·Excel 검토표에 집중.

---

## 4. 검사 항목별 구현 기법 매핑 (SolidWorks API 중심)

- 표의 "검증 상태": ●=소스코드·공식 도움말로 확인, ◐=검색 요약·포럼 근거, ○=조사자 배경지식(해당 SW 버전 도움말로 **확인 필요**).

| 검사 항목 | 추출 메서드·경로 | 검증 상태 | 함정 |
|---|---|---|---|
| 도면 순회 | `GetFirstView`/`GetNextView`(첫 뷰는 시트 자체) | ● [THERMOPAC](https://github.com/THERMOPAC/ThermopacCommunication-R1/blob/HEAD/local-agent/extractor/extract_dimensions.py) | 시트 전환 필요 |
| 치수 값 | `GetFirstDisplayDimension5`/`GetNext5`(폴백 `GetNext4`/`GetNext`) 또는 `GetDisplayDimensions` → `GetDimension2(0)` → `GetSystemValue2("")`(m 단위) | ● THERMOPAC, [Help 예제](https://help.solidworks.com/2021/english/api/sldworksapi/get_dimension_values_in_drawing_example_csharp.htm) | 번호 붙은 메서드가 버전마다 다름 |
| 공차 유무·값 | `GetTolerance()` → `.Type`(0=없음), `MinValue`/`MaxValue` | ● THERMOPAC | 일반공차 적용 대상 판정 규칙은 사내 정의 필요 |
| 참조(Driven) 치수 | `IsReference()` | ● THERMOPAC | — |
| Dangling | `IAnnotation.IsDangling`(주석 객체), `GetFirstAnnotation3`/`GetNext3` 순회 | ◐ [Visiativ 포럼](https://forum.mycad.visiativ.com/t/comment-supprimer-les-cotes-bancales-en-vba-sur-solidworks2016/104848) | DisplayDimension 순회만으로는 노트·GTol 누락 |
| Override | `IDisplayDimension.GetOverride`(불리언) + `GetText(Prefix/Suffix/CalloutAbove/Below)` 재조합 | ◐ [3DSwym](https://3dswym.3dexperience.3ds.com/post/solidworks-user-forum/get-dimension-override-value-text-api_1hJFPIE3T6C51lPeVN3Daw), ○ | Override 텍스트 값 직접 취득 방법 미해결, `swDimensionTextAll`은 SetText 전용(검색 요약) |
| GD&T | `IView.GetFirstGTOL` → `IGtol.GetNextGTOL`, `GetFrameValues`/`GetFrameSymbols3`, `GetFirstDatumTag` | ○ | 확인 필요 |
| 표제란 | 시트 포맷 노트 `INote.GetText`, 링크 속성 `$PRP:"..."`/`$PRPSHEET:"..."` + 모델/도면 사용자 속성 `CustomPropertyManager(config).Get6` | ○ | 확인 필요, 구성별 속성 차이 |
| 도면 BOM 표 | `IView.GetTableAnnotations` → `ITableAnnotation`(`Type`, `RowCount`, `DisplayedText(r,c)`, `RowHidden`), `IBomTableAnnotation.GetComponents2(row, config)` | ◐/○ | `IBomFeature::GetTableAnnotations` 빈 배열 반환 보고 ([3DSwym](https://3dswym.3dexperience.3ds.com/question/solidworks-user-forum/gettableannotations-method-ibomfeature-returns-empty-array_A0wbYZ9KTk6_e-_WNAw76A)) |
| 리비전표 | `ISheet.RevisionTable` → `IRevisionTableAnnotation` | ○ | 확인 필요 |
| 속성 대량 읽기(SW 미실행) | DM `SwDMClassFactory.GetApplication(key)` → `GetDocument` → `GetCustomPropertyNames`/`GetCustomProperty`, `GetAllExternalReferences5`(폴백 4), 표 셀 텍스트 예제 존재 | ● [sw_drawing_studio](https://github.com/yzlin286-wq/sw_drawing_studio/blob/HEAD/app/services/docmgr_service.py), ◐ [DM Help](https://help.solidworks.com/2015/English/api/swdocmgrapi/SOLIDWORKS.Interop.swdocumentmgr~SOLIDWORKS.Interop.swdocumentmgr.ISwDMDocument~GetCustomProperty.html) | 값은 **마지막 저장 시점** 평가값, 무효 키 시 "ActiveX can't create the object" ([포럼 18091](https://forum.solidworks.com/thread/18091)) |
| pywin32 ByRef | `OpenDoc6`의 Errors/Warnings를 `VARIANT(VT_BYREF|VT_I4, 0)`로 전달 | ◐ [OpenDoc6 Help](https://help.solidworks.com/2015/english/api/sldworksapi/SolidWorks.Interop.sldworks~SolidWorks.Interop.sldworks.ISldWorks~OpenDoc6.html) | 정수 전달 시 "Type mismatch" |

---

## 5. JUSTEM 적용 시사점

### 5.1 재사용 가능한 구성요소

| 구성요소 | 재사용 방식 | 라이선스 | 비고 |
|---|---|---|---|
| THERMOPAC `extract_dimensions.py` 패턴 | 다중 메서드명 폴백·소프트 실패·dict 반환 패턴 차용 | 확인 필요 → **코드 복사 금지, 패턴만** | 라이선스 확인 전까지 재구현 |
| smart_SW 구조 | IR → 결정론적 검사 → session/check/attention JSON → 보고서, YAML 표준 프로파일 | AGPL-3.0 → **아키텍처 참고만** | — |
| bomcheck 로직·보고서 레이아웃 | 다단 BOM 분해, UoM 환산, `_sw`/`_sl` 병렬 컬럼 + IQDU 플래그 | GPL-3.0 → **아이디어 재구현** 또는 외부 도구로 별도 실행 | — |
| xCAD.NET / codestack | C# 추출기 선택 시 프레임워크·예제 | MIT | 저작권 고지 유지 |
| ezdxf + ODA File Converter | 외부 DXF/DWG 수신 도면 판독 | ezdxf MIT, ODA 무료(약관 확인 필요) | — |
| CadQuery `importers/assembly.py`, OCP | STEP XCAF 순회 참조 | Apache-2.0 | 고지·NOTICE 유지 |
| eDOCr2 | 스캔·레거시 도면 GD&T/치수 OCR | MIT | 모델 가중치 라이선스 확인 필요 |
| pdfplumber / PaddleOCR | 벡터 PDF 좌표 텍스트 / 한글 OCR | pdfplumber 확인 필요, PaddleOCR 확인 필요 | PyMuPDF는 AGPL이므로 회피 권장 |
| iso7200-cad-standard | 표제란 속성 스키마·감사 룰북 참고 | Apache-2.0 | — |
| openpyxl 3.1.5 + pandas | Excel 검토표(조건부서식, 드롭다운, 틀고정, 행 그룹) ([openpyxl](https://openpyxl.readthedocs.io/en/stable/)) | MIT | — |
| rapidfuzz | 미매칭 행에 한해 PN·품명 유사도 "후보 제안" | MIT(조사 안 함, 확인 필요) | 자동 매칭 금지 |
| Design Checker `RunDesignCheck5` | 기존 `.swstd`로 서식·Dangling 검사 재사용, 결과 파싱 | SW Professional/Premium 포함 | 보고서 형식 확인 필요 |

### 5.2 권장 아키텍처 — 4계층 분리, 판정은 결정론적 코드만

```
[입력]
 SLDDRW/SLDASM/SLDPRT ──► (A) 추출기: pywin32 + SolidWorks API (필요 시 DM API 병행)
 SW BOM Excel ───────────┐      └─ 도면·모델 1건당 JSON 1개 (schema_version 포함)
 ERP BOM Excel ──────────┤
 (선택) 외부 DXF/PDF/STEP ─► (A') 보조 추출기: ezdxf / pdfplumber / OCP
                         ▼
                 (B) 정규화 계층: pydantic IR, 단위(mm) 통일, PN 정규화
                         ▼
                 (C) 규칙엔진: YAML 룰북(규칙 ID·심각도·대상·근거) + Python 검사함수
                         ▼
                 (D) 보고 계층: openpyxl Excel 검토표 (+선택: LLM 설명문, 사내/무보존 채널)
```

- **(A) 추출기 원칙**
  - • `DispatchEx`로 별도 SolidWorks 인스턴스 기동, `Visible=False`, `OpenDoc6` Silent+ReadOnly, 파일마다 `CloseDoc`, N개 파일마다 재기동, 파일 단위 서브프로세스 + 타임아웃(모달 대화상자 고착 사례: [solidworks-bridge](https://github.com/infei12306/solidworks-bridge)). 이 항목들은 조사자 권장안이며 사내 SW 버전에서 검증 필요.
  - • 추출기는 **절대 저장·수정하지 않음**(smart_SW의 read-only guard 원칙).
  - • 모든 추출 항목에 출처 키(시트명·뷰명·주석명·표 행/열) 포함 → Excel에서 역추적 가능.
- **(B) 정규화**: PN은 NFKC 정규화(전각 문자 처리), 공백 제거, 대문자화, 하이픈 변형(‐ - – —) 통일, 리비전 접미사 분리. 단위는 SI→mm.
- **(C) 규칙엔진**: 규칙은 코드가 아닌 YAML로 관리해 설계팀이 심각도·적용범위를 조정. 각 결과에 `rule_id, severity, object_ref, expected, actual, evidence` 필드.
- **(D) 보고**: LLM을 쓰더라도 Excel의 판정 컬럼은 규칙엔진 값만 사용, LLM 문장은 별도 "설명" 컬럼에 엔티티 ID 인용 강제.

### 5.3 1차 규칙셋(예시) — 도면 체크

| 규칙 ID | 내용 | 원천 |
|---|---|---|
| TB-01~05 | 표제란 필수 필드(도번·품명·재질·표면처리·리비전·작성/검도/승인·날짜) 공란 여부, 도번 정규식 일치 | 도면/모델 사용자 속성 |
| TB-10 | 표제란 리비전 = 모델 속성 리비전 = 리비전표 최신행 = ERP 리비전 | 3자 대조 |
| DIM-01 | Override 치수 목록화(`GetOverride=True`) — 경고 | Display Dimension |
| DIM-02 | Dangling 치수·주석(`IsDangling`) — 오류 | Annotation |
| TOL-01 | 지정 특성(끼워맞춤·베어링부 등)에 공차 없음 — 사내 기준 정의 필요 | `GetTolerance().Type` |
| GDT-01 | GD&T 프레임의 데이텀 참조가 도면 내 데이텀 태그에 존재하는지 | GTOL/DatumTag(확인 필요) |
| NOTE-01 | 클린룸·표면처리 표준 주기 존재 여부 | Note 텍스트 |
| STD-xx | 서식·폰트·레이어·BOM 풍선 | Design Checker `.swstd` 결과 흡수 |

### 5.4 BOM 교차검증 로직 — 키 기반 외부조인, 퍼지는 제안만

- **절차**
  1. 소스별 정규화 후 (부모 PN, 자식 PN, 개당수량, UoM) 엣지로 분해(ERP `Level`, SW 항목번호 `3.1.2` 활용 — bomcheck 방식).
  2. 동일 부모 하 중복 자식 행 합산(구성·"부품만/들여쓰기" 보기 차이 흡수).
  3. (부모 PN, 자식 PN) **완전 외부조인** → 분류: CAD에만 / ERP에만 / 도면 BOM에만 / 수량 / UoM / 품명 / 리비전 불일치.
  4. 전개(flatten) 총수량 비교로 팬텀·구매 하위조립 구조 차이를 "구조 차이"로 별도 분류.
  5. 미매칭 행에만 rapidfuzz 후보 제안(O/0, 자리바꿈) — 사람 확인 필수.
- **존재 비트마스크**: bomcheck의 IQDU에 착안해 `C/D/E`(CAD 모델·도면 BOM 표·ERP) 3자 존재 플래그 컬럼 추가 ([BomCadCompare 3자 패턴](https://github.com/Luis-123-long/BomCadCompare)).
- **반도체 장비 특화 점검**: 케이블·튜브·알루미늄 프로파일의 EA vs M 단위, 구매품 Make/Buy·재질 일치, "BOM에서 제외"·가상·엔벨로프·억제 부품 처리 일관성, 동일 품명 상이 PN(중복 품목 생성 의심), 질량 vs ERP 중량(선택).
- **주의**: SW BOM Excel은 "인스턴스 수량 합산" 등 내보내기 옵션에 따라 형태가 달라짐 ([DriveWorks](https://docs.driveworkspro.com/Topic/GTExportBOM)). 템플릿·옵션을 고정하고, 가능하면 API로 BOM 표를 직접 읽어 Excel 내보내기 단계의 수작업 오류를 제거.

### 5.5 Excel 검토표 설계

- 시트 구성: `요약`(규칙별·심각도별 건수) / `도면검사`(1행=1결과) / `BOM대조`(`_SW`/`_DRW`/`_ERP` 병렬 컬럼 + 플래그) / `원본데이터` / `규칙정의`.
- 컬럼: 규칙ID·심각도·도번·시트·뷰·대상·기대값·실제값·근거·**검토자 처리**(DataValidation 드롭다운: OK / 도면수정 / ERP수정 / 예외승인)·검토자·코멘트·처리일(YYYY-MM-DD).
- 조건부서식으로 오류/경고 색상, 틀고정·자동필터, 다단 BOM은 행 그룹 처리 ([openpyxl](https://openpyxl.readthedocs.io/en/stable/)).

---

## 6. 라이선스 재사용 주의점

| 라이선스 | 해당 사례 | 사내 전용 사용 | 주의점 |
|---|---|---|---|
| AGPL-3.0 | smart_SW, SolidPilot, PyMuPDF | 사내 실행 자체는 일반적으로 가능 | **네트워크 서비스로 제공(사내 웹 포털 포함 여부 법무 확인 필요)하거나 외부 배포 시 전체 소스 공개 의무** 발생 가능. 코드 복사·링크 금지, 구조만 참고 권장 |
| GPL-3.0 | bomcheck, LibreDWG, dwg-mcp-server, pySolidWorks | 사내 전용은 일반적으로 무방 | 협력사·고객·자회사 배포 시 결합 저작물 소스 공개 의무. 독립 프로세스로 호출하거나 재구현 |
| LGPL | OCCT, pythonocc-core | 동적 링크 사용 시 부담 적음 | 수정 시 해당 라이브러리 소스 공개 |
| MIT / Apache-2.0 | ezdxf, xCAD.NET, codestack, CadQuery, OCP, eDOCr2, openswx, openpyxl | 자유 | 저작권·라이선스 고지 유지, Apache는 NOTICE·특허 조항 |
| 라이선스 미표시 | Bakkopi, tylerstoltz/SW_MCP 등 | **사용 불가로 간주** | 기본 저작권 보호 — 코드 복사 금지 |
| 상용 | Werk24, Design Checker, ODA File Converter | 약관 따름 | ODA 무료 변환기 재배포·상업 이용 조건 확인 필요, SolidWorks 무인(unattended) COM 자동화 EULA 조건 확인 필요 |

- **원칙**: 배포 가능성이 조금이라도 있으면 GPL/AGPL 코드는 "읽고 재구현(clean-room)"으로 처리하고 의존성 목록(SBOM)과 라이선스 점검을 CI에 포함.

---

## 7. 보안 리스크 — 고객 도면 기밀 보호

| 리스크 | 근거 | 대응 |
|---|---|---|
| 임의 코드/API 실행 MCP 도구 | `execute_solidworks_api`([SW_MCP](https://github.com/tylerstoltz/sw_mcp)), FreeCAD 임의 Python([freecad-mcp](https://github.com/neka-nat/freecad-mcp)), AutoLISP 자유 실행([autocad-mcp](https://github.com/puran-water/autocad-mcp)) | 검토용 환경에서 해당 도구 제거·비활성화, 읽기전용 도구만 노출 |
| 도면 텍스트 내 프롬프트 인젝션 | 주기·표제란 문자열이 LLM 입력으로 전달됨 | LLM은 판정 권한 없음, 도구 호출 권한 없음, 입력은 데이터로만 취급 |
| 인증 없는 로컬/LAN 엔드포인트 | SolidPilot localhost:5000 REST, haunchen LAN HTTP 모드 | localhost 바인딩·방화벽, 원칙적으로 MCP/HTTP 미사용(배치 CLI 우선) |
| 미고정 코드 실행 | `uvx` git URL 실행 | 커밋 고정·사내 미러(vendoring) |
| 클라우드 LLM/SaaS 전송 | MCP 서버가 로컬이어도 추출 내용은 LLM 공급자로 전송, Werk24·CADDi 등 해외 클라우드 | 고객 NDA 검토, 사내 온프레미스 모델 또는 무보존(Zero Data Retention) 계약 채널만 허용, 기본값 LLM 비활성 |
| 샌드박스 부재 | — | dwg-mcp-server식 허용 루트 제한·원본 미저장·문서별 프로세스 격리 차용 ([dwg-mcp-server](https://github.com/dimitrovakulenko/dwg-mcp-server)) |
| 비밀정보 노출 | DM 라이선스 키 | 환경변수·설정파일 분리, 저장소 커밋 금지 ([sw_drawing_studio 패턴](https://github.com/yzlin286-wq/sw_drawing_studio/blob/HEAD/app/services/docmgr_service.py)) |
| 외부 실행파일 경로 변조 | ezdxf odafc 문서 경고 ([ezdxf](https://ezdxf.readthedocs.io/en/stable/addons/odafc.html)) | ODA 경로 고정·권한 제한 |
| 산출물 유출 | JSON·Excel에 도면 정보 전량 포함 | 산출물 저장 위치 접근통제, 보존기간 정의 |

---

## 8. 단계별 실행안

| 단계 | 기간(안) | 목표 | 주요 작업 | 완료 기준 |
|---|---|---|---|---|
| Phase 0 기반 정리 | 2주 | 범위·규칙·샘플 확정 | 사내 SW 버전·라이선스 등급(Design Checker 유무)·PDM 여부 확인, 도면 체크리스트 → 규칙 ID화, 대표 도면 20~30건·BOM Excel·ERP Excel 샘플 확보(정답표 포함), DM 라이선스 키 신청 | 규칙 YAML 초안, 정답 데이터셋 |
| Phase 1 BOM 교차검증 MVP | 3~4주 | 가장 빠른 효과 | SW BOM Excel ↔ ERP Excel 정규화·외부조인·IQDU식 플래그·Excel 검토표(SolidWorks 불필요) | 샘플 BOM 누락·수량·UoM 불일치 100% 검출, 오탐 목록화 |
| Phase 2 도면 추출기 + 규칙 | 6~8주 | 도면 체크 자동화 | pywin32 추출기(시트·뷰·치수·공차·Override·Dangling·노트·BOM 표·리비전표·속성) → JSON 스키마 v1, 규칙 TB/DIM/TOL, Design Checker `RunDesignCheck5` 결과 흡수 | 정답 데이터셋 대비 재현율·오탐률 측정 |
| Phase 3 3자 대조·배치화 | 4주 | 모델↔도면↔ERP 통합 | 도면 BOM 표 ↔ 어셈블리 ↔ ERP C/D/E 플래그, 폴더 단위 배치·재기동·타임아웃, (선택) DM 기반 고속 속성 스캔, (선택) STEP 독립 대조 | 프로젝트 1개 전체 BOM 무인 처리 |
| Phase 4 확장(선택) | 별도 | 외부 도면·AI 보조 | 고객 DXF(ezdxf)·벡터 PDF(pdfplumber) 경로, 스캔 도면 eDOCr2/PaddleOCR, 사내 LLM으로 결과 설명문·주기 문구 검토, 필요 시 읽기전용 사내 MCP | 보안 검토 통과 후 시범 적용 |

- **기간은 조사자 추정치이며 팀 인력·SW 버전에 따라 조정 필요(확인 필요)**.
- Phase 1을 먼저 하는 이유: SolidWorks COM 의존이 없어 위험이 낮고, bomcheck라는 검증된 선례가 있으며, 결과가 즉시 ERP 오류 감소로 연결됨.

---

## 9. 미확인·추가 확인 필요 사항

- • GD&T(`GetFirstGTOL` 등)·리비전표·표제란 노트 API 메서드명과 버전 차이 — 사내 SW 버전 API 도움말로 확인 필요(help.solidworks.com 접속 차단으로 미검증).
- • Override 치수의 표시 텍스트 값을 안정적으로 읽는 방법 — 포럼에서도 미해결.
- • DM API 표 기능이 BOM/리비전표를 구분하는지, 표시값/원시값 중 무엇을 반환하는지.
- • Design Checker의 공차 값·GD&T 내용 검사 지원 여부, `RunDesignCheck5` 보고서 형식 목록, SOLIDWORKS 2026 변경사항.
- • SOLIDWORKS 무인 COM 자동화 및 서버 사용 EULA 조건, DM 라이선스 키 신청 절차(2026).
- • SolidWorks DXF/PDF 내보내기 시 DIMENSION 엔티티 유지 여부, 한글 폰트 텍스트 유지 여부.
- • THERMOPAC·EdmBOM_CSharp·bom_check·xlBOM 저장소 라이선스 및 내부 코드.
- • 국내 리셀러의 모델·도면·ERP 통합 검사 상용 애드인 존재 여부(개발 착수 전 확인 권장).
- • 처리량(파일/시간) 벤치마크 — 공개 자료 없음, 자체 측정 필요.

---

## 10. 실행 체크리스트

- [ ] 사내 SolidWorks 버전·라이선스 등급(Professional/Premium 여부, Design Checker 사용 가능 여부)·PDM 사용 여부 확인
- [ ] SOLIDWORKS Document Manager API 라이선스 키 신청 및 환경변수 보관 방식 확정
- [ ] 현행 도면 체크리스트를 규칙 ID·심각도·원천 필드로 변환한 YAML 룰북 v0 작성
- [ ] 대표 도면 20~30건 + SW BOM Excel + ERP Excel 샘플과 사람이 작성한 정답 검토표 확보
- [ ] SW BOM Excel 내보내기 템플릿·옵션(인스턴스 수량 합산 등) 표준화
- [ ] Phase 1: PN 정규화 규칙(NFKC·하이픈·리비전 분리) 및 BOM 외부조인·플래그·Excel 검토표 MVP 구현
- [ ] bomcheck(GPL) 로직은 코드 복사 없이 재구현, 의존성 라이선스 점검 CI 추가
- [ ] Phase 2: pywin32 추출기 JSON 스키마 v1 정의(출처 키·schema_version 포함)
- [ ] 치수·공차·Override·Dangling·노트·BOM 표·리비전표 API 메서드를 사내 SW 버전 도움말로 검증
- [ ] 추출기 읽기전용 원칙(ReadOnly 열기, 저장 금지)·서브프로세스 타임아웃·주기적 재기동 구현
- [ ] Design Checker `RunDesignCheck5` 호출 및 결과 파싱 PoC(가능한 경우)
- [ ] 정답 데이터셋 대비 재현율·오탐률 측정 후 규칙 임계값 조정
- [ ] Phase 3: 모델↔도면 BOM 표↔ERP 3자 대조(C/D/E 플래그) 및 폴더 배치 처리
- [ ] (선택) STEP AP242 독립 대조 PoC(CadQuery/OCP, Apache-2.0)
- [ ] 보안 정책 수립: 고객 도면의 클라우드 LLM/SaaS 전송 금지 기본값, 임의 실행 MCP 도구 사용 금지, 산출물 접근통제
- [ ] Phase 4 착수 전 법무·보안 검토(AGPL/GPL 의존성, 고객 NDA, ODA·SolidWorks 약관)
- [ ] 국내 리셀러 대상 통합 검사 상용 애드인 존재 여부 확인
