# HOME PAGE — Enhanced Rebuild Prompt

## CRITICAL CONSTRAINTS (반드시 준수)

### 1. 섹션 순서 (절대 변경 금지)
Header (sticky nav) 다음, 아래 순서를 정확히 따라라:
1. hero
2. value-proposition
3. trust-badges
4. services
5. partner-logos
6. news-highlight
7. cta
8. footer
순서를 바꾸거나 섹션을 누락하면 안 된다.

### 2. Hero 좌측 정렬 (필수)
Hero 콘텐츠는 반드시 좌측 정렬이다. 중앙 정렬 금지.
```css
#hero .container {
    max-width: none;
    width: 100%;
    padding-left: clamp(40px, 8vw, 160px);
    padding-right: 40px;
}
.hero-content {
    max-width: 700px;
    /* margin: 0 auto 금지 — 좌측 정렬 유지 */
}
```

### 3. 인터랙션 (전부 구현 필수 — 생략 금지)
a) **스티키 헤더**: 초기 transparent → scrollY > 50에서 배경색 전환 + box-shadow (300ms)
b) **섹션 fade-up**: IntersectionObserver, `.reveal` 클래스, opacity 0→1, translateY(30px→0) (600ms)
c) **Stagger reveal**: 카드/항목에 transition-delay 150ms 간격
d) **Hero 패럴랙스**: 별도 `.hero-bg` div, JS scroll listener로 translateY(scrolled * 0.4)
e) **Hero 텍스트 stagger**: badge → headline → subline → CTA 순서로 200ms 간격 fadeSlideUp
f) **SCROLL 인디케이터**: Hero 하단, bounce 2s infinite
g) **카운터 카운트업**: IntersectionObserver, requestAnimationFrame, 2s ease-out, 한 번만 실행
h) **서비스 카드 hover**: 이미지 scale(1.05) + gradient overlay 강화 (500ms)
i) **뉴스 카드 hover**: translateY(-4px) + border-bottom 2px #C23B3B (300ms)
j) **아코디언 toggle**: max-height 0→auto, opacity (300ms)

### 4. 카피 규칙
- 아래 제공된 카피만 사용. 임의 문구 절대 삽입 금지
- 감성 전환 문구, 설교형 closing 금지
- "함께해요", "시작하세요" 같은 감성 종결 금지

---

## 기술 요구사항

- 단일 HTML 파일 (CSS, JS 모두 인라인)
- 모바일 반응형 필수 (breakpoint: 768px)
- Google Fonts CDN + preconnect
- 모든 섹션에 section id 부여
- 이미지: unsplash 실사 or placehold.co 사용
- 모든 CSS/HTML/JS에 한국어 주석 필수
- HTML 코드만 출력. 설명이나 마크다운 코드블록 없이 <!DOCTYPE html>부터 </html>까지만

---

## ★★★ DESIGN ENHANCEMENT DIRECTIVES (이전 버전 대비 반드시 개선) ★★★

이전 빌드에서 발견된 품질 문제를 반드시 해결하라:

### E1. 서비스 카드 — 기본 상태에서 콘텐츠 보여야 함
- 이전 문제: service-card-content가 opacity: 0으로 hover 전에 완전히 숨김 → 무슨 카드인지 모름
- 수정: 기본 상태에서 카테고리 + 타이틀은 보여야 한다. hover 시 추가 설명/링크 등장 + 이미지 scale 효과
- 카드 하단에 항상 보이는 gradient overlay (투명→rgba(0,0,0,0.7)) + 텍스트 배치

### E2. 카운터 포맷팅
- "8년+" "50개+" "200만 건+" "13개" — 접미사(년+, 개+, 만 건+, 개)를 별도 span으로 분리
- 숫자(Inter 700 코랄) + 접미사(Inter 400 서브 색상, 약간 작게)
- data-target에는 순수 숫자만, data-suffix에 접미사 저장
- 카운트업 JS에서 완료 후 접미사 추가 표시

### E3. 파트너 로고 16개 (4x4)
- placeholder 16개 전부 배치 (4열 x 4행)
- 다양한 placeholder 텍스트: Coupang, GS25, Olive Young, TikTok, Douyin, 华西子, TMALL, JD.com, 大韓通運, CJ대한통운, 순풍택배, Taobao, Pinduoduo, Naver, 11번가, WeChat

