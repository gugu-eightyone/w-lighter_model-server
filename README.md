# w-lighter_model-server

> **w.LiGHTER**의 **MODEL 서버(FastAPI)** 레포입니다.   
> AI 번역 파이프라인과 창작 지원 기능(현지화 가이드·표지 이미지·캐릭터 설정집/관계도)의 모델 추론을 전담합니다.   
> 이 레포는 **팀 프로젝트([원본](https://github.com/IsekaiPD/w-lighter_model-server))를 포트폴리오용으로 재구성·확장해 개인적으로 관리하는 미러**입니다.   
> 프로젝트 전체 소개(기획 배경·화면·비즈니스 모델)는 팀 레포를 참고해 주세요 → [SKN24-FINAL-5Team](https://github.com/SKNETWORKS-FAMILY-AICAMP/SKN24-FINAL-5Team).   
> **제 담당(진세형)**: AI 모델 개발(3인) 중 **번역 파이프라인 설계·구현**과 문화 각주 RAG의 **임베딩 모델 평가·선정**을 맡았습니다. 아래 4·5절이 그 내용입니다.

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

웹소설 번역에서는 캐릭터 말투, 용어 통일, 문화 차이를 함께 챙겨야 합니다. 한 번의 LLM 호출로는 세 가지를 모두 다루기 어려워, **초벌 번역 1 · 검수 4 · 최종 수정 1**로 단계를 나눈 파이프라인을 **LangGraph(12노드)** 로 구현했습니다. 구조는 멀티에이전트 문학 번역 논문 TransAgents(*Beyond Human Translation*, 2024)의 번역–검토–판정 구조를 참고했습니다.

> 번역 요청 → **작품 용어집 조회**(설정 일관성) → 초벌 번역 → **검수 4종 병렬**(말투·자연스러움·문화·용어) → 검수 취합 → 최종 번역 → **한글 잔류 점검·수리**(최대 2회) → **한국 문화 미주**(한국 문화 RAG / Qdrant) → 결과 패키징 → 응답(최종 번역 + 리포트)

<img width="3480" height="2300" alt="번역_파이프라인_구조도" src="https://github.com/user-attachments/assets/d93f8cac-fdab-42cd-be38-1e0f0db8c78b" />

### 구조 설명

- **문화 표현 검색은 번역과 병렬**: 원문의 한국 문화 표현 검색은 번역과 동시에 실행하고, 각주는 최종 번역이 나온 뒤 작성합니다.
- **검수 4개를 병렬 실행**: 말투, 자연스러움, 문화 적합성, 용어 일관성을 각각 다른 에이전트가 검수하고, 최종 수정 단계가 의견을 종합해 최종본과 수정 근거를 냅니다. 확정 LLM 호출은 6회(초벌 1 + 검수 4 + 최종 1)이고, 검수 4회가 동시에 실행되어 대기 시간은 호출 1회 수준입니다.
- **문화 각주 RAG**: 원문 속 한국 고유 표현(수능, 제사 등)에 대한 설명을 Qdrant에서 검색해 독자용 각주로 붙입니다. 검색 결과는 번역문에는 넣지 않습니다.
- **용어집**: 작품·국가별 용어집에서 이번 회차 원문에 나오는 항목만 골라 번역에 반영하고, 새 고유명사는 자동으로 추출해 제안만 합니다. DB 반영은 사용자 확인 후에 합니다.
- **구조화 출력**: 모든 단계의 LLM 출력을 JSON schema(strict)로 받아 파싱 실패와 필드 누락을 막고, 프롬프트는 코드와 분리했습니다(`prompts/*.md`).

### 구조를 바꾼 과정

처음에는 초벌 번역과 최종 수정 2단계 구조였고, 번역 품질을 높이려고 한국어–외국어 관용구 대응 사전을 RAG로 검색해 번역 모델에 함께 넣었습니다. 그런데 팀 평가(LLM-as-judge)에서 관용구 정보가 오히려 번역문을 딱딱하게 만든다는 결과가 나왔습니다.

임베딩 모델을 추가 학습해 문맥에 맞는 관용구를 찾는 방법도 검토했지만, 남은 기간 안에 데이터 확보부터 학습·검증까지 마치기 어려웠습니다. 그래서 기간 안에 구현과 검증이 가능한 구조 변경을 선택했습니다.

- 번역 단계의 관용구 RAG를 제거했습니다([`include_rag_context=False`](model_server/domains/translation/agents/direct_translator.py#L81)).
- 검수를 4개 관점으로 나눠 각각 다른 에이전트가 맡게 했습니다.
- 기존의 재번역 반복과 차단 판정을 없애고, 최종 수정 단계가 검수 의견을 종합하도록 단순화했습니다.

이후 팀 재평가에서 번역이 더 자연스러워진 것을 확인했습니다. 번역 품질 평가와 번역 모델 변경(GPT-4.1-mini → GPT-5.4-mini)은 팀에서 진행했고, 저는 평가 결과를 바탕으로 구조를 재설계·구현했습니다.

핵심 코드: [`model_server/domains/translation/`](model_server/domains/translation/) · 그래프 정의: [`engine/graph_orchestrator.py`](model_server/domains/translation/engine/graph_orchestrator.py)

# 5. 임베딩 모델 선정 (정량 평가)

문화 각주 RAG에 쓸 임베딩 모델을 정하기 위해 3개 모델(**multilingual-E5** / **KURE-v1** / **F2LLM-v2-1.7B**)을 5개 데이터셋(한국 문화 RAG 문서 494개·질의 50개, 관용표현 4개국)에서 비교했습니다.

- **지표**: Hit@1/3/5/10, MRR, 정답·오답 점수 차이, threshold F1, 지연 시간(avg/p95). 실제 서비스는 상위 5개 후보만 쓰기 때문에 Top-5 기준으로 다시 분석했습니다.
- **결과**: 한국 문화 데이터셋에서 KURE의 Hit@1은 0.820(E5 0.740)이었고, 정답·오답 점수 차이는 E5의 약 4배(0.069 / 0.017)라 임계값으로 걸러내기 쉬웠습니다. F2LLM은 검색 품질이 가장 좋았지만 지연 시간이 약 6배라 실시간 서비스에는 맞지 않았습니다.

이 결과를 바탕으로 **KURE-v1**을 선택했습니다.

### 운영 환경에서 임계값 조정 (0.60 → 0.55)

Qdrant를 연결한 뒤 각주가 표시되지 않는 문제가 있었습니다. 서술형 원문과 문화 설명 문서 사이의 KURE 점수가 0.55~0.60에 몰려 있었고, 정답 문서(0.581)도 0.60 기준에 걸려 제외되고 있었습니다. 평가 보고서의 권장값인 0.55로 낮추고, 환경변수(`WLIGHTER_ANNOTATION_SCORE_THRESHOLD`)로 바꿀 수 있게 했습니다.

### 평가의 한계

- 관용표현 데이터셋은 질의마다 정답이 1개인데, 관용표현은 비슷한 뜻의 표현이 여러 개일 수 있어 KURE가 낮게 평가됐을 가능성이 있습니다.
- F2LLM은 다른 두 모델보다 파라미터가 2~3배 많아 같은 조건의 비교는 아니었습니다. 중국어·태국어 관용표현에서는 E5의 검색 순위가 더 좋았습니다.

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

- **Mock 모드**: `WLIGHTER_MOCK_MODE=true`로 설정하면 OpenAI 키, Qdrant, KURE 없이 번역 파이프라인 전 구간(router → service → engine)을 확인할 수 있습니다.
- 기본 확인 엔드포인트: `GET /health`, `POST /api/v1/translation/translate`
- Docker 배포 구성은 `docker-compose.yml`을 참고해 주세요.
