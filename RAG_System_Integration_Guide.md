# 🧠 [기술 문서] 초경량 RAG 메모리 시스템 설계도 및 설치 가이드

> **"5분 만에 당신의 AI에 '장기 기억'을 심으세요."**
> SQLite와 OpenAI 임베딩을 활용한 **Serverless급 초경량 RAG(Retrieval-Augmented Generation)** 시스템입니다. 복잡한 Vector DB 설치 없이 로컬 파일 하나로 완벽하게 작동합니다.

---

## 1. System Architecture (설계도)

```mermaid
graph TD
    User[사용자/AI 에이전트] -->|1. 질문 (Query)| API[Flask API Server]
    API -->|2. 임베딩 변환| OpenAI[OpenAI Embedding API]
    API -->|3. 벡터 검색 (Cosine Sim)| DB[(SQLite Database)]
    DB -->|4. 유사 대화 반환| API
    API -->|5. 기억 주입 (Context)| LLM[LLM (GPT/Claude)]
    LLM -->|6. 답변 생성| User
```

### 💎 핵심 특징
*   **Zero Infrastructure**: 별도의 Vector DB(Pinecone, Milvus 등) 설치 불필요. `memory.db` 파일 하나로 끝.
*   **Hybrid Storage**: 대화 내용(Text)과 임베딩 벡터(JSON)를 하나의 SQLite 테이블에 저장.
*   **Cost Effective**: 검색 비용 $0 (로컬 연산). 임베딩 생성 비용만 발생 ($0.02/1M tokens).
*   **Standard API**: REST API 제공으로 어떤 언어/프레임워크와도 연동 가능.

---

## 2. Quick Start (설치 가이드)

### 전제 조건
*   Python 3.9 이상
*   OpenAI API Key

### 🚀 3단계 설치법

**Step 1: 프로젝트 클론 및 패키지 설치**
```bash
# 프로젝트 폴더 생성
mkdir my-rag-memory
cd my-rag-memory

# 가상환경 생성 (선택사항)
python -m venv venv
source venv/bin/activate  # Windows: venv\Scripts\activate

# 필수 패키지 설치
pip install openai flask flask-cors python-dotenv numpy scikit-learn
```

**Step 2: 환경 설정**
`.env` 파일을 생성하고 키를 입력하세요.
```env
OPENAI_API_KEY=sk-your-api-key-here
DB_PATH=data/memory.db
```

**Step 3: DB 초기화 및 서버 실행**
(아래 코드를 `server.py`로 저장 후 실행)

```python
# server.py (간소화된 버전)
from flask import Flask, request, jsonify
import sqlite3, json, os, numpy as np
from openai import OpenAI
from sklearn.metrics.pairwise import cosine_similarity
from dotenv import load_dotenv

load_dotenv()
app = Flask(__name__)
client = OpenAI()
DB_PATH = os.getenv('DB_PATH', 'memory.db')

# DB 초기화
def init_db():
    conn = sqlite3.connect(DB_PATH)
    conn.execute('''CREATE TABLE IF NOT EXISTS memories 
                    (id INTEGER PRIMARY KEY, content TEXT, embedding TEXT)''')
    conn.close()

init_db()

# 임베딩 생성
def get_embedding(text):
    return client.embeddings.create(input=text, model="text-embedding-3-small").data[0].embedding

@app.route('/add', methods=['POST'])
def add_memory():
    data = request.json
    emb = json.dumps(get_embedding(data['content']))
    conn = sqlite3.connect(DB_PATH)
    conn.execute("INSERT INTO memories (content, embedding) VALUES (?, ?)", (data['content'], emb))
    conn.commit()
    conn.close()
    return jsonify({"status": "saved"})

@app.route('/search', methods=['POST'])
def search_memory():
    query = request.json['query']
    query_vec = get_embedding(query)
    
    conn = sqlite3.connect(DB_PATH)
    rows = conn.execute("SELECT content, embedding FROM memories").fetchall()
    conn.close()
    
    results = []
    for content, emb_json in rows:
        score = cosine_similarity([query_vec], [json.loads(emb_json)])[0][0]
        results.append({"content": content, "score": float(score)})
    
    # Top 5 반환
    return jsonify(sorted(results, key=lambda x: x['score'], reverse=True)[:5])

if __name__ == '__main__':
    app.run(port=5000)
```

```bash
python server.py
```
👉 이제 `http://localhost:5000`에서 기억 시스템이 동작합니다!

---

## 3. API Reference (사용법)

### 📥 기억 저장 (Add Memory)
대화 로그나 정보를 저장합니다.

**Request:** `POST /add`
```json
{
  "content": "Secret Match 프로젝트의 타겟은 25-39세 현실적 낭만주의자입니다."
}
```

### 🔍 기억 검색 (Search Memory)
질문과 관련된 과거 기억을 찾습니다.

**Request:** `POST /search`
```json
{
  "query": "Secret Match 타겟이 누구야?"
}
```

**Response:**
```json
[
  {
    "content": "Secret Match 프로젝트의 타겟은 25-39세 현실적 낭만주의자입니다.",
    "score": 0.892
  }
]
```

---

## 4. 활용 시나리오 (Integration Patterns)

### 🤖 AI 챗봇에 적용하기
사용자의 질문이 들어오면 먼저 검색 API를 호출하세요.

```python
# 1. 사용자 질문
user_query = "우리 타겟이 누구였지?"

# 2. 기억 검색 (RAG)
memories = requests.post("http://localhost:5000/search", json={"query": user_query}).json()
context = "\n".join([m['content'] for m in memories])

# 3. LLM 프롬프트 주입
prompt = f"""
참고 정보(Memory):
{context}

사용자 질문: {user_query}
답변:
"""

# 4. LLM 호출
response = openai.ChatCompletion.create(model="gpt-4", messages=[{"role": "user", "content": prompt}])
```

---

## 5. 확장성 (Scalability)

*   **데이터가 많아지면?**: SQLite는 수십만 건까지 문제없습니다. 백만 건 이상이면 `pgvector`(PostgreSQL)나 `Qdrant`로 마이그레이션하세요.
*   **보안**: 이 시스템은 로컬에서 작동하므로 데이터가 외부로 유출되지 않습니다. (OpenAI API로 전송되는 텍스트 제외)

---
*Created: 2026-02-05*
*Author: Judy (AI자만추 Team)*


---
[🏠 메인으로 돌아가기](README.md)