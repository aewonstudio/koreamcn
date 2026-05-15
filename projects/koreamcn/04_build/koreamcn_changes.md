# KoreaMCN 프로젝트 변경 이력

## 네비게이션 탭 이름 변경 (2026-05-15)
- Company → **회사 소개**
- Business → **사업 소개**
- 셀러 → **셀러 모집**
- 파트너십, 보도자료, 문의하기: 유지
- 회사 소개 하위: 개요, CEO 인사말, 연혁, 글로벌 네트워크
- 사업 소개 하위: 국제 물류 서비스, 크로스보더 서비스, 라이브커머스 · MCN
- 적용: 전체 12개 HTML (index, about, ceo, history, network, logistics, crossborder, live-commerce, partner, seller, news, contact)

## Sub-Hero 구조 변경 (2026-05-15)
- **라벨 제거**: sub-hero에서 영문 라벨 삭제 (partner, seller)
- **타이틀 통일**: 상위 메뉴 단위로 sub-hero 타이틀 고정
  - 회사 소개 그룹 (about/ceo/history/network): "회사 소개"
  - 사업 소개 그룹 (logistics/crossborder/live-commerce): "사업 소개" + "물류·유통·라이브커머스, 한중 비즈니스의 전 과정을 지원합니다"
  - partner: "파트너십", seller: "셀러 모집", news: "보도자료", contact: "문의하기"
- **이미지 교체**: picsum.photos 랜덤 → Gemini 생성 로컬 이미지 (6개)
  - subhero-company.jpg (about/ceo/history/network)
  - subhero-business.jpg (logistics/crossborder/live-commerce)
  - subhero-partnership.jpg (partner)
  - subhero-seller.jpg (seller)
  - subhero-news.jpg (news)
  - subhero-contact.jpg (contact)

## 사업 소개 서브페이지 위계 변경 (2026-05-15)
- sub-hero가 "사업 소개"로 통일되면서, 첫 콘텐츠 섹션이 페이지 식별 역할 수행
- 패턴: 영문 라벨(유지) + h2 페이지명(신규) + h3 서브타이틀(기존 타이틀 강등) + 본문
- logistics.html: 라벨 `LOGISTICS` / h2 "국제 물류 서비스" / 서브타이틀 삭제, 설명에 4PL 내용 통합
- crossborder.html: 라벨 `CROSSBORDER` / h2 "크로스보더 서비스" / 설명 "한국 시장 진출 전략 수립부터 인증·법인 설립·유통 채널 운영까지, 전 과정을 원스톱으로 컨설팅합니다."
- live-commerce.html: 라벨 `LIVE COMMERCE` / h2 "라이브커머스 · MCN" / 서브타이틀 삭제, 설명에 TikTok MCN 파트너 내용 통합

## 기타 수정 (2026-05-15)
- 로고: 텍스트 → `koreamcn-logo-white.png` (헤더 44px, 푸터 36px)
- index.html: 슬래시 링크 → .html 링크, MCN 중복 제거, 뉴스 섹션 "보도자료"로 변경
- news.html: 태그를 홈 기준으로 통일 (전략 협력, 산업 협력, 보도자료, 전략 협력, 행사)
- h1→h2 통일 (sub-hero 전체)
- partner.html: 첫 섹션 중앙 정렬, 라벨 `PARTNERSHIP`, 설명 추가
- seller.html: 첫 섹션 라벨 `SELLER`, 설명 추가
- 사업 소개 nav href: `#` → `logistics.html`
- about 바로가기: `about.html` → `about.html#company-overview`

## Accent 컬러 통일 + 로고 크기 조정 (2026-05-15)
- accent 컬러: `#C23B3B`(레드) + `#E8734A`(오렌지) → **`#E8603C`** 단일 컬러로 통일 (전체 14개 HTML)
- CJ대한통운·쿠팡 로고: 기본 220×90px → **340×130px** 확대 (index.html 파트너 마키)
- 로고 스타일: grayscale 유지 (호버 시 컬러 전환)

## 이전 변경 이력

### 네비게이션 메뉴명 변경 (2026-05-14)
- About → Company, Services → Business
- 드롭다운 서브메뉴 추가 (Company/Business 하위)

### 서브페이지 Sub-Hero 구조 (2026-05-14)
- 한솔로지스틱스 `sub-visual` 패턴 채택
- 하단 앵커 바: glassmorphism, container max-width 맞춤
- 앵커 바 항목 간 구분: `sub-anchor-nav__sep` (20px 세로선)

### 2depth 드롭다운 서브메뉴 (2026-05-14)
- `.site-nav ul` → `.site-nav > ul` (sub-menu 스타일 상속 방지)
- 드롭다운 스타일: `var(--color-bg-alt)`, 호버 accent 코랄 레드
