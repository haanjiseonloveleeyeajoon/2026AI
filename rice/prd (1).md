# PRD: 부산축제모아 (Busan Festival Archive)

> **부산광역시 16개 구·군의 문화 축제 및 행사 정보를 한눈에 조회하고 AI 기반의 맞춤형 여행 코스를 추천받는 웹 플랫폼**

---

## 1. 프로젝트 개요 (Project Overview)

- **제품명**: 부산축제모아 (Busan Festival Archive)
- **목적**: 공공데이터포털(data.go.kr)의 실시간 부산 축제 API 데이터를 기반으로, 부산을 방문하는 관광객과 시민들에게 직관적인 축제 탐색, 시각적 지도를 통한 위치 파악, AI 맞춤형 코스 가이드 및 개인화(찜하기, 캘린더 저장) 기능을 제공합니다.
- **배포 환경**: Vercel Serverless Functions & Express (Cloud Run / Node.js 지원)
- **디자인 컨셉**: **Bold Typography Theme** (고대비 에디토리얼 타이포그래피, 매거진 형태의 강렬한 레이아웃 및 세리프/산세리프 조화)

---

## 2. 타겟 사용자 (Target Audience)

1. **부산 여행객 & 관광객**: 시즌별/지역별로 가볼 만한 축제를 탐색하고 여행 동선을 계획하려는 방문객
2. **부산 시민**: 거주하는 구·군의 문화 행사 및 주말 나들이 정보를 빠르게 확인하려는 지역 주민
3. **축제/문화 이벤트 기획자 & 취재진**: 부산 전역에서 개최되는 문화 행사 및 축제 정보를 일괄 조회하려는 사용자

---

## 3. 핵심 기능 명세 (Key Features & Specifications)

### 3.1. 실시간 공공데이터 연동 및 폴백 시스템
- **데이터 출처**: 공공데이터포털 부산광역시 축제 서비스 API (`apis.data.go.kr/6260000/FestivalService/getFestivalKr`)
- **API 프록시 엔드포인트**: `/api/festivals`
- **캐싱 및 폴백**:
  - 10분 주기 메모리 캐싱으로 API 호출 최적화
  - 공공데이터포털 트래픽 초과나 점검 시 검증된 대표 축제 정제 데이터(Fallback) 자동 전환

### 3.2. 탐색 및 필터링 (Search & Filtering)
- **지역별 필터 (District Filter)**: 부산 16개 구·군(해운대구, 수영구, 중구, 영도구, 동래구 등)별 독립 필터링 및 카운트 표시
- **시기별 필터 (Date/Month Filter)**: 1월~12월 월별 개최 축제 분류 및 전체보기
- **키워드 실시간 검색**: 축제명, 장소, 구·군 명칭 실시간 쿼리 매칭
- **정렬 기능**: 날짜순(최신순), 축제명순 정렬
- **찜한 축제 전용 뷰 (Bookmarks)**: 브라우저 `localStorage` 기반 개인화 찜 목록 관리

### 3.3. 멀티 뷰 모드 (Multi-View System)
- **그리드 뷰 (Grid View)**: 카드형 에디토리얼 레이아웃으로 축제 대표 이미지, 뱃지, 날짜, 장소 제공
- **리스트 뷰 (List View)**: 가로형 리스트 패턴으로 긴 설명과 정보를 빠르게 스캔 가능
- **대화형 지도 뷰 (Map View)**: Leaflet 기반 커스텀 마커 지도, 클릭 시 인포 팝업 및 상세 보기 연동

### 3.4. 축제 상세 모달 (Festival Detail Modal)
- **시각적 헤로 영역**: 고화질 축제 이미지 및 구·군 식별 뱃지
- **상세 정보 패널**:
  - 축제 기간, 운영 시간, 이용 요금, 문의 전화번호 (`tel:` 바로 연결)
  - 상세 주소 표기 및 원클릭 주소 복사 기능
  - 축제 개요, 주요 행사 & 프로그램 상세 설명
- **인터랙티브 기능**:
  - **캘린더 저장 기능 (`.ics` 파일 생성)**: 구글 캘린더, 애플 캘린더, 아웃룩 연동
  - **공유하기**: 현재 축제 웹 페이지 URL 클립보드 복사
  - **공식 홈페이지 바로가기**: 외부 링크 이동

### 3.5. Gemini AI 맞춤형 여행 가이드 (AI Tour Guide)
- **엔드포인트**: `/api/ai-recommendation` (Serverless Function)
- **AI 모델**: Google GenAI (`gemini-2.5-flash`)
- **기능**:
  - 사용자가 선택한 특정 축제 또는 관심 구·군/월별 조건을 바탕으로 맞춤형 관광 동선, 교통 팁, 주변 추천 맛집 및 명소 추천
  - 자주 묻는 질문 프리스셋 버튼 제공 (예: 해운대 당일치기 코스, 데이트 코스 추천 등)

---

## 4. 기술 스택 (Tech Stack)

### Frontend
- **Framework**: React 18, Vite
- **Language**: TypeScript
- **Styling**: Tailwind CSS (v4), Custom Typography (`Playfair Display`, `Noto Serif KR`)
- **Icons**: Lucide React
- **Map Library**: Leaflet, React-Leaflet

### Backend & API
- **Runtime**: Node.js, Express
- **Serverless**: Vercel Node Functions (`@vercel/node`)
- **AI SDK**: `@google/genai` (Gemini API)
- **Data Source**: 공공데이터포털 REST API

---

## 5. 사용자 경험(UX) 및 디자인 가이드라인

- **컬러 팔레트**:
  - Canvas: `#FDFCFB` (Off-white), Card Container: `#F5F2EF`
  - Text & Line: `#1A1A1A` (Rich Dark Neutral)
  - Accent: `#EF4444` (Live Red Badge), Border: `#E5E1DA`
- **타이포그래피**:
  - Display Font: `Playfair Display`, `Noto Serif KR`
  - Base Font: Sans-serif System
  - 수치/영문 헤더: Black, Italic, Uppercase, Tracking-tighter
- **컴포넌트 룰**: Sharp rectangular borders (`border-[#E5E1DA]`, `border-black`), rounded-none 스타일 지향

---

## 6. 배포 및 이관 안내 (Deployment Guide)

### Vercel 배포 방법
1. **Repository 연결**: GitHub에 코드를 업로드한 후 Vercel에 프로젝트 연결
2. **환경 변수 설정 (Environment Variables)**:
   - `GEMINI_API_KEY`: Google Gemini API Key
   - `FESTIVAL_SERVICE_KEY`: 공공데이터포털 인증키 (미설정 시 기본 공공키 작동)
3. **빌드 설정**:
   - Framework Preset: `Vite`
   - Build Command: `npm run build`
   - Output Directory: `dist`