### E4. 네비게이션 — 서브페이지 링크 연결
- 실제 서브페이지 연결: 회사소개→about.html, 네트워크→network.html, 사업영역(드롭다운)→각 서브페이지
- 서비스 카드도 실제 링크: 물류→logistics.html, 크로스보더→crossborder.html, 유통→crossborder.html, 라이브커머스→live-commerce.html
- CTA 버튼: Contact Us→contact.html, 파트너→partner.html, 셀러→seller.html

### E5. 시각적 정교함 강화
- **그라디언트 테두리**: 서비스 카드에 미세한 1px border (rgba(255,255,255,0.08)) 추가
- **섹션 간 자연스러운 전환**: 섹션 경계에 subtle gradient divider (예: 8px height, 위 섹션색→아래 섹션색)는 사용하지 마라. 대신 섹션 배경색 교차로 자연스러운 리듬
- **텍스트 그라데이션**: Hero 헤드라인에 subtle white→light-gray gradient (text-fill)
- **호버 마이크로인터랙션**: 모든 링크/버튼에 미세한 transition 반드시 포함
- **뉴스 카드 이미지**: 실제 unsplash 비즈니스/물류 이미지 3개 (각각 다른 이미지)
- **Value Prop 이미지**: unsplash에서 전문적인 비즈니스 팀/오피스 이미지

### E6. 모바일 네비게이션 완성도
- 햄버거 → 풀스크린 오버레이 (다크 배경)
- 메뉴 열릴 때 body overflow hidden
- 각 링크 클릭 시 메뉴 자동 닫힘
- 닫기(X) 애니메이션 — 3줄→X 전환

