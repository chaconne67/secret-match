# 🧠 [기술 전략] Multi-Model Orchestration Strategy

> **"Right Tool for the Right Job"**
> 단일 모델 의존도를 낮추고, 각 LLM의 강점을 극대화하여 성능과 비용 효율을 동시에 잡는 하이브리드 아키텍처 전략입니다.

---

## 1. Model Role Matrix (모델 할당표)

| 역할 (Role) | 최적 모델 (Best Fit) | 선정 이유 (Rationale) |
| :--- | :--- | :--- |
| **💬 Persona Chat**<br>(메인 상담/대화) | **Claude 3.5 Sonnet** | • **한국어 뉘앙스 최강:** "츤데레", "위로", "공감" 등 감성적 텍스트 생성 능력이 탁월함.<br>• 자연스러운 대화 흐름 유지에 유리. |
| **🧠 Matching Logic**<br>(심층 분석/추론) | **OpenAI o1 (Preview)** | • **복합 추론 능력:** 유저의 말과 행동 사이의 모순을 찾아내고, 최적의 상대를 찾아내는 논리적 사고(Chain of Thought)에 압도적.<br>• 매칭 실패율 최소화. |
| **📸 Vision Analysis**<br>(프로필/사진 분석) | **GPT-4o** | • **멀티모달 밸런스:** 이미지 인식 정확도와 처리 속도가 가장 균형 잡힘.<br>• 프로필 사진의 분위기, 옷차림 등을 분석해 스타일 태그 생성. |
| **⚡ Background Task**<br>(요약/검색/분류) | **DeepSeek V3** | • **압도적 가성비:** 단순 요약, 대화 로그 정리, 키워드 추출 등 백그라운드 작업에 최적.<br>• 토큰 비용 절감의 핵심 키(Key). |

---

## 2. Orchestration Flow (작업 처리 흐름)

### Scenario: "이번 주말 데이트 코스 짜줘" (복합 요청)

1.  **Router (DeepSeek V3):** 사용자의 의도가 "정보 검색" + "추천"임을 파악하고 작업 분할.
2.  **Information Retrieval (DeepSeek V3):** 성수동 맛집, 날씨, 핫플 정보 검색 및 요약.
3.  **Reasoning (o1):** 유저의 성향(조용한 곳 선호)과 검색 결과를 대조하여 최적의 코스(A안, B안) 설계.
4.  **Response Generation (Claude 3.5 Sonnet):** 설계된 코스를 바탕으로 '루나'의 페르소나를 입혀 답변 작성.
    *   *"오빠, 성수동은 사람 많아서 싫다며? 그래서 내가 구석진 힙한 카페 찾아왔어. 칭찬해줘."*

---

## 3. Intelligent Memory System (지능형 메모리)

*   **Periodic Summarization:** 대화 10턴마다 **DeepSeek V3**가 요약 수행 → 벡터 DB 저장.
*   **Context Injection:** 질문 시 관련 기억만 **RAG**로 추출하여 **Claude 3.5 Sonnet**에게 전달.
*   **Effect:** 무한한 대화가 가능하면서도, 프롬프트 비용은 일정하게 유지.

---
*Created: 2026-02-04*
*Linked to: [Project_Secret_Match](Project_Secret_Match.md)*



---
[🏠 메인으로 돌아가기](README.md)