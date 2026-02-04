# 🧠 [기술 기획] AI Memory System Architecture

> **목표:** 사용자와의 장기적인 대화를 기억하고, 깊은 라포(Rapport)를 형성하기 위한 **'인간 뇌 모방'** 메모리 시스템 설계.
> **핵심:** 무조건 긴 컨텍스트를 쓰는 게 아니라, **"필요한 기억을 적시에 꺼내는 것"**이 기술의 핵심.

---

## 1. 하이브리드 메모리 구조 (The 3-Layer Memory)

### ⚡ Layer 1: Short-term Memory (단기 기억)
*   **저장 대상:** 최근 대화 10~20턴 (Sliding Window 방식).
*   **역할:** 바로 직전의 대화 맥락을 파악하고 자연스러운 티키타카 유지.
*   **구현:** LLM의 기본 Context Window에 원문 그대로 삽입.

### 👤 Layer 2: User Profile (핵심 기억 - Entity)
*   **저장 대상:** 변하지 않는 고정 팩트 (Fact).
    *   기본 정보: 이름, 나이, 직업, MBTI
    *   연애 정보: 이상형, 전 연인 특징, 연애 가치관, 기피 대상
*   **역할:** 페르소나가 유저를 '알고 있다'는 느낌(Personalization) 제공.
*   **구현:** 대화가 끝날 때마다 AI가 요약하여 **JSON 형태**로 구조화된 DB(RDB/NoSQL)에 업데이트.
    *   *예: `{"preference": {"food": "spicy", "date_style": "active"}}`*

### 📚 Layer 3: Long-term Memory (장기 기억 - Vector DB)
*   **저장 대상:** 과거의 모든 상담 로그, 감정선, 특정 에피소드.
*   **역할:** "저번 달에 그랬잖아" 같은 장기 서사 연결 및 과거 회상.
*   **구현:**
    1.  대화 로그를 의미 단위(Chunk)로 쪼개어 임베딩(Embedding).
    2.  **Vector DB (Pinecone, Weaviate 등)**에 저장.
    3.  **RAG (Retrieval):** 현재 대화와 유사도가 높은 과거 기억을 검색하여 프롬프트에 주입.

---

## 2. 메모리 처리 파이프라인 (Memory Pipeline)

1.  **Input:** 유저가 메시지를 보냄. (*"또 그 사람한테 연락 왔어..."*)
2.  **Retrieval (검색):** Vector DB에서 '그 사람', '연락', '과거 이별' 관련 키워드로 과거 기억 검색.
3.  **Prompt Assembly:**
    *   `System Prompt` (페르소나 설정)
    *   `+ User Profile` (유저 성향 JSON)
    *   `+ Retrieved Memory` (검색된 과거 기억: *"3주 전, 그 사람 때문에 힘들어서 차단한다고 했었음"*)
    *   `+ Short-term Context` (최근 대화)
4.  **Generation:** AI가 답변 생성.
    *   *AI: "저번에 차단한다고 큰소리치더니, 결국 또 흔들리는 거야? 내가 말했지, 그 사람은 아니라고."*
5.  **Update:** 새로운 대화 내용을 각 메모리 레이어에 저장 및 업데이트.

---
*Created: 2026-02-04*
*Linked to: [Secret_Match_MVP_Spec](Secret_Match_MVP_Spec.md)*


---
[🏠 메인으로 돌아가기](README.md)