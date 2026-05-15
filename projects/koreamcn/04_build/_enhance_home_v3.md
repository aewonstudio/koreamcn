# HOME PAGE — v3 (Design System Compliant + Safe Animation)

## CRITICAL CONSTRAINTS (위반 시 전체 재작성)

### 1. 섹션 순서 (절대 변경 금지)
Header → hero → value-proposition → trust-badges → services → partner-logos → news-highlight → cta → footer

### 2. Hero 좌측 정렬 (필수)
```css
#hero .container { max-width: none; width: 100%; padding-left: clamp(40px, 8vw, 160px); padding-right: 40px; }
.hero-content { max-width: 700px; }
```

### 3. 카피 규칙
- 아래 [카피] 섹션에 제공된 카피만 사용. 임의 문구 절대 삽입 금지
- "Cross-border Solution Provider" 같은 배지/칩 삽입 금지
- 감성 전환 문구, 설교형 closing 금지

### 4. 디자인 시스템 토큰 강제
아래 CSS VARIABLES 섹션의 값을 그대로 사용. 임의 변경 금지.

---

## 기술 요구사항

- 단일 HTML 파일 (CSS, JS 모두 인라인)
- 모바일 반응형 필수 (breakpoint: 768px)
- Google Fonts CDN + preconnect
- 모든 섹션에 section id 부여
- 이미지: unsplash 실사 사용
- HTML 코드만 출력. <!DOCTYPE html>부터 </html>까지만

---

## CSS VARIABLES (design-system.md에서 복사 — 값 변경 금지)

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
  --shadow-card: none;
  --shadow-card-hover: 0 8px 24px rgba(0, 0, 0, 0.3);
  --ease-default: cubic-bezier(0.25, 0.46, 0.45, 0.94);
  --duration-base: 0.6s;
  --delay-step: 0.15s;
}
```

**반드시 지켜야 할 토큰:**
- `--radius: 0` → 모든 border-radius는 0. 버튼, 카드, 드롭다운, 로고 pill 전부 0.
- `--shadow-card: none` → 카드 기본 상태에 shadow 없음
- `section { padding: var(--section-padding-desktop) 0; }` → 80px
- 버튼: font-family: var(--font-display), font-weight: 700, font-size: 18px
- body-text: line-height: 1.7
- hero headline: line-height: 1.2, letter-spacing: -0.02em
- section-title: line-height: 1.25, letter-spacing: -0.02em

---

## GOOGLE FONTS CDN

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/variable/pretendardvariable-dynamic-subset.min.css" />
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<link href="https://fonts.googleapis.com/css2?family=Noto+Sans+SC:wght@400;500;700&display=swap" rel="stylesheet">
```

---

## ★★★ 애니메이션 시스템 (충돌 방지 — 반드시 이 패턴만 사용) ★★★

### 원칙
1. **리빌 애니메이션**: CSS transition + IntersectionObserver만 사용
2. **GSAP**: 패럴랙스, 카운터, 프로그레스바만 (콘텐츠 숨김/보임 절대 금지)
3. **Hero 입장**: CSS @keyframes만 (GSAP 금지)

### 금지 사항 (절대 사용 금지)
- gsap.set()으로 opacity:0, clip-path 등 설정 금지
- gsap.from() / gsap.fromTo()로 리빌 애니메이션 금지
- CSS에서 .line-inner { transform: translateY(100%) } 같은 숨김 금지
- clip-path: inset(100% 0 0 0)으로 콘텐츠 숨기기 금지
- .reveal-text, .reveal-up, .line, .line-inner 클래스 사용 금지

### 리빌 패턴 (이것만 사용)

```css
/* 스크롤 리빌 — CSS transition */
.anim {
    opacity: 0;
    transform: translateY(32px);
    transition: opacity 0.7s var(--ease-default), transform 0.7s var(--ease-default);
}
.anim.is-visible {
    opacity: 1;
    transform: translateY(0);
}
/* stagger 딜레이 */
.anim-d1 { transition-delay: 0.1s; }
.anim-d2 { transition-delay: 0.2s; }
.anim-d3 { transition-delay: 0.3s; }
.anim-d4 { transition-delay: 0.4s; }
```

```js
// IntersectionObserver — 1회만 실행
const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            entry.target.classList.add('is-visible');
            observer.unobserve(entry.target);
        }
    });
}, { threshold: 0.15 });
document.querySelectorAll('.anim').forEach(el => observer.observe(el));
```

### Hero 입장 (CSS @keyframes만)
```css
.hero-content .hero-headline { animation: heroFadeUp 1s var(--ease-default) 0.2s both; }
.hero-content .hero-subline  { animation: heroFadeUp 0.8s var(--ease-default) 0.5s both; }
.hero-content .btn            { animation: heroFadeUp 0.8s var(--ease-default) 0.7s both; }
@keyframes heroFadeUp { from { opacity:0; transform:translateY(30px); } to { opacity:1; transform:translateY(0); } }
```

### GSAP 허용 범위 (이것만)
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js"></script>
<script src="https://unpkg.com/lenis@1.1.18/dist/lenis.min.js"></script>
```

```js
// Lenis 스무스 스크롤
const lenis = new Lenis({ duration: 1.2, easing: (t) => Math.min(1, 1.001 - Math.pow(2, -10 * t)) });
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add((time) => lenis.raf(time * 1000));

// Hero 패럴랙스 (scrub — 위치만 이동, 숨기지 않음)
gsap.to('.hero-bg', { yPercent: -20, ease: 'none', scrollTrigger: { trigger: '#hero', start: 'top top', end: 'bottom top', scrub: true } });

// 카운터 롤업 (숫자만 변경, 숨기지 않음)
document.querySelectorAll('.counter-value').forEach(el => {
    const target = parseInt(el.dataset.count, 10);
    el.textContent = '0';
    const obj = { val: 0 };
    gsap.to(obj, { val: target, duration: 2, ease: 'power2.out',
        scrollTrigger: { trigger: el, start: 'top 85%', once: true },
        onUpdate: () => { el.textContent = Math.round(obj.val); }
    });
});

// 스크롤 프로그레스 바
gsap.to('#progress-bar', { scaleX: 1, ease: 'none', scrollTrigger: { scrub: 0.3 } });
```

---

## 네비게이션

```
KOREAMCN (로고)         About  Network  Business▼  News        [Contact Us] (코랄 배경 버튼)
                                        ├ Logistics
                                        ├ Crossborder
                                        ├ Distribution
                                        └ Live Commerce
```

- Contact Us: 네비 맨 오른쪽, 코랄 배경 + 흰색 텍스트 (filled)
- 드롭다운: 다크 배경, hover 시 코랄 좌측 라인
- 스크롤 시 헤더 배경 전환 (transparent → #0A0F1C + backdrop-filter blur)
- 모바일: 햄버거 → 풀스크린 오버레이
- 모바일에서 .mobile-nav는 display:none → 햄버거 클릭 시 display:flex

---

## BACKGROUND RHYTHM (design-system.md 기준)

```
Hero:          #0A0F1C + image overlay — DARK-1
Value Prop:    #0A0F1C — DARK-1
Trust Badges:  #111827 — DARK-2
Services:      #111827 — DARK-2
Partner Logos: #F9F7F4 — LIGHT (유일한 밝은 섹션)
News:          #0A0F1C — DARK-1
CTA:           #1E3A6E — NAVY
Footer:        #080D18 — DARKER
```

---

## TYPOGRAPHY (design-system.md 기준)

| Role | Desktop | Mobile | Weight | Line Height | Letter Spacing | Font |
|------|---------|--------|--------|-------------|----------------|------|
| Hero Headline | 56px | 36px | 700 | 1.2 | -0.02em | Pretendard |
| Section Title | 40px | 28px | 700 | 1.25 | -0.02em | Pretendard |
| Body | 18px | 16px | 400 | 1.7 | 0 | Pretendard |
| Label | 14px | 13px | 600 | 1.4 | 0.05em | Inter |
| CTA Button | 18px | 17px | 700 | 1.2 | 0 | Pretendard |
| Counter | 40px | 32px | 700 | 1.1 | 0 | Inter |

---

## 카피 (copy.json에서 추출 — 임의 변경 금지)

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
- label: "BUSINESS AREA"
- headline: "사업영역"
- Logistics & Warehouse | "13개 해외 직영 창고, 월 200만 건 처리. 한중 물류를 하나의 시스템으로 운영합니다." → logistics.html
- Certification & Incorporation | "KC인증, KFDA, 상표등록, 법인 설립, 세무까지. 한국 시장 진입에 필요한 행정 전 과정을 대행합니다." → crossborder.html
- Distribution & Marketing | "쿠팡, GS25, 올리브영 등 온·오프라인 동시 유통. 상품에 맞는 채널과 마케팅을 설계합니다." → crossborder.html
- Live Commerce & MCN | "TikTok Korea Top MCN 인증. 자체 스튜디오에서 기획부터 방송, 성과 분석까지 운영합니다." → live-commerce.html

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
### Value Prop: 2컬럼 (좌: 이미지, 우: 텍스트). 이미지 hover scale(1.03)
### Trust Badges: 4열 그리드, 1px 구분선, 카운트업 애니메이션
### Services: label + headline + 2x2 대형 이미지 카드 (높이 400px), hover: bg scale(1.05) + overlay 강화 + 텍스트 translateY(-8px)
### Partners: 마퀴 2줄 (좌→우 / 우→좌). border-radius: 0. 밝은 배경(#F9F7F4)
### News: 좌측 타이틀 + "전체보기→" + 3열 카드 (unsplash 이미지 각각 다르게). hover: translateY(-8px) + shadow
### CTA: Navy 배경, 중앙 정렬, gradient-shift 배경 애니메이션 (은은하게)
### Footer: 3컬럼 + Copyright

---

위 명세를 정확히 따라 완성된 HTML 파일 하나를 생성해라.
HTML 코드만 출력하고, 설명이나 마크다운 코드블록 없이 <!DOCTYPE html>부터 </html>까지만 출력해라.
