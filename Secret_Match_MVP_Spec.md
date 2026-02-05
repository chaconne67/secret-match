# 📱 [기획] AI자만추 MVP 프로토타입 명세서

> **목표:** 핵심 가치인 "페르소나 상담"과 "게임화된 데이터 수집"을 검증하기 위한 최소 기능 제품.
> **전략:** 빠르고 가볍게, 하지만 핵심 재미(Fun)는 확실하게.

---

## 1. 핵심 기능 (Core Features)

### 🎭 페르소나 선택 (The Choice)
*   앱 실행 시 5명의 매니저(제시카, 루나 등) 프로필이 카드 형태로 등장.
*   Tinder처럼 좌우로 스와이프하며 선택.
*   선택 시 해당 매니저의 "첫 마디"가 음성(TTS)이나 텍스트로 재생.

### 💬 1:1 상담 채팅 (The Chat)
*   **몰입감:** 카카오톡과 유사한 UI/UX.
*   **인터랙션:** 단순 텍스트뿐만 아니라, **선택지(Button)**를 제공하여 대화 유도.
    *   *(예: "오빠, 밥 먹었어?" → [어 먹었어] / [아직, 너는?])*

### 🎮 미니 게임 트리거 (The Game)
*   대화 도중 AI가 맥락에 맞는 **밸런스 게임**이나 **심리 테스트**를 툭 던짐.
*   유저의 답변은 태그(Tag) 형태로 DB에 저장되어 성향 분석에 활용.

---

## 2. 기술 스택 (Tech Stack)

### 🎨 Frontend
*   **Framework:** Next.js (React) - 빠른 개발과 배포.
*   **Styling:** Tailwind CSS - 모바일 친화적 UI 디자인.
*   **State:** Zustand - 가벼운 상태 관리.

### 🧠 AI & Backend
*   **Architecture:** **Model Agnostic (모델 불가지론)** 설계.
    *   **AI Provider Layer:** OpenAI, Anthropic, Google, DeepSeek 등 다양한 모델을 설정값 변경만으로 교체 가능하도록 추상화.
    *   **LLM Router:** 상담 난이도나 비용에 따라 최적의 모델 자동 라우팅.
*   **Memory System:** **3-Layer Hybrid Memory** [👉 메모리 설계 상세 보기](Secret_Match_Memory_Architecture.md)
    *   단기(Short-term) + 프로필(User Profile) + 장기(Vector DB) 결합 구조.
*   **Database:** Supabase (PostgreSQL) - 유저 데이터, 대화 로그, 성향 태그 저장.

---

## 3. 화면 흐름 (User Flow)

1.  **Splash:** 로고 노출 ("당신의 연애 세포를 깨워드립니다")
2.  **Onboarding:** 닉네임, 성별, 나이 입력 (최소화)
3.  **Persona Select:** 매니저 카드 스와이프 → 선택
4.  **Chat Room:**
    *   AI 선톡 ("안녕? 오늘 기분 어때?")
    *   대화 진행 & 중간중간 미니 게임
    *   상담 종료 후 "내 연애 리포트" 요약 보여주기

---
*Created: 2026-02-04*
*Linked to: [Project_Secret_Match](Project_Secret_Match.md)*



---
[🏠 메인으로 돌아가기](README.md)