# 🛠️ [개발 계획] AI자만추 MVP 개발 실행 계획서

> **목표:** 3개월 내 **실사용 가능한 베타 버전** 출시  
> **전략:** Lean & Agile - 핵심 기능 먼저, 빠르게 검증, 지속적 개선

---

## 📅 개발 일정 (12주 Sprint Plan)

### Week 1-2: 기술 스택 셋업 & 아키텍처 설계
**목표:** 개발 환경 구축 및 핵심 아키텍처 확정

**세부 작업:**
- [ ] Next.js 프로젝트 초기화 (TypeScript)
- [ ] Supabase 프로젝트 생성 & DB 스키마 설계
- [ ] AI Provider 추상화 레이어 구현 (Model Agnostic)
- [ ] 3-Layer Memory 시스템 설계 문서화
- [ ] Git 저장소 & CI/CD 파이프라인 구축 (Vercel)

**Output:** 기술 스택 문서 & 초기 코드베이스

---

### Week 3-4: 페르소나 시스템 & 채팅 UI
**목표:** 사용자가 AI와 대화할 수 있는 기본 인터페이스 완성

**세부 작업:**
- [ ] 페르소나 선택 화면 (카드 스와이프 UI)
- [ ] 채팅 화면 (카카오톡 스타일)
- [ ] AI 응답 스트리밍 (실시간 타이핑 효과)
- [ ] 페르소나별 System Prompt 작성 (루나, 제시카 등)
- [ ] TTS 연동 (선택 시 음성 재생)

**Output:** 동작하는 1:1 채팅 프로토타입

---

### Week 5-6: 게임화 & 데이터 수집
**목표:** 밸런스 게임, 심리 테스트를 대화 중 자연스럽게 삽입

**세부 작업:**
- [ ] 게임 컨텐츠 DB 설계 (질문, 선택지, 태그 매핑)
- [ ] 게임 트리거 로직 (AI가 맥락 파악 후 제안)
- [ ] 선택지 버튼 UI 구현
- [ ] 유저 답변 → 태그 저장 → 프로필 업데이트 자동화
- [ ] 대화 종료 후 "내 연애 리포트" 생성 (요약 + 태그 시각화)

**Output:** 데이터 수집 가능한 상호작용 시스템

---

### Week 7-8: 메모리 시스템 & RAG 구현
**목표:** 긴 대화에서도 맥락을 잃지 않는 AI 구현

**세부 작업:**
- [ ] Short-term Memory: 최근 10턴 대화 컨텍스트 유지
- [ ] User Profile: 구조화된 JSON 프로필 (나이, 성향, 태그 등)
- [ ] Long-term Memory: Vector DB (Pinecone/Supabase pgvector) 연동
- [ ] RAG 파이프라인: 질문 → 임베딩 → 관련 기억 검색 → AI 응답
- [ ] 주기적 요약 (10턴마다 DeepSeek로 요약 → DB 저장)

**Output:** 맥락 기억하는 AI 상담 시스템

---

### Week 9-10: 인증 & 보안 & 결제 기초
**목표:** 실제 서비스 가능한 수준의 시스템 안정화

**세부 작업:**
- [ ] Supabase Auth 연동 (소셜 로그인: 카카오/구글)
- [ ] 유저 세션 관리 & 토큰 갱신
- [ ] Rate Limiting (API 남용 방지)
- [ ] 결제 모듈 준비 (Stripe/토스페이먼츠 연동, 테스트 모드)
- [ ] 개인정보 처리방침 & 이용약관 페이지

**Output:** 보안 & 인증 완료된 베타 버전

---

### Week 11-12: 베타 테스트 & 피드백 반영
**목표:** 실사용자 20~50명 베타 테스트 & 버그 수정

**세부 작업:**
- [ ] 베타 테스터 모집 (지인, 커뮤니티)
- [ ] 피드백 수집 채널 (Discord, Google Forms)
- [ ] 버그 픽스 & UX 개선 (채팅 속도, UI 버그 등)
- [ ] 모니터링 시스템 구축 (Sentry, Google Analytics)
- [ ] 공개 출시 준비 (앱스토어/플레이스토어 등록 시작)

**Output:** 안정화된 Public Beta 버전

---

## 🎯 기능 우선순위 (Priority Matrix)

| 우선순위 | 기능 | 이유 |
| :--- | :--- | :--- |
| **P0 (필수)** | 1:1 채팅 (AI 상담) | 핵심 가치 검증 |
| **P0** | 페르소나 선택 | 차별화 포인트 |
| **P0** | 기본 게임 (밸런스 게임 3개) | 데이터 수집 필수 |
| **P1 (중요)** | 메모리 시스템 (RAG) | 장기 대화 품질 향상 |
| **P1** | 소셜 로그인 | 진입 장벽 최소화 |
| **P1** | 연애 리포트 생성 | 리텐션 확보 |
| **P2 (나중)** | TTS 음성 | 몰입감 향상 (추후 적용) |
| **P2** | 결제 시스템 | MVP 단계에선 테스트만 |
| **P2** | 매칭 기능 | Phase 2에서 본격 개발 |

---

## 🛠️ 기술 스택 상세 (Tech Stack Details)

