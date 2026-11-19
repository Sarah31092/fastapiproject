# AI 와인 소믈리에

FastAPI와 Jinja2로 만든 와인 검색 및 AI 소믈리에 웹 애플리케이션입니다. 와인 데이터의 벡터 검색, 가격·재고 필터, 대화 기록을 유지하는 챗봇, 요청 시 YouTube 영상 추천을 제공합니다.

## 주요 기능

- 와인 기본 정보와 상세 설명을 벡터 유사도로 검색하고 결과 점수를 표시합니다.
- 검색 시 최소·최대 가격과 재고 여부를 필터링할 수 있습니다.
- 챗봇은 와인 정보를 검색해 답하고, 브라우저별 대화 기록을 저장합니다.
- 챗봇은 사용자가 요청한 경우 YouTube 와인 영상을 검색합니다.

## 실행 방법

Python 가상환경을 활성화한 뒤 프로젝트 루트에서 의존성을 설치합니다.

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
```

프로젝트 루트에 `.env` 파일을 만들고 OpenAI API 키를 설정합니다.

```dotenv
OPENAI_API_KEY=your-openai-api-key
```

최초 실행 전, 또는 와인 데이터가 변경된 경우 벡터 DB를 생성합니다. 이어서 FastAPI 서버를 실행합니다.

```powershell
cd backend
python vectordb/create_vector_db.py
uvicorn main:app --reload
```

브라우저에서 <http://localhost:8000>에 접속합니다. 대화와 벡터 DB는 `backend/storage/`에 저장됩니다. 벡터 DB를 다시 생성한 경우 서버도 재시작해야 합니다.

## 화면 및 API

| 경로 | 설명 |
|---|---|
| `/` | 홈 |
| `/search` | 와인 검색 화면 |
| `/chat` | 와인 챗봇 |
| `GET /api/search` | 벡터 검색 API |
| `POST /api/chat` | 챗봇 질문 (`{"message": "불고기에 어울리는 와인을 추천해줘"}`) |
| `GET /api/chat/history` | 현재 브라우저 대화 기록 조회 |
| `DELETE /api/chat/history` | 현재 브라우저 대화 기록 삭제 |
| `/docs` | API 문서 |

검색 API 예시:

```text
/api/search?query=불고기&max_price=20&in_stock_only=true
```

가격 단위는 만원입니다. 검색 옵션과 요청 형식은 `/docs`에서 확인할 수 있습니다.

## 프로젝트 구조

```text
backend/
├── chat/       챗봇 API, 에이전트, 검색 도구
├── common/     설정, 템플릿 및 마크다운 처리
├── data/       와인 기본·상세 데이터
├── home/       홈 화면
├── search/     검색 화면 및 API
├── static/     CSS와 챗봇 JavaScript
├── templates/  Jinja2 화면 템플릿
└── vectordb/   벡터 DB 생성 및 검색
```

검색어 임베딩, 벡터 DB 생성, 챗봇 응답에는 OpenAI API가 사용됩니다.