### E7. 헤더 2depth 네비게이션
- "사업영역" 메뉴에 hover 시 드롭다운: 물류·통관, 크로스보더 컨설팅, 유통·마케팅, 라이브커머스 MCN
- 드롭다운: 다크 배경 (#111827), 각 항목 hover 시 코랄 좌측 라인 + 배경 밝아짐
- 모바일에서는 드롭다운 대신 accordion 확장

### E8. 푸터 완성도
- 3컬럼: 회사정보 | Quick Links (서브페이지 전체) | Services (4개 서비스)
- 하단: Copyright + 개인정보처리방침
- email: mailto, phone: tel 링크 반드시

---

## 디자인 시스템 (공통 토큰)

### CSS VARIABLES

```css
:root {
  /* === Color System === */
  --color-primary: #1E3A6E;
  --color-secondary: #F9F7F4;
  --color-accent: #C23B3B;
  --color-accent-soft: #E8734A;

  /* Backgrounds (다크 모드 기조) */
  --color-bg: #0A0F1C;
  --color-bg-dark: #0A0F1C;
  --color-bg-dark2: #111827;
  --color-bg-darker: #080D18;
  --color-bg-navy: #1E3A6E;
  --color-bg-light: #F9F7F4;
  --color-surface: #111827;

  /* Text */
  --color-text: #1A1D2B;
  --color-text-secondary: #6B7280;
  --color-text-on-dark: #FFFFFF;
  --color-text-on-dark-sub: #E8E4DF;

  /* Border */
  --color-border: #E5E7EB;

  /* === Typography === */
  --font-display: 'Pretendard', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-body: 'Pretendard', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-accent: 'Inter', sans-serif;
  --font-chinese: 'Noto Sans SC', sans-serif;

  /* === Spacing === */
  --section-gap-desktop: 120px;
  --section-gap-mobile: 80px;
  --section-padding-desktop: 80px;
  --section-padding-mobile: 48px;
  --container-max: 1200px;
  --container-padding-desktop: 32px;
  --container-padding-mobile: 20px;

  /* === Components === */
  --radius: 0;
  --shadow-card: none;
  --shadow-card-hover: 0 8px 24px rgba(0, 0, 0, 0.3);

  /* === Motion === */
  --ease-default: cubic-bezier(0.25, 0.46, 0.45, 0.94);
  --duration-base: 0.6s;
  --delay-step: 0.15s;
}
```

### GOOGLE FONTS CDN

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/variable/pretendardvariable-dynamic-subset.min.css" />
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;500;700&display=swap" rel="stylesheet">
```

### TYPOGRAPHY

| Role | Desktop | Mobile | Weight | Line Height | Font |
|------|---------|--------|--------|-------------|------|
| Headline (Hero) | 56px | 36px | 700 | 1.2 | Pretendard |
| Section Title | 40px | 28px | 700 | 1.25 | Pretendard |
| Body | 18px | 16px | 400 | 1.7 | Pretendard |
| Caption / Label | 14px | 13px | 500 | 1.5 | Pretendard |
| Counter Numbers | 40px | 32px | 700 | 1.1 | Inter |
| English Labels | 14px | 13px | 600 | 1.4 | Inter |

### BACKGROUND RHYTHM (홈페이지)
```
Hero:          #0A0F1C + image overlay — DARK-1
Value Prop:    #0A0F1C — DARK-1
Trust Badges:  #111827 — DARK-2
Services:      #111827 — DARK-2
Partner Logos: #F9F7F4 — LIGHT (유일한 밝은 포인트)
News:          #0A0F1C — DARK-1
CTA:           #1E3A6E — NAVY
Footer:        #080D18 — DARKER
```

### MOTION
금지: fullPage.js, 스크롤 하이재킹, pin-scroll, bounce/spring easing, background particles, 3D transforms, GSAP

### AI SLOP HARD GATE
- copy.json에 없는 문구 임의 삽입 금지
- 감성 전환/설교형/마케팅 톤 금지
- 같은 컴포넌트 패턴 3회 반복 금지
- hover 없는 카드/버튼 금지

---

## 카피 (copy.json — 최우선)

```json
{
  "is_main": true,
  "sections": [
    {
      "id": "hero",
      "headline": "한중을 잇고, 브랜드의 성장을 세계 무대로 확장합니다",
      "subline": "크로스보더 전문 역량과 현지 운영 인프라를 기반으로 최적의 한국 시장 진출 솔루션을 제공합니다.",
      "cta_primary": "Contact Us",
      "badge": "Cross-border Solution Provider"
    },
    {
      "id": "value-proposition",
      "section_label": "ABOUT KOREAMCN",
      "headline": "한중 크로스보더 비즈니스의 전 과정을 운영합니다",
      "body": "한국 기업 및 브랜드의 중국 시장 진출에 필요한 전 과정을 지원합니다. 크로스보더 전자상거래 운영, 공급망 및 상품 유통, 보세창고 및 물류 관리, 라이브커머스 연계, 현지 운영까지 — 시장 진입부터 운영 안정화까지 체계적으로 운영합니다.",
      "cta": "회사소개 바로가기",
      "cta_link": "about.html"
    },
    {
      "id": "trust-badges",
      "items": [
        { "value": "8", "suffix": "년+", "label": "Years of Experience" },
        { "value": "50", "suffix": "개+", "label": "Partner Companies" },
        { "value": "200", "suffix": "만 건+", "label": "Monthly Shipments" },
        { "value": "13", "suffix": "개", "label": "Global Warehouses" }
      ]
    },
    {
      "id": "services",
      "section_label": "BUSINESS AREA",
      "headline": "사업영역",
      "cards": [
        {
          "category": "Logistics & Warehouse",
          "title": "물류·통관",
          "description": "13개 해외 직영 창고, 월 200만 건 처리",
          "link": "logistics.html"
        },
        {
          "category": "Crossborder Consulting",
          "title": "크로스보더 컨설팅",
          "description": "KC인증, 법인 설립, 세무까지 행정 전 과정 대행",
          "link": "crossborder.html"
        },
        {
          "category": "Distribution & Marketing",
          "title": "유통·마케팅",
          "description": "쿠팡, GS25, 올리브영 등 온·오프라인 동시 유통",
          "link": "crossborder.html"
        },
        {
          "category": "Live Commerce & MCN",
          "title": "라이브커머스 MCN",
          "description": "TikTok Korea Top MCN, 자체 스튜디오 운영",
          "link": "live-commerce.html"
        }
      ]
    },
    {
      "id": "partner-logos",
      "headline": "Partners",
      "subline": "물류, 유통, 뷰티, 미디어 분야의 파트너와 함께합니다."
    },
    {
      "id": "news-highlight",
      "headline": "News & Press",
      "subline": "KOREAMCN의 최신 소식",
      "view_all_link": "news.html",
      "items": [
        { "title": "KOREAMCN, 틱톡 코리아 Top MCN 파트너사 선정", "date": "2024.07.15" },
        { "title": "군포 스마트 물류센터 확장 이전 완료, 월 처리량 300만 건 돌파", "date": "2024.06.28" },
        { "title": "중국 뷰티 브랜드 '화시즈' 한국 런칭 파트너십 체결", "date": "2024.05.10" }
      ]
    },
    {
      "id": "cta",
      "headline": "비즈니스 문의",
      "buttons": [
        { "text": "Contact Us", "style": "primary", "link": "contact.html" },
        { "text": "파트너 모집 알아보기", "style": "secondary", "link": "partner.html" },
        { "text": "셀러 입점 알아보기", "style": "secondary", "link": "seller.html" }
      ]
    }
  ]
}
```

## Footer 카피

```json
{
  "company_name": "KOREAMCN",
  "address": "경기도 군포시 번영로 82-23",
  "email": "woohyunkim@kmhldgs.com",
  "phone": "010-5650-1965",
  "copyright": "© 2026 KOREAMCN. All rights reserved.",
  "privacy_link": "개인정보처리방침"
}
```

---

## 이 페이지 레이아웃 가이드

### Section: hero
- **배경**: DARK-1 + 이미지 오버레이 (linear-gradient(rgba(10,15,28,0.65), rgba(10,15,28,0.8)))
- **레이아웃**: 100vh, 콘텐츠 좌측 정렬 (max-width 700px), 수직 중앙
- **구성**: 인증 배지(pill) → 헤드라인(56px, subtle white gradient text) → 서브카피(18px) → CTA 버튼(코랄)
- **하단**: SCROLL 인디케이터 (bounce 2s infinite)
- **모바일**: 헤드라인 36px, CTA 풀폭

### Section: value-proposition
- **배경**: DARK-1
- **레이아웃**: 2컬럼 (좌: 이미지, 우: 텍스트)
- **모바일**: 1열 스택 (이미지 먼저)

### Section: trust-badges
- **배경**: DARK-2
- **레이아웃**: 4열 수평 그리드, 구분선 (1px rgba(255,255,255,0.1))
- **구성**: 숫자(Inter 40px 코랄) + 접미사(약간 작게, 서브색) + 라벨(14px 서브)
- **인터랙션**: 카운트업 애니메이션
- **모바일**: 2x2, 숫자 32px

### Section: services
- **배경**: DARK-2
- **레이아웃**: 2x2 대형 이미지 카드 그리드
- **카드**: 높이 400px, 이미지 풀 배경 + gradient overlay
  - 기본 상태: 카테고리(Inter 14px) + 타이틀(28px) 항상 보임
  - hover: 이미지 scale(1.05), 설명 텍스트 등장, overlay 강화
- **링크**: 각 카드 → 실제 서브페이지 연결

### Section: partner-logos
- **배경**: LIGHT (유일한 밝은 섹션)
- **레이아웃**: 타이틀 + 서브카피 → 로고 그리드 (4열 x 4행, 16개)
- **hover**: scale(1.05), grayscale→color
- **모바일**: 3열

### Section: news-highlight
- **배경**: DARK-1
- **레이아웃**: 좌측 타이틀 + 우측 "전체 보기 →" → 3열 카드 그리드
- **카드**: 실제 unsplash 이미지(각각 다른) + 제목 + 날짜
- **모바일**: 1열

### Section: cta
- **배경**: NAVY
- **레이아웃**: 중앙 정렬, max-width 700px
- **모바일**: 버튼 풀폭 세로 스택

### Section: footer
- **배경**: DARKER
- **구성**: 3컬럼 (회사정보 | Quick Links | Services) + Copyright

---

위 명세를 정확히 따라 완성된 HTML 파일 하나를 생성해라.
HTML 코드만 출력하고, 설명이나 마크다운 코드블록 없이 <!DOCTYPE html>부터 </html>까지만 출력해라.