### Frontend
- **Framework:** Next.js 14 (App Router)
- **언어:** TypeScript
- **스타일:** Tailwind CSS + shadcn/ui
- **상태 관리:** Zustand
- **HTTP 클라이언트:** Axios
- **호스팅:** Vercel (자동 배포)

### Backend & AI
- **Database:** Supabase (PostgreSQL + Realtime + Auth)
- **Vector DB:** Supabase pgvector 또는 Pinecone
- **AI Provider:**
  - **Tier 1 (Premium):** OpenAI GPT-4o, Anthropic Claude 3.5 Sonnet
  - **Tier 2 (Mass Market):** Qwen3-235B-A22B (OpenRouter), DeepSeek V3
  - **Vision:** Gemini 3 Pro, Qwen3-VL 72B
  - **Trend Search:** Grok 4 (SuperGrok API)
- **LLM SDK:** LangChain.js 또는 Vercel AI SDK
- **메모리:** Redis (Short-term cache) + Vector DB (Long-term)

### Infra & DevOps
- **호스팅:** Vercel (프론트) + Supabase (백엔드 + DB)
- **CI/CD:** GitHub Actions
- **모니터링:** Sentry (에러), Vercel Analytics (트래픽)
- **버전 관리:** Git + GitHub

---

## 👥 개발 인력 & 예상 공수

### 시나리오 1: 1인 풀스택 개발자
- **개발 기간:** 12주 (3개월)
- **주당 작업 시간:** 40시간 (풀타임)
- **총 공수:** ~480시간
- **장점:** 빠른 의사결정, 낮은 초기 비용
- **단점:** 병목 위험, UI/UX 퀄리티 제한

### 시나리오 2: 소규모 팀 (3명)
- **구성:** 프론트엔드 1명 + 백엔드/AI 1명 + 디자이너 1명
- **개발 기간:** 8~10주
- **주당 작업 시간:** 각 40시간
- **총 공수:** ~960~1,200시간
- **장점:** 높은 UI/UX 퀄리티, 병렬 개발 가능
- **단점:** 초기 인건비 부담 (~2,000만원)

**추천:** 초기엔 **1인 개발 + 외주 디자이너** 조합 (비용 절감 + 속도)

---

## ⚠️ 기술적 리스크 & 대응 방안

| 리스크 | 영향도 | 대응 방안 |
| :--- | :--- | :--- |
| **LLM API 비용 폭발** | 높음 | • Tier 2 모델 적극 활용 (Qwen, DeepSeek)<br>• Rate Limiting & 캐싱<br>• 무료 사용자는 일일 대화 횟수 제한 |
| **AI 응답 품질 저하** | 중간 | • System Prompt 세밀 튜닝<br>• Few-shot 예시 제공<br>• 사용자 피드백 수집 (👍/👎 버튼) |
| **대화 맥락 손실** | 중간 | • 3-Layer Memory 철저 구현<br>• RAG 파이프라인 테스트<br>• 주기적 요약 자동화 |
| **보안 취약점** | 높음 | • Supabase RLS (Row Level Security) 설정<br>• API 토큰 환경변수 관리<br>• HTTPS 필수 |
| **확장성 문제** | 낮음 | • Vercel 자동 스케일링<br>• Supabase 용량 모니터링<br>• DB 인덱싱 최적화 |

---

## 💰 예상 개발 비용 (3개월 기준)

| 항목 | 비용 (월) | 3개월 합계 |
| :--- | :--- | :--- |
| **인건비 (1인 개발자)** | ₩5,000,000 | ₩15,000,000 |
| **디자이너 (외주)** | ₩1,000,000 | ₩3,000,000 |
| **Supabase (Pro)** | $25 (~₩35,000) | ₩105,000 |
| **Vercel (Pro)** | $20 (~₩28,000) | ₩84,000 |
| **AI API (테스트)** | $100 (~₩140,000) | ₩420,000 |
| **도메인/기타** | ₩50,000 | ₩150,000 |
| **합계** | - | **₩18,759,000** |

*※ 1인 개발자 기준, 인건비 제외 시 실비용은 약 **380만원***

---

## 🎯 성공 지표 (MVP Success Metrics)

베타 출시 후 1개월 내 달성 목표:

- **MAU (월간 활성 사용자):** 100명 이상
- **리텐션 (D7):** 30% 이상
- **평균 대화 턴:** 10턴 이상 (깊이 있는 대화)
- **페르소나 선호도:** 루나 vs 제시카 등 데이터 수집
- **유저 만족도:** NPS 40+ (설문 조사)

---

## 📌 Next Steps (다음 단계)

1. **기술 검증 (Spike):** AI Provider 성능 테스트 (1주)
2. **프로토타입 개발:** Week 1-4 집중 개발
3. **내부 테스트:** Week 5-8 사내 QA
4. **베타 출시:** Week 9-12 실사용자 피드백
5. **Phase 2 기획:** 매칭 기능 설계 착수

---
*Created: 2026-02-04*
*Linked to: [Project_Secret_Match](Project_Secret_Match.md) | [Secret_Match_MVP_Spec](Secret_Match_MVP_Spec.md)*



---
[🏠 메인으로 돌아가기](README.md)