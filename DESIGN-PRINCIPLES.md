# SIGNATURE LAB 디자인 원칙

> 토스(Toss) 웹디자인 분석 기반 + shadcn/ui 컴포넌트 매핑 가이드
> 이 문서는 insusemina 프로젝트의 UI 수정 시 참고 자료입니다.

---

## 1. 컬러 시스템

### 토스 디자인에서 추출한 컬러

| 역할 | 토스 컬러 | 현재 프로젝트 | shadcn 변수 |
|------|-----------|-------------|------------|
| Primary (액센트) | `#3182F6` (토스 블루) | `--accent: #3182f6` | `--primary` |
| Background | `#FFFFFF` | `--bg: #ffffff` | `--background` |
| Background Alt | `#F7F8FA` (연한 회색) | `--bg2: #f7f8fa` | `--muted` |
| Text Primary | `#191F28` (거의 검정) | `--text: #191f28` | `--foreground` |
| Text Secondary | `#4E5968` | `--text2: #4e5968` | `--muted-foreground` |
| Text Tertiary | `#8B95A1` | `--text3: #8b95a1` | `--muted-foreground` (lighter) |
| Text Disabled | `#B0B8C1` | `--text4: #b0b8c1` | 사용 자제 |
| Border | `#E5E8EB` | `--border: #e5e8eb` | `--border` |
| Destructive | `#EF4444` | `--red: #ef4444` | `--destructive` |

### 브랜드 고유 컬러 (유지)

| 역할 | 값 | 용도 |
|------|-----|------|
| Navy | `#03264F` | CTA 버튼, 강조 |
| Blue | `#05357D` | Navy hover 상태 |
| Copper | `#CC886D` | 포인트 강조 (숫자 등) |

### 원칙
- **배경은 흰색(#FFF) + 연회색(#F7F8FA) 교차** 사용으로 섹션 구분
- 다크 배경은 **풋터/비즈니스 영역**에만 제한적 사용 (`#191F28`)
- 액센트 블루는 **eyebrow 텍스트, 배지, 링크**에만 사용 (과용 금지)

---

## 2. 타이포그래피

### 폰트
- **Pretendard Variable** (현재 사용 중, 유지)
- fallback: `-apple-system, BlinkMacSystemFont, sans-serif`

### 사이즈 체계 (토스 기준)

| 용도 | 사이즈 | Weight | Line Height | Letter Spacing |
|------|--------|--------|-------------|---------------|
| Hero 제목 | 44px → 모바일 28px | 700 (Bold) | 1.35 | -0.03em |
| 섹션 제목 | 36px → 모바일 24px | 700 | 1.35 | -0.03em |
| 카드 제목 | 16-26px | 700 | 1.4 | -0.02em |
| 본문 | 15-16px | 400 | 1.7-1.8 | 0 |
| Eyebrow | 13-14px | 600-700 | 1.5 | 0 |
| 캡션/작은 텍스트 | 12-13px | 500 | 1.5 | 0 |

### 원칙
- **제목은 크고 굵게, 본문은 가볍게** (대비 극대화)
- 제목 letter-spacing은 항상 **음수** (-0.02em ~ -0.03em)
- 토스 스타일: 제목에서 **줄바꿈으로 리듬감** 부여 (2-3줄 끊어 읽기)
- 강조 텍스트는 `<strong>` + `color: var(--text)` 조합

### shadcn 매핑
```
h1 → text-4xl font-bold tracking-tight (Hero)
h2 → text-3xl font-bold tracking-tight (Section)
h3 → text-xl font-bold (Card)
p  → text-base text-muted-foreground leading-relaxed (Body)
small → text-sm font-semibold text-primary (Eyebrow)
```

---

## 3. 레이아웃 구조

### 토스 페이지 구조 패턴

```
[Fixed Header - 64px, 반투명 블러]
[Hero Section - 풀스크린, 2-column grid]
[Feature Section - 흰 배경]
  └ eyebrow + 대제목 + 부제목
  └ 2-column: 텍스트 왼쪽 + 이미지 오른쪽 (교차 배치)
[Feature Section - 회색 배경]
  └ 같은 패턴, 배경만 교차
[Grid Cards Section]
  └ 2x2 또는 3열 카드 그리드
[Dark Section - 비즈니스/풋터]
[Footer - 다단 링크 구조]
```

### 핵심 수치

| 요소 | 값 |
|------|-----|
| 최대 너비 | `1080px` (현재 유지) |
| 섹션 패딩 | `120px 24px` → 모바일 `80px 16px` |
| 카드 border-radius | `16px ~ 24px` |
| 카드 그림자 | `0 4px 24px rgba(0,0,0,0.06)` |
| 그리드 gap | `16px ~ 20px` |
| Header 높이 | `64px` |

### shadcn 매핑
```
Container  → max-w-5xl mx-auto px-6
Section    → py-24 (또는 py-20)
Card       → shadcn <Card> with rounded-2xl
Grid       → grid grid-cols-1 md:grid-cols-2 gap-5
```

---

## 4. 컴포넌트 매핑 (shadcn/ui)

### 4.1 Header / Navigation

**토스 패턴**: 고정 헤더, 반투명 블러 배경, 로고 왼쪽 + 네비 오른쪽

```
shadcn: NavigationMenu 또는 커스텀
- fixed top-0, backdrop-blur-xl
- bg-white/85
- border-b (스크롤 시)
- h-16
```

### 4.2 Badge (Eyebrow 텍스트)

**토스 패턴**: 섹션 상단 파란 작은 텍스트 (홈 · 소비, 송금, 대출, 신용 등)

```
shadcn: <Badge variant="secondary">
- text-sm font-bold text-primary
- 배경 없이 텍스트만 사용하거나
- bg-blue-50 text-blue-600 rounded-full px-4 py-1
```

현재 프로젝트의 `.s-eyebrow`, `.hero-badge` 가 이에 해당

### 4.3 Card

**토스 패턴**: 둥근 모서리, 얇은 테두리, 미세한 그림자, 호버 시 살짝 올라감

```
shadcn: <Card>
- rounded-2xl (16px) 또는 rounded-3xl (24px)
- border border-border
- shadow-sm → hover:shadow-lg hover:-translate-y-1
- transition-all duration-200
```

현재 프로젝트의 `.stat-card`, `.testi-card`, `.curri-card` 에 적용

### 4.4 Button

**토스 패턴**:
- Primary: 진한 배경 + 흰 텍스트, 큰 패딩, 둥근 모서리
- 호버 시: 위로 살짝 이동 + 그림자 확대

```
shadcn: <Button>
- Primary: bg-navy text-white rounded-xl px-9 py-4 font-bold
  → hover: -translate-y-0.5 shadow-lg
- Secondary: bg-muted text-foreground
- Ghost: text-muted-foreground hover:text-foreground
```

### 4.5 Accordion (FAQ)

**토스 패턴**: 깔끔한 구분선, Q 마크 강조, +/- 토글

```
shadcn: <Accordion type="single" collapsible>
- border-b border-border
- trigger: py-5 font-semibold
- content: text-muted-foreground text-sm leading-relaxed
```

### 4.6 Input / Form

**토스 패턴**: 밑줄형 인풋, 포커스 시 색상 변경

```
shadcn: <Input> 커스텀
- border-0 border-b-2 border-muted rounded-none
- focus:border-primary
- placeholder:text-muted-foreground/50
```

### 4.7 Separator (섹션 구분)

**토스 패턴**: 배경색 교차로 자연스러운 구분

```
- 흰 섹션: bg-background
- 회색 섹션: bg-muted (또는 bg-slate-50)
- 다크 섹션: bg-foreground text-background (풋터용)
```

---

## 5. 애니메이션 & 인터랙션

### 토스 패턴

| 효과 | 구현 |
|------|------|
| Fade-up 진입 | opacity 0→1, translateY 24px→0 |
| 호버 리프트 | translateY(-4px) + shadow 증가 |
| 부드러운 전환 | `cubic-bezier(0.16, 1, 0.3, 1)` |
| 스크롤 트리거 | IntersectionObserver (threshold 0.08) |
| 헤더 전환 | 스크롤 시 border + shadow 추가 |

### 원칙
- **모든 애니메이션은 0.2s ~ 0.7s** 범위
- ease-out 또는 spring 커브 사용
- 과도한 애니메이션 금지 (토스는 미니멀)
- 모바일에서는 **fade-up만 유지**, 호버 효과 제거

### shadcn 매핑
```
transition-all duration-200    → 호버 효과
transition-all duration-700    → 스크롤 진입 효과
animate-in fade-in slide-in-from-bottom-6 → fade-up 대체
```

---

## 6. 반응형 브레이크포인트

| 브레이크포인트 | 변경사항 |
|---------------|---------|
| `≤768px` (md 이하) | 1열 레이아웃, 제목 축소, 패딩 축소 |
| `>768px` | 2열 그리드, 풀사이즈 타이포 |

### 핵심 반응형 규칙

```
- grid-cols-2 → grid-cols-1 (md 이하)
- hero 제목: 44px → 28px
- 섹션 제목: 36px → 24px
- 섹션 패딩: 120px → 80px
- 카드 패딩: 48px → 28px
- header 패딩: 32px → 16px
```

---

## 7. 토스 디자인의 핵심 원칙 요약

### 7.1 여백이 곧 디자인이다
- 섹션 간 **120px** 이상의 넉넉한 패딩
- 요소 간 충분한 gap (16px~20px)
- 빈 공간을 두려워하지 않음

### 7.2 한 화면에 하나의 메시지
- 각 섹션은 **eyebrow + 대제목 + 설명 + 콘텐츠** 구조
- 하나의 섹션에서 하나의 기능/가치만 전달
- 텍스트 왼쪽 + 비주얼 오른쪽 (또는 교차)

### 7.3 시각적 위계를 명확하게
```
1순위: 대제목 (44px, 700, 검정)
2순위: 부제목 (16px, 400, 회색)
3순위: eyebrow (14px, 700, 파랑)
4순위: 카드 내용 (14-15px, 400, 연회색)
```

### 7.4 신뢰감을 주는 디자인
- **깨끗한 흰 배경** 기반
- 모서리는 항상 **둥글게** (16px+)
- 그림자는 **미세하게** (rgba 0.04~0.06)
- 색상은 **절제** (블루 하나 + 뉴트럴 톤)

### 7.5 스크롤이 곧 스토리텔링
- 위에서 아래로 자연스러운 흐름:
  ```
  Hero (첫인상) → 브랜드 소개 → 통계/성과 → 강의 정보
  → 후기 → 고민(공감) → 해결책 → 커리큘럼 → 혜택
  → 강사 → FAQ → CTA
  ```
- 각 섹션은 scroll-trigger로 fade-up 진입

---

## 8. 현재 프로젝트 개선 포인트

### 이미 잘 되어 있는 것
- Pretendard 폰트 사용
- 토스 스타일 컬러 변수 시스템
- fade-up 스크롤 애니메이션
- 고정 헤더 + 하단 CTA 바
- 반응형 레이아웃

### 개선 가능한 부분
1. **이미지 에셋 부족** - `sol-img` 영역이 플레이스홀더 상태
2. **후기 카드 이미지** - 이모지(👤) 대신 실제 이미지 또는 아바타 필요
3. **강사 사진** - 플레이스홀더 상태
4. **다크 섹션 활용** - 풋터를 토스처럼 다단 링크 구조로 확장 가능
5. **Bottom CTA 바** - 모바일에서 좌우 패딩 더 줄이고 버튼 풀너비 고려

---

## 9. shadcn/ui 도입 시 참고

현재 프로젝트는 **순수 HTML/CSS**로 구성되어 있습니다.
shadcn/ui를 직접 도입하려면 React/Next.js 전환이 필요하지만,
**shadcn의 디자인 토큰과 스타일 원칙**은 현재 CSS에도 적용 가능합니다.

### CSS 변수를 shadcn 방식으로 정리

```css
:root {
  --background: #ffffff;
  --foreground: #191f28;
  --muted: #f7f8fa;
  --muted-foreground: #8b95a1;
  --primary: #03264F;        /* navy - 브랜드 primary */
  --primary-foreground: #ffffff;
  --accent: #3182f6;         /* 토스 블루 */
  --accent-foreground: #ffffff;
  --destructive: #ef4444;
  --border: #e5e8eb;
  --ring: #3182f6;
  --radius: 16px;
}
```

### 유틸리티 클래스 (Tailwind 없이 참고용)

```css
.rounded-2xl { border-radius: 16px; }
.rounded-3xl { border-radius: 24px; }
.shadow-sm { box-shadow: 0 2px 8px rgba(0,0,0,0.04); }
.shadow-md { box-shadow: 0 4px 24px rgba(0,0,0,0.06); }
.shadow-lg { box-shadow: 0 12px 40px rgba(0,0,0,0.08); }
```

---

*이 문서는 토스 웹사이트(toss.im) 디자인 분석 + shadcn/ui 디자인 시스템을 기반으로 작성되었습니다.*
