# HOME PAGE — Enhanced v2 (Advanced Interactions)

## CRITICAL CONSTRAINTS

### 1. 섹션 순서 (절대 변경 금지)
Header → hero → value-proposition → trust-badges → services → partner-logos → news-highlight → cta → footer

### 2. Hero 좌측 정렬 (필수)
```css
#hero .container { max-width: none; width: 100%; padding-left: clamp(40px, 8vw, 160px); padding-right: 40px; }
.hero-content { max-width: 700px; }
```

### 3. 카피 규칙
- 아래 제공된 카피만 사용. 임의 문구 절대 삽입 금지
- "Cross-border Solution Provider" 같은 배지/칩 삽입 금지
- 감성 전환 문구, 설교형 closing 금지

---

## 기술 요구사항

- 단일 HTML 파일 (CSS, JS 모두 인라인)
- 모바일 반응형 필수 (breakpoint: 768px)
- Google Fonts CDN + preconnect
- 모든 섹션에 section id 부여
- 이미지: unsplash 실사 사용
- 모든 CSS/HTML/JS에 한국어 주석 필수
- HTML 코드만 출력. <!DOCTYPE html>부터 </html>까지만

---

## ★★★ ADVANCED INTERACTIONS (Type A — Awwwards급) ★★★

### 외부 라이브러리 (CDN)
```html
<!-- GSAP + ScrollTrigger -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<!-- Lenis Smooth Scroll -->
<script src="https://unpkg.com/lenis@1.1.18/dist/lenis.min.js"></script>
```

### I1. Lenis 스무스 스크롤
- 페이지 전체에 Lenis 적용. 부드러운 관성 스크롤
- GSAP ScrollTrigger와 Lenis 연동 (requestAnimationFrame)
```js
const lenis = new Lenis({ duration: 1.2, easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)) });
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add((time) => lenis.raf(time * 1000));
gsap.ticker.lagSmoothing(0);
```

### I2. Hero 패럴랙스 (scrub)
- Hero 배경 이미지: 스크롤 시 느리게 이동 (scrub 기반, translateY 0→-20%)
- Hero 텍스트: 스크롤 시 위로 빠르게 이동 + opacity fade-out (scrollTrigger scrub)
```js
gsap.to('.hero-bg', { yPercent: -20, ease: 'none', scrollTrigger: { trigger: '#hero', start: 'top top', end: 'bottom top', scrub: true } });
gsap.to('.hero-content', { y: -100, opacity: 0, ease: 'none', scrollTrigger: { trigger: '#hero', start: 'top top', end: 'bottom top', scrub: true } });
```

### I3. 텍스트 스플릿 + Stagger 리빌
- Hero 헤드라인: 글자 단위가 아닌 **단어 단위** 스플릿 → stagger fadeSlideUp
- 섹션 타이틀들(.section-title): **줄 단위** 스플릿 → stagger reveal
- 구현: span으로 감싸고 GSAP stagger (0.08s per word)
```js
// Hero headline을 단어 단위로 분리
const heroH = document.querySelector('.hero-headline');
heroH.innerHTML = heroH.textContent.split(' ').map(w => `<span class="word">${w}</span>`).join(' ');
gsap.from('.hero-headline .word', { opacity: 0, y: 40, stagger: 0.08, duration: 0.8, delay: 0.3 });
```

### I4. 섹션 스크롤 리빌 (GSAP ScrollTrigger)
- 모든 섹션 콘텐츠: ScrollTrigger로 진입 시 reveal
- fade-up 대신 **clip-path 리빌** 사용 (더 고급):
  - `clip-path: inset(100% 0 0 0)` → `clip-path: inset(0% 0 0 0)` (아래에서 위로 드러남)
- 카드/항목: stagger 0.15s
- 이미지: **마스크 리빌** — overflow hidden container + 이미지 scale(1.2) → scale(1) + container clip-path reveal

### I5. 서비스 카드 — 이미지 줌인 리빌
- 스크롤 진입 시: 카드 배경 이미지가 scale(1.3) → scale(1)로 줌인하며 등장 (1s ease-out)
- hover: 이미지 scale(1.05) + overlay 강화 + 텍스트 translateY(-8px)
- 각 카드 stagger: 0.2s

### I6. Trust Badges 카운터 — GSAP 연동
- ScrollTrigger 진입 시 카운트업 (숫자 롤링)
- 숫자(코랄) + 접미사(서브색, 작게) 분리
- gsap.to로 textContent 업데이트 (snap 사용)

### I7. 파트너 로고 — 무한 마퀴 (선택적)
- 16개 로고를 2줄로 나눠 좌→우 / 우→좌 무한 스크롤 (CSS animation 또는 GSAP)
- hover 시 마퀴 일시정지
- 또는 기존 그리드 유지하되, 스크롤 진입 시 stagger fade-up

### I8. 뉴스 카드 — 호버 이미지 시차
- hover 시 이미지가 카드 이동 방향 반대로 미세하게 이동 (마우스 추적 패럴랙스)
- 또는 간단하게: hover → 이미지 scale(1.08) + translateY(-4px) + shadow 강화

### I9. CTA 섹션 — 배경 그라디언트 시프트
- Navy 배경에 subtle gradient animation (hue-shift 또는 linear-gradient 각도 변화)
- 매우 은은하게 (8s infinite)

### I10. 스크롤 프로그레스 바
- 페이지 최상단에 1px 코랄 라인이 스크롤 진행률에 따라 width 0→100%
- position: fixed, z-index 9999

### 금지 (과하면 안 됨)
- 커스텀 커서 — 불필요
- 로딩 스크린 — 불필요
- 3D transforms — 불필요
- background particles — 불필요
- 의미 없는 바운스/스프링

---

## 네비게이션 구조

```
KOREAMCN (로고)         About  Network  Business▼  News        [Contact Us] (코랄 배경 버튼)
                                        ├ Logistics
                                        ├ Crossborder
                                        ├ Distribution
                                        └ Live Commerce
```

