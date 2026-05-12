# 나만의 시작 홈페이지 (My Dashboard)

> 카페24 AI SPACE에 배포된 개인 맞춤형 시작 페이지입니다.

**URL:** https://k1000c-dashboard.mycafe24.ai

---

## 주요 기능

- **실시간 시계** — 초 단위 업데이트, 날짜 및 요일 표시
- **날씨** — 브라우저 위치 자동 감지 후 open-meteo API로 현재 날씨·3일 예보 표시
- **주요 뉴스** — Google News RSS를 CORS 프록시를 통해 파싱, 종합·기술·경제·세계 탭 제공
- **빠른 링크** — Google, Gmail, GitHub, YouTube, Claude AI, Notion

---

## 기술 스택

| 항목 | 내용 |
|------|------|
| 런타임 | Static HTML (순수 HTML/CSS/JS) |
| 날씨 API | [open-meteo](https://open-meteo.com/) (무료, 인증 불필요) |
| 뉴스 | Google News RSS + CORS 프록시 |
| 배포 | 카페24 AI SPACE |

---

## 수정 이력

### v1.0.0 — 최초 배포 (2026-05-12)
- 실시간 시계, 날씨, 뉴스, 빠른 링크로 구성된 대시보드 최초 생성
- 날씨: open-meteo API 사용, 위치 자동 감지
- 뉴스: Anthropic Claude API + 웹 검색 도구로 뉴스 생성
- 디자인: 다크 테마 / 전문적·비즈니스 스타일

### v1.1.0 — 날씨 API 파라미터 수정 (2026-05-12)
- `weathercode` → `weather_code` 로 파라미터명 수정 (open-meteo API 변경 대응)
- 날씨 에러 메시지에 오류 원인 텍스트 및 "다시 시도" 버튼 추가
- 역지오코딩: Nominatim(CORS 오류) → ip-api.com 으로 교체

### v1.2.0 — 뉴스 방식 전면 교체 (2026-05-12)
- Anthropic API 직접 호출 방식 제거 (서버 환경 CORS 오류)
- Google News RSS + `allorigins.win` CORS 프록시 방식으로 전환
- 뉴스 클릭 시 원본 기사 링크 이동 기능 추가
- 날씨: 복잡한 역지오코딩 제거, 브라우저 Geolocation API만 사용하도록 단순화
- 위치 권한 거부 시 서울(37.5665, 126.9780) 기본값으로 자동 대체

### v1.3.0 — 뉴스 프록시 다중 폴백 적용 (2026-05-12)
- `allorigins.win` 단일 프록시에서 408 타임아웃 오류 발생 문제 해결
- 프록시 우선순위 목록 도입 (순서대로 자동 시도):
  1. `corsproxy.io` (1순위, 가장 안정적)
  2. `allorigins.win` (2순위, 폴백)
  3. `codetabs.com` (3순위, 최후 폴백)
- 각 프록시마다 8초 타임아웃 설정 (`AbortController` 사용)
- 타임아웃/오류 발생 시 자동으로 다음 프록시로 전환

---

## 파일 구조

```
/
├── index.html   # 전체 대시보드 (HTML + CSS + JS 단일 파일)
└── README.md    # 프로젝트 설명 및 수정 이력
```