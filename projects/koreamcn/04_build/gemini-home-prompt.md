# KoreaMCN HOME PAGE — Gemini Build Prompt

## CRITICAL CONSTRAINTS (이전 빌드에서 실패한 항목 — 반드시 준수)

### 1. 섹션 순서 (절대 변경 금지)
아래 순서를 정확히 따라라. 순서를 바꾸거나 섹션을 누락하면 안 된다:
1. Header (sticky nav)
2. Hero (100vh)
3. ABOUT KOREAMCN (회사 소개)
4. Trust Badges (수치 카운터 밴드)
5. BUSINESS AREA (서비스 카드 4개)
6. Partners (파트너 로고 월)
7. News & Press (뉴스 카드 3개)
8. CTA (최종 전환)
9. Footer

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
    /* margin: 0 auto 금지. 좌측 정렬 유지 */
}
```

### 3. 인터랙션 (전부 구현 필수 — 생략 금지)
이 사이트는 Type B+ 인터랙션이다. 아래 모든 항목을 반드시 구현해라:

a) **Hero 패럴랙스**: 별도 `.hero-bg` div로 배경 이미지 분리. JS scroll listener로 `translateY(scrolled * 0.4)` 적용
b) **Hero 텍스트 stagger 진입**: badge → headline → subline → CTA 순서로 200ms 간격 fadeSlideUp
c) **스티키 헤더**: transparent → scrollY > 50에서 배경색 전환 + box-shadow
d) **섹션 fade-up**: IntersectionObserver, `.reveal` 클래스, opacity 0→1, translateY(30px→0)
e) **Stagger reveal**: 카드/항목에 transition-delay 150ms 간격
f) **카운터 카운트업**: IntersectionObserver 트리거, requestAnimationFrame, 2s ease-out, 한 번만 실행
g) **서비스 카드 hover**: 이미지 scale(1.05) + gradient overlay 강화
h) **뉴스 카드 hover**: translateY(-4px) + border-bottom 2px solid #C23B3B
i) **SCROLL 인디케이터**: Hero 하단, bounce 애니메이션 2s infinite

### 4. 카피 규칙
- 아래 제공된 copy.json 카피만 사용. 임의 문구 절대 삽입 금지
- 감성 전환 문구, 설교형 closing 금지
- "함께해요", "시작하세요", "동행합니다" 같은 감성 종결 금지

---

## 기술 요구사항

- 단일 HTML 파일 (CSS, JS 모두 인라인)
- 모바일 반응형 필수 (breakpoint: 768px)
- Google Fonts CDN + preconnect
- 모든 섹션에 section id 부여
- 이미지: placehold.co 또는 unsplash 임시 사용
- 모든 CSS/HTML/JS에 한국어 주석 필수
- HTML 코드만 출력. 설명이나 마크다운 코드블록 없이 `<!DOCTYPE html>`부터 `</html>`까지만

---

## 폰트 CDN

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/variable/pretendardvariable-dynamic-subset.min.css" />
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Noto+Sans+SC:wght@400;500;700&display=swap" rel="stylesheet">
```

---

## CSS 변수 (정확히 이 값 사용)

```css
:root {
    /* 컬러 */
    --c-primary: #1E3A6E;
    --c-accent: #C23B3B;
    --c-bg-dark: #162D54;
    --c-bg-darker: #0F1D36;
    --c-bg-light: #F9F7F4;
    --c-bg-warm: #F2EDE7;
    --c-bg-white: #FFFFFF;
    --c-text: #FFFFFF;
    --c-text-sub: #E8E4DF;
    --c-text-muted: rgba(255,255,255,0.4);
    --c-text-dark: #1A1D2B;
    --c-text-dark-sub: #6B7280;
    --c-border: rgba(255,255,255,0.1);

    /* 폰트 */
    --font-kr: 'Pretendard Variable', 'Pretendard', sans-serif;
    --font-en: 'Inter', sans-serif;

    /* 레이아웃 */
    --max-w: 1200px;
    --radius: 0;

    /* 모션 */
    --ease: cubic-bezier(0.25, 0.46, 0.45, 0.94);
}
```

---

## 타이포그래피 (정확한 크기 — 변경 금지)

| Role | Desktop | Mobile | Weight | Line Height | Font |
|------|---------|--------|--------|-------------|------|
| Hero Headline | 56px | 36px | 700 | 1.2 | Pretendard |
| Section Title | 40px | 28px | 700 | 1.25 | Pretendard |
| Section Label | 14px | 13px | 600 | 1.4 | Inter |
| Subheadline | 24px | 20px | 600 | 1.4 | Pretendard |
| Body | 18px | 16px | 400 | 1.7 | Pretendard |
| Body Large | 20px | 18px | 400 | 1.6 | Pretendard |
| Caption | 14px | 13px | 500 | 1.5 | Pretendard |
| Counter Number | 40px | 32px | 700 | 1.1 | Inter |
| CTA Button | 18px | 17px | 700 | 1.2 | Pretendard |
| Nav | 17px | - | 500 | - | Pretendard |

위계 규칙:
- label(14px) → [12px gap] → title(40px) → [16px gap] → body(18px) → [48px gap] → content
- 헤드라인-본문 크기비 최소 2.2:1
- weight 혼합 필수: 700(타이틀) / 600(서브) / 400(본문) / 500(라벨)

---

## 간격 시스템

| Element | Desktop | Mobile |
|---------|---------|--------|
| 섹션 padding (top/bottom) | 120px | 80px |
| 컨테이너 좌우 padding | 32px | 20px |
| 카드 간 gap | 24px | 16px |
| 타이틀 → 카드그리드 | 48px | 32px |
| 섹션레이블 → 타이틀 | 12px | 8px |
| 타이틀 → 본문 | 16px | 12px |

핵심: "빽빽하다 = AI slop". 여백이 디자인 품질을 결정한다.

---

## 배경 리듬 (이 순서대로)

```
Hero:           #162D54 + 이미지 오버레이 — DARK
ABOUT:          #F9F7F4 — LIGHT
Trust Badges:   #0F1D36 — DARKER
Services:       #FFFFFF — WHITE
Partners:       #F2EDE7 — WARM
News:           #FFFFFF — WHITE
CTA:            #162D54 — DARK
Footer:         #0F1D36 — DARKER
```

---

## 섹션별 상세 가이드

### Section 1: Header
- 로고(좌) + 메뉴 5개 수평 + Contact Us 버튼(우)
- 메뉴: About, Services, Partnership, Seller, News
- 초기: 투명 배경, 화이트 텍스트
- 스크롤 시: background #162D54, box-shadow, 300ms transition
- Contact Us: background #C23B3B, padding 10px 24px
- 높이: 80px, position: fixed, z-index: 1000

### Section 2: Hero
- 100vh, 풀폭 배경 이미지
- 별도 `.hero-bg` div: absolute, inset 0, background-image + overlay(linear-gradient(rgba(15,29,54,0.65), rgba(15,29,54,0.8)))
- 콘텐츠 좌측 정렬, 수직 중앙
- 배지: "TikTok Korea Top MCN" (pill 스타일, 코랄 배경 15% + 코랄 텍스트)
- 헤드라인: "한중을 잇고, 브랜드의 성장을 세계 무대로 확장합니다"
- 서브카피: "크로스보더 전문 역량과 현지 운영 인프라를 기반으로 최적의 한국 시장 진출 솔루션을 제공합니다."
- CTA: "Contact Us" (코랄 버튼)
- 하단: SCROLL 인디케이터 (bounce 2s infinite)
- stagger 진입: badge 0.3s → headline 0.5s → subline 0.7s → CTA 0.9s

### Section 3: ABOUT KOREAMCN
- 배경: #F9F7F4 (라이트)
- 섹션 레이블: "ABOUT KOREAMCN" (Inter 14px 600, letter-spacing 0.15em, 코랄)
- 타이틀: "한중 크로스보더 비즈니스의 전 과정을 운영합니다" (40px, 다크 텍스트)
- 본문: "한국 기업 및 브랜드의 중국 시장 진출에 필요한 전 과정을 지원합니다. 크로스보더 전자상거래 운영, 공급망 및 상품 유통, 보세창고 및 물류 관리, 라이브커머스 연계, 현지 운영까지 — 시장 진입부터 운영 안정화까지 체계적으로 운영합니다." (18px, 다크서브 텍스트)
- CTA: "회사소개 바로가기" (Outline 버튼, 네이비)
- 레이아웃: 2컬럼 (좌측 텍스트, 우측 키워드 비주얼 or 이미지)

### Section 4: Trust Badges (수치 카운터)
- 배경: #0F1D36
- 4열 수평 그리드, 세로 구분선 (1px rgba(255,255,255,0.1))
- 패딩: 48px top/bottom
- 항목:
  - "8년+" / "Years of Experience"
  - "50개+" / "Partner Companies"
  - "200만 건+" / "Monthly Shipments"
  - "13개" / "Global Warehouses"
