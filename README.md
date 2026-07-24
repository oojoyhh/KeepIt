# Keepit

> 저장은 했는데, 왜 못 찾을까?

Keepit은 URL을 저장하면 AI가 자동으로 분류·요약하고, 자연어로 검색할 수 있는 개인 링크 아카이빙 서비스입니다.

---

## About

카카오톡 나에게 보내기, 브라우저 북마크, 노션 붙여넣기.  
저장 방법은 많지만 결국 **"그 링크 어디 갔지?"** 를 반복하게 됩니다.

Keepit은 URL을 붙여넣는 순간 **스크래핑 → Vision 분석 → 자동 분류 → 임베딩**까지 자동으로 처리하고,  
`"딥러닝 강의 저번에 저장한 거 찾아줘"` 같은 자연어로 꺼내 쓸 수 있습니다.

---

## Key Features

- 🔗 **URL 저장 & 자동 분류** — 16개 카테고리, 태그, 한줄 요약 자동 생성
- 🔍 **자연어 검색** — RAG 기반 벡터 검색 + Reranker로 정확도 향상
- 📁 **폴더 관리** — 자연어로 이동·합치기·삭제 (Tool Calling)
- 📊 **주간·월간 레포트** — 내 저장 취향을 AI가 요약
- ⏰ **마감 리마인드** — 마감일 감지 후 텔레그램 알림
- 💬 **텔레그램 연동** — 앱 밖에서도 저장·검색 가능

---

## Tech Stack

| 영역 | 기술 |
|------|------|
| Frontend | Next.js |
| Backend | FastAPI |
| Database | Supabase (PostgreSQL + pgvector) |
| AI | GPT-4o · GPT-4o-mini · OpenAI Embeddings |
| Auth | Kakao OAuth 2.0 |
| Messaging | Telegram Bot API |

---
