# Keepit

> 저장은 했는데, 왜 못 찾을까?

Keepit은 URL을 저장하면 AI가 자동으로 분류·요약하고, 자연어로 다시 찾아 주는 개인 링크 아카이빙 서비스입니다.

---

## Key Features

- 🔗 **URL 저장 & 자동 분류** — 메타데이터 추출 → 썸네일 Vision 분석 → 카테고리·태그·요약 생성 → 임베딩 저장
- 🔍 **자연어 검색 (RAG)** — 벡터 검색으로 후보를 좁히고, 리랭킹한 뒤, 좁혀진 자료만 근거로 답변 생성
- 📁 **폴더 관리** — 자연어로 이동·합치기·삭제 (LLM 의도 분류 → 핸들러 라우팅, 실행 전 사용자 확인)
- 📊 **주간·월간 레포트** — 저장 취향 요약
- ⏰ **마감 리마인드** — 저장 시 마감일 감지, 채팅·캘린더에서 조회·수정
- 💬 **텔레그램 연동** — 앱 밖에서도 저장·검색

---

## RAG 파이프라인

`POST /chat` → `app/services/chat.py`의 `_handle_search`

| 단계 | 하는 일 | 코드 |
|------|---------|------|
| 1. Query rewrite | 이전 대화 맥락을 반영해 검색 쿼리 재작성 | `chat._build_context_query` |
| 2. Query expansion | 줄임말·동의어·한/영 표기를 풀어 임베딩 친화적으로 확장 | `query_expander.expand_query` |
| 3. Embedding | `text-embedding-3-small` (1536차원) | `embedding.generate_embedding` |
| 4. Retrieval | pgvector 코사인 유사도 검색 (Supabase RPC `match_user_contents`), 유사도 0.3 미만 제외 | `database.search_contents` |
| 5. Rerank | LLM이 후보를 질문 기준으로 0~10점 채점 → 재정렬, 무관한 후보 제거 | `rag.rerank` |
| 6. Generation | 상위 5건의 요약·분류·태그만 근거로 답변 생성, 근거는 `[1]`, `[2]`로 표시 | `rag.generate_answer` |

전체 저장 목록을 프롬프트에 넣지 않습니다. LLM이 보는 것은 4~5단계를 통과한 최대 5건뿐입니다.
5·6단계의 LLM 호출이 실패하면 각각 벡터 유사도 순서, 고정 안내 문구로 대체되어 검색 자체는 계속 동작합니다.

저장 시 임베딩에는 제목 + 태그 + 요약 + 썸네일 Vision 설명이 함께 들어가므로 "핑크색 부츠 신은 영상" 같은 시각적 단서로도 검색됩니다.

---

## Tech Stack

| 영역 | 기술 |
|------|------|
| Frontend | Jinja2 템플릿 + Vanilla JS |
| Backend | FastAPI |
| Database | Supabase (PostgreSQL + pgvector) |
| AI | GPT-4o (Vision) · GPT-4o-mini · OpenAI Embeddings |
| Auth | Kakao OAuth 2.0 |
| Messaging | Telegram Bot API |

---

## 실행

```bash
pip install -r requirements.txt
cp env.example .env   # 키 입력
python main.py        # http://localhost:8000
```