- 숫자: Inter 700 40px 코랄
- 라벨: Inter 500 14px 서브텍스트
- 카운트업 애니메이션: IntersectionObserver, 2s ease-out

### Section 5: BUSINESS AREA (서비스 카드)
- 배경: #FFFFFF
- 섹션 레이블: "BUSINESS AREA" (Inter, 코랄)
- 타이틀: "사업영역" (40px, 다크)
- 2x2 대형 이미지 카드 그리드 (desktop), 1열 (mobile)
- 각 카드: 높이 400px, 이미지 풀 배경 + gradient overlay(transparent 50%, rgba(0,0,0,0.7))
- 카드 내: 영문 카테고리(Inter 14px 600) + 한글 서비스명(Pretendard 28px 700) + "자세히 보기 →" 링크
- hover: 이미지 scale(1.05), overlay 강화, 500ms
- 카드 목록:
  1. Logistics & Warehouse / 물류·창고 → logistics.html
  2. Certification & Incorporation / 인증·법인 → crossborder.html
  3. Distribution & Marketing / 유통·마케팅 → crossborder.html
  4. Live Commerce & MCN / 라이브커머스 → live-commerce.html

### Section 6: Partners
- 배경: #F2EDE7 (따뜻한)
- 헤드라인: "Partners" (40px)
- 서브카피: "물류, 유통, 뷰티, 미디어 분야의 파트너와 함께합니다."
- 로고 그리드: 4열 x 4행 (desktop), placeholder 로고 16개
- 로고: grayscale → hover시 컬러 + scale(1.05)

### Section 7: News & Press
- 배경: #FFFFFF
- 섹션 상단: 좌측 "News & Press" (40px) + 우측 "전체 보기 →" 링크(코랄)
- 서브카피: "KOREAMCN의 최신 소식"
- 3열 카드 그리드: 이미지(4:3) + 제목 + 날짜
- 카드: 다크 테두리 or 밝은 배경, hover시 translateY(-4px) + border-bottom 2px #C23B3B
- 뉴스 3개 placeholder

### Section 8: CTA
- 배경: #162D54
- 중앙 정렬, max-width 700px
- 헤드라인: "비즈니스 문의" (40px, 화이트)
- 버튼 그룹 (수평):
  - Primary: "Contact Us" (코랄)
  - Secondary: "파트너 모집 알아보기" (outline dark)
  - Tertiary: "셀러 입점 알아보기" (outline dark)

### Section 9: Footer
- 배경: #0F1D36
- 로고 + 회사정보 + 네비 링크 + Copyright
- 회사: KOREAMCN / 경기도 군포시 번영로 82-23 / woohyunkim@kmhldgs.com / 010-5650-1965
- Copyright: © 2026 KOREAMCN. All rights reserved.

---

## 컴포넌트 다양성 (AI Slop 방지)

한 페이지에 같은 컴포넌트 패턴 3회 반복 금지:
- ABOUT: 2컬럼 텍스트+비주얼
- Trust: 카운터 밴드
- Services: 대형 이미지 카드 그리드
- Partners: 로고 그리드
- News: 이미지+텍스트 카드
- CTA: 블록

총 6종 컴포넌트 — 각각 다른 패턴이어야 한다.

---

## 버튼 스타일

Primary CTA: background #C23B3B, color white, padding 16px 32px, font-size 18px, font-weight 700, border-radius 0
Outline Dark: background transparent, border 2px solid rgba(255,255,255,0.5), color white, padding 14px 30px
Outline Light: background transparent, border 2px solid #1E3A6E, color #1E3A6E, padding 14px 30px
Read More: inline-flex, border 1px solid rgba(255,255,255,0.4), padding 14px 24px, "→" 화살표 포함

---

## 레퍼런스: uanlogis.com

이 사이트는 uanlogis.com의 레이아웃 구조와 다크 톤을 따른다:
- 전체 다크 배경 기조 (네이비)
- 날카로운 엣지 (border-radius: 0)
- 100vh Hero + 좌측 정렬
- 대형 이미지 카드 + gradient overlay
- 여유로운 간격 (섹션 간 120px)
- 스티키 헤더 (투명→불투명)
- SCROLL 인디케이터

---

위 명세를 정확히 따라 완성된 HTML 파일 하나를 생성해라.
HTML 코드만 출력하고, 설명이나 마크다운 코드블록 없이 <!DOCTYPE html>부터 </html>까지만 출력해라.