- **Contact Us 버튼**: 네비 맨 오른쪽, **코랄 배경 + 흰색 텍스트** (filled, outline 아님)
- 드롭다운: 다크 배경, hover 시 코랄 좌측 라인
- 스크롤 시 헤더 배경 전환 (transparent → #0A0F1C + box-shadow)
- 모바일: 햄버거 → 풀스크린 오버레이

---

## 디자인 시스템

### CSS VARIABLES
```css
:root {
  --color-primary: #1E3A6E;
  --color-secondary: #F9F7F4;
  --color-accent: #C23B3B;
  --color-accent-soft: #E8734A;
  --color-bg: #0A0F1C;
  --color-bg-dark: #0A0F1C;
  --color-bg-dark2: #111827;
  --color-bg-darker: #080D18;
  --color-bg-navy: #1E3A6E;
  --color-bg-light: #F9F7F4;
  --color-surface: #111827;
  --color-text: #1A1D2B;
  --color-text-secondary: #6B7280;
  --color-text-on-dark: #FFFFFF;
  --color-text-on-dark-sub: #E8E4DF;
  --color-border: #E5E7EB;
  --font-display: 'Pretendard', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-body: 'Pretendard', -apple-system, BlinkMacSystemFont, sans-serif;
  --font-accent: 'Inter', sans-serif;
  --font-chinese: 'Noto Sans SC', sans-serif;
  --section-padding-desktop: 80px;
  --section-padding-mobile: 48px;
  --container-max: 1200px;
  --container-padding-desktop: 32px;
  --container-padding-mobile: 20px;
  --radius: 0;
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
| Role | Desktop | Mobile | Weight | Font |
|------|---------|--------|--------|------|
| Hero Headline | 56px | 36px | 700 | Pretendard |
| Section Title | 40px | 28px | 700 | Pretendard |
| Body | 18px | 16px | 400 | Pretendard |
| Label | 14px | 13px | 600 | Inter |
| Counter | 40px | 32px | 700 | Inter |

### BACKGROUND RHYTHM
```
Hero:          #0A0F1C + image overlay — DARK-1
Value Prop:    #0A0F1C — DARK-1
Trust Badges:  #111827 — DARK-2
Services:      #111827 — DARK-2
Partner Logos: #F9F7F4 — LIGHT
News:          #0A0F1C — DARK-1
CTA:           #1E3A6E — NAVY
Footer:        #080D18 — DARKER
```

---

## 카피

### Hero
- headline: "한중을 잇고, 브랜드의 성장을 세계 무대로 확장합니다"
- subline: "크로스보더 전문 역량과 현지 운영 인프라를 기반으로 최적의 한국 시장 진출 솔루션을 제공합니다."
- cta: "Contact Us" → contact.html
- 배지/칩 삽입 금지

### Value Proposition
- label: "ABOUT KOREAMCN"
- headline: "한중 크로스보더 비즈니스의 전 과정을 운영합니다"
- body: "한국 기업 및 브랜드의 중국 시장 진출에 필요한 전 과정을 지원합니다. 크로스보더 전자상거래 운영, 공급망 및 상품 유통, 보세창고 및 물류 관리, 라이브커머스 연계, 현지 운영까지 — 시장 진입부터 운영 안정화까지 체계적으로 운영합니다."
- cta: "회사소개 바로가기" → about.html

### Trust Badges
- 8년+ (Years of Experience)
- 50개+ (Partner Companies)
- 200만 건+ (Monthly Shipments)
- 13개 (Global Warehouses)

### Services (2x2 이미지 카드)
- Logistics & Warehouse | 물류·통관 | "13개 해외 직영 창고, 월 200만 건 처리" → logistics.html
- Crossborder Consulting | 크로스보더 컨설팅 | "KC인증, 법인 설립, 세무까지 행정 전 과정 대행" → crossborder.html
- Distribution & Marketing | 유통·마케팅 | "쿠팡, GS25, 올리브영 등 온·오프라인 동시 유통" → crossborder.html
- Live Commerce & MCN | 라이브커머스 MCN | "TikTok Korea Top MCN, 자체 스튜디오 운영" → live-commerce.html

### Partners
- title: "Partners"
- subtitle: "물류, 유통, 뷰티, 미디어 분야의 파트너와 함께합니다."
- 16개: Coupang, GS25, Olive Young, TikTok, Douyin, 华西子, TMALL, JD.com, 大韓通運, CJ대한통운, 순풍택배, Taobao, Pinduoduo, Naver, 11번가, WeChat

### News
- title: "News & Press" | subtitle: "KOREAMCN의 최신 소식" | view_all → news.html
- Card 1: "KOREAMCN, 틱톡 코리아 Top MCN 파트너사 선정" | 2024.07.15
- Card 2: "군포 스마트 물류센터 확장 이전 완료, 월 처리량 300만 건 돌파" | 2024.06.28
- Card 3: "중국 뷰티 브랜드 '화시즈' 한국 런칭 파트너십 체결" | 2024.05.10

### CTA
- headline: "비즈니스 문의"
- Contact Us (primary, → contact.html) | 파트너 모집 알아보기 (secondary, → partner.html) | 셀러 입점 알아보기 (secondary, → seller.html)

### Footer
- KOREAMCN | 경기도 군포시 번영로 82-23 | woohyunkim@kmhldgs.com (mailto) | 010-5650-1965 (tel)
- Quick Links: About, Network, News, Contact
- Services: Logistics, Crossborder, Distribution, Live Commerce
- © 2026 KOREAMCN. All rights reserved. | 개인정보처리방침

---

## 레이아웃 가이드

### Hero: 100vh, 좌측 정렬, 이미지 오버레이, SCROLL 인디케이터
### Value Prop: 2컬럼 (좌: 이미지 마스크 리빌, 우: 텍스트)
### Trust Badges: 4열 그리드, 구분선, 카운트업
### Services: 2x2 대형 이미지 카드 (높이 400px), 기본 상태에서 카테고리+타이틀 보임
### Partners: pill 스타일 4x4 그리드 (border pill, 텍스트)
### News: 좌측 타이틀 + "전체보기→" + 3열 카드 (unsplash 이미지 각각 다르게)
### CTA: Navy 배경, 중앙 정렬
### Footer: 3컬럼 + Copyright

---

위 명세를 정확히 따라 완성된 HTML 파일 하나를 생성해라.
HTML 코드만 출력하고, 설명이나 마크다운 코드블록 없이 <!DOCTYPE html>부터 </html>까지만 출력해라.
