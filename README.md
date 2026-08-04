# w-lighter_model-server

> **w.LiGHTER**의 **MODEL 서버(FastAPI)** 레포입니다.   
> AI 번역 파이프라인과 창작 지원 기능(현지화 가이드·표지 이미지·캐릭터 설정집/관계도)의 모델 추론을 전담합니다.   
> 이 레포는 **팀 프로젝트([원본](https://github.com/IsekaiPD/w-lighter_model-server))를 포트폴리오용으로 재구성·확장해 개인적으로 관리하는 미러**입니다.   
> 프로젝트 전체 소개(기획 배경·화면·비즈니스 모델)는 팀 레포를 참고해 주세요 → [SKN24-FINAL-5Team](https://github.com/SKNETWORKS-FAMILY-AICAMP/SKN24-FINAL-5Team).

---

# 1. 팀 소개

### 팀명 : 이세계출판부

| <img src="https://github.com/Akoh-0909.png" width="180" height="180"> | <img src="https://github.com/min3802.png" width="180" height="180"> | <img src="https://github.com/shortcut-2.png" width="180" height="180"> | <img src="https://github.com/LeeDongMin0115.png" width="180" height="180"> | <img src="https://github.com/gugu-eightyone.png" width="180" height="180"> | <img src="https://github.com/lifeisgoodlg.png" width="180" height="180"> |
|:---:|:---:|:---:|:---:|:---:|:---:|
| **고아라** | **권민제** | **김유진** | **이동민** | **진세형** | **최현진** |
| 기획/설계 | AI 모델 개발 | 프론트/인프라 | 시스템/DB | AI 모델 개발 | 백엔드/인프라 |
| [![Akoh-0909](https://img.shields.io/badge/Akoh--0909-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/Akoh-0909) | [![min3802](https://img.shields.io/badge/min3802-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/min3802) | [![shortcut-2](https://img.shields.io/badge/shortcut--2-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/shortcut-2) | [![LeeDongMin0115](https://img.shields.io/badge/LeeDongMin0115-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/LeeDongMin0115) | [![gugu-eightyone](https://img.shields.io/badge/gugu--eightyone-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/gugu-eightyone) | [![lifeisgoodlg](https://img.shields.io/badge/lifeisgoodlg-181717?style=for-the-badge&logo=github&logoColor=white)](https://github.com/lifeisgoodlg) |

# 2. 프로젝트 개요

**w.LiGHTER**는 해외 진출을 준비하는 국내 웹소설 작가를 위한 **AI 기반 웹소설 현지화 번역 어시스턴트**입니다. 한국어 원문을 **영어·중국어·일본어·태국어** 4개 언어로 **다국어 번역**하고, 번역 과정에서 문화권 오해·민감 표현을 사전 검수합니다. 나아가 **현지화 가이드, 표지 이미지, 캐릭터 설정집/관계도**까지 창작 워크플로우 전반을 지원합니다.

서비스는 **WEB 서버(Django)** 와 **MODEL 서버(FastAPI)** 로 분리된 구조이며, 이 저장소는 그중 **MODEL 서버**입니다. LangGraph 기반 멀티 에이전트 파이프라인과 RAG 검색 등 AI 추론 파이프라인을 REST API로 제공합니다.

# 3. 기술 스택

| 분류 | 기술 |
|:---:|---|
| **Backend** | ![Python](https://img.shields.io/badge/Python-3776AB?style=for-the-badge&logo=python&logoColor=white) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=for-the-badge&logo=fastapi&logoColor=white) |
| **AI / Agent** | ![LangGraph](https://img.shields.io/badge/LangGraph-1C3C3C?style=for-the-badge&logo=langgraph&logoColor=white) ![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white) |
| **Database** | ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=for-the-badge&logo=mysql&logoColor=white) ![Qdrant](https://img.shields.io/badge/Qdrant-DC244C?style=for-the-badge&logo=qdrant&logoColor=white) |
| **Infrastructure** | ![Docker](https://img.shields.io/badge/Docker-2496ED?style=for-the-badge&logo=docker&logoColor=white) ![Amazon EC2](https://img.shields.io/badge/Amazon%20EC2-FF9900?style=for-the-badge&logo=amazonec2&logoColor=white) |

### 사용 모델

| 분류 | 모델 |
|:---:|---|
| Embedding | ![nlpai-lab/KURE-v1](https://img.shields.io/badge/nlpai--lab%2FKURE--v1-FFD21E?style=for-the-badge&logo=huggingface&logoColor=black) |
| LLM | ![GPT-5.4-mini](https://img.shields.io/badge/GPT--5.4--mini-412991?style=for-the-badge&logo=openai&logoColor=white) ![GPT-5.4-nano](https://img.shields.io/badge/GPT--5.4--nano-412991?style=for-the-badge&logo=openai&logoColor=white) |
| Image Generation | ![GPT-image-2](https://img.shields.io/badge/GPT--image--2-412991?style=for-the-badge&logo=openai&logoColor=white) |

# 4. 번역 파이프라인

한 번의 LLM 호출에 번역 전체를 맡기는 대신, **번역가 1 · 전문 검수자 4 · 최종 수정가 1**로 역할을 나눈 "번역 편집팀"을 **LangGraph 그래프(12노드)** 로 구현했습니다.

> 번역 요청 → **작품 용어집 조회**(설정 일관성) → 초벌 번역 → **검수 4종 병렬**(말투·문체·문화·용어) → 검수 취합 → 최종 번역 → **한글 잔류 점검·수리**(최대 2회) → **한국 문화 미주**(한국 문화 RAG / Qdrant) → 결과 패키징 → 응답(최종 번역 + 리포트)

<img width="3480" height="2300" alt="번역_파이프라인_구조도" src="https://github.com/user-attachments/assets/d93f8cac-fdab-42cd-be38-1e0f0db8c78b" />

### 설계 포인트

- **2갈래 병렬 구조** — 번역 본류(초벌 → 검수 → 최종 번역)와 **문화 각주 갈래**(한국 문화 RAG → 독자용 미주)가 병렬로 진행됩니다.
- **검수 4종 병렬(fan-out/fan-in)** — 말투·문체·문화·용어를 각각 전담하는 검수 에이전트가 동시에 검토하고, 최종 번역 단계가 의견을 취사선택해 결정 근거와 함께 최종본을 냅니다. 확정 LLM 호출은 6회(초벌 1 + 검수 4 + 최종 1)이며 검수가 병렬이라 체감 지연은 1회 수준입니다.
- **RAG 문화 각주** — 원문 속 한국 고유 표현(수능·제사 등)을 Qdrant 벡터DB에서 **검색**(생성이 아님)해 독자용 미주를 답니다. 번역문에는 주입하지 않아 번역 경직을 방지합니다.
- **용어집 일관성** — 작품×국가별 용어집에서 **이번 회차 원문에 등장하는 항목만** 골라 강제 주입하고, 신규 고유명사는 자동 추출 후 **제안만** 합니다(DB 반영은 사용자 확인 후).
- **구조적 출력** — 전 단계 JSON schema strict 강제로 파싱 실패·필드 누락을 구조적으로 차단하고, 프롬프트는 코드와 분리해 외부화했습니다(`prompts/*.md`).

핵심 코드: [`model_server/domains/translation/`](model_server/domains/translation/) · 그래프 정의: [`engine/graph_orchestrator.py`](model_server/domains/translation/engine/graph_orchestrator.py)

# 5. 임베딩 모델 선정 (정량 평가)

문화 각주 RAG의 임베딩 모델은 유행이 아니라 **직접 측정**으로 선정했습니다. 3개 모델(**multilingual-E5** / **KURE-v1** / **F2LLM-v2-1.7B**)을 5개 데이터셋(한국 문화 RAG 494문서·50쿼리 + 관용표현 4개국)에서 비교 평가했습니다.

- **지표** — Hit@1/3/5/10 · MRR · 정답–오답 score 분리도 · Top-5 threshold F1 · latency(avg/p95)
- **판단** — F2LLM은 품질 상한이 가장 높았으나 지연이 약 6~7배로 실서비스 부적합, E5는 점수 분포가 좁아 threshold 필터링에 불리했습니다. 한국어 질의→한국어 문서 구조에서 품질·지연·threshold 운용성 균형이 가장 좋은 **KURE-v1을 채택**했습니다.
- **반영** — 검색 threshold **0.55**를 코드에 반영했습니다(`annotation_score_threshold`).

# 6. 프로젝트 구조

```
model_server/
├─ main.py                  # 진입점: create_app() → health + /api/v1 마운트
├─ core/                    # 설정(pydantic-settings, .env)·lifespan·logging
├─ common/                  # deps·exceptions·middleware(CORS)
├─ api/v1/router.py         # 도메인 라우터 집계 (prefix=/api/v1)
├─ db/                      # 세션·모델·repository
├─ domains/
│  ├─ translation/          # ★ 번역 파이프라인 (engine·agents·retrieval·glossary·prompts)
│  ├─ guide/                # 현지화 가이드
│  ├─ cover/                # 표지 이미지
│  ├─ relationship_map/     # 캐릭터 관계도
│  └─ character_extract/    # 캐릭터 설정집
└─ workers/                 # 백그라운드 작업
```

새 기능은 `domains/<name>/{schemas, service, router}.py` 패턴을 따릅니다.

# 7. 실행 방법

```bash
# 1) 환경 변수 준비 — 저장소 루트의 .env.example을 참고해 model_server/.env 작성
# 2) 실행
cd model_server
uvicorn main:app --reload        # → http://localhost:8000/docs
```

- **Mock 모드** — `WLIGHTER_MOCK_MODE=true` 설정 시 외부 의존성(OpenAI 키·Qdrant·KURE) 없이 번역 파이프라인 전 구간(router → service → engine)을 확인할 수 있습니다.
- 기본 확인 엔드포인트 — `GET /health` · `POST /api/v1/translation/translate`
- Docker 배포 구성은 `docker-compose.yml`을 참고해 주세요.
