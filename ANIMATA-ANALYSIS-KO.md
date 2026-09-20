# Animata 전수조사 분석 보고서 (한국어)

> 저장소 전체를 조사하여 정리한 분석 문서입니다.
> 작성일: 2026-09-20

## 저장소 주소

| 구분 | URL |
| --- | --- |
| 현재 저장소 (포크) | https://github.com/bmshin94/animata |
| 원본 저장소 (upstream) | https://github.com/codse/animata |
| 공식 사이트 | https://animata.design |
| 레지스트리 URL 형식 | `https://animata.design/r/{category}/{name}.json` |
| Discord | https://discord.gg/STYEh3UW |

---

## 1. 프로젝트 정체

**Animata = 복사·붙여넣기 방식의 무료 오픈소스 React 애니메이션 컴포넌트 모음 + 쇼케이스 웹사이트**

npm 패키지가 아니라 shadcn/ui와 동일한 "소스 복사" 방식이다. 설치하면 코드가
사용자 프로젝트로 들어오고, 그 순간부터 사용자 소유 코드가 된다.

| 항목 | 값 |
| --- | --- |
| 버전 | v4.0.0 |
| 라이선스 | MIT (상업적 이용·수정·재배포 자유) |
| 커밋 수 | 112 |
| 기간 | 2024-10-15 ~ 2026-09-18 |
| 주 기여자 | Hari Lamichhane(47), Sudha Shrestha(29), 외부 기여자 13명+ |
| 패키지 매니저 | pnpm 11.5.0 (Node 22) |

### 기술 스택

```
Next.js 16 · React 19 · Tailwind CSS v4 · TypeScript 5
Motion 12 (framer-motion 후속) · Storybook 10 · Velite 0.2 (MDX)
Biome 2 (ESLint+Prettier 대체) · Radix UI · Zod · tsParticles
배포: Cloudflare Pages (wrangler) · 분석: PostHog
이메일: Plunk · DB: Supabase
```

---

## 2. 폴더 구조 분석

### `animata/` — 컴포넌트 소스 (알맹이)

23개 카테고리, 실제 컴포넌트 약 203개 (tsx 414개 중 절반은 `.stories.tsx`), CSS 파일 20개.

| 카테고리 | 개수 | 설명 |
| --- | --- | --- |
| `text/` | 45 | 글자 애니메이션 (roll-text, metis-text 등) |
| `widget/` | 34 | 복잡한 인터랙티브 위젯 |
| `card/` | 27 | 카드 (flip-card, card-stack 등) |
| `button/` | 15 | 버튼 변형 |
| `background/` | 10 | 배경 효과 |
| `bento-grid/` | 10 | 벤토 그리드 레이아웃 |
| `container/` | 8 | 레이아웃 래퍼 (marquee, dock, ribbon) |
| `image/` | 8 | 이미지 효과 |
| `list/` | 8 | 리스트 |
| `skeleton/` | 8 | 로딩 스켈레톤 + 카테고리 글리프 |
| `graphs/` | 6 | 차트 |
| `hero/` | 5 | 히어로 섹션 |
| `tabs/` | 3 | 탭 |
| `carousel/` `fabs/` `feature-cards/` `icon/` `preloader/` `progress/` | 각 2 | 보조 UI |
| `accordion/` `overlay/` `scroll/` `section/` | 각 1 | 단일 컴포넌트 |

**파일 규칙 (CLAUDE.md 명시):**

```
animata/<category>/<name>.tsx          # 컴포넌트 본체
animata/<category>/<name>.stories.tsx  # Storybook 미리보기
animata/<category>/<name>.css          # Tailwind로 표현 불가한 keyframes만
```

- `cn()` (`@/lib/utils`) 사용 필수, 문자열 연결 금지
- 인라인 `<style>` 금지, CSS 모듈/styled-components 금지
- 모든 신규 컴포넌트는 라이트/다크 테마 대응 필수

### 나머지 주요 디렉터리

| 경로 | 역할 |
| --- | --- |
| `app/(main)/` | 랜딩, docs, blog, components 갤러리, resources, text-animations |
| `app/demo/` | 실사용 데모 (library: hero/footer/browse/scroll) |
| `content/docs/` | 컴포넌트 MDX 문서 + changelog(13개월치) + contributing(10종) + guides |
| `content/blog/` | 블로그 (경쟁사 비교글, Hacktoberfest 등) |
| `components/` | 사이트 자체 UI (site-header, mdx-components, registry-install, ads) |
| `config/docs.ts` | 사이드바 네비게이션 설정 |
| `scripts/` | 빌드 자동화 스크립트 14종 |
| `templates/` | component / doc / story 스캐폴딩 템플릿 |
| `.github/` | 이슈 템플릿 3종, PR 템플릿, 배포 워크플로 |
| `CLAUDE.md` | Claude Code용 프로젝트 규칙 문서 |

### `scripts/` — 이 프로젝트의 핵심 자산

| 스크립트 | 역할 |
| --- | --- |
| `build-registry.js` | MDX 문서를 파싱해 shadcn 레지스트리 JSON 자동 생성 (`public/r/`) |
| `build-llms-txt.js` | AI 크롤러용 `llms.txt` 생성 |
| `build-docs-markdown.js` | 문서 마크다운 산출물 생성 |
| `build-og-images.mjs` | 소셜 공유용 OG 이미지 자동 생성 |
| `build-demo-sources.js` | 데모 소스 코드 패널용 번들 |
| `create-new.js` | `pnpm animata:new` — 컴포넌트 스캐폴딩 |
| `validate-demo-registry.js` | 데모 레지스트리 검증 |
| `validate-registry-install.js` | 레지스트리가 실제로 설치되는지 CI 검증 |
| `upload-og-r2.mjs` | Cloudflare R2 업로드 |

### 빌드 파이프라인

```
MDX 문서 1개 작성
   ↓  pnpm build
   ├─ velite            → 콘텐츠 파싱
   ├─ build-registry    → public/r/{category}/{name}.json (설치용)
   ├─ build-docs-md     → 문서 마크다운
   ├─ build-llms-txt    → llms.txt (AI 검색 대응)
   ├─ storybook build   → public/preview (미리보기 iframe)
   └─ next build        → 정적 사이트
```

---

## 3. 자주 묻는 질문 정리

### 3.1 설치 및 사용법

**A. 컴포넌트만 내 프로젝트에 쓰는 경우 (일반적)**

```bash
# 1) 의존성
npm install tailwind-merge clsx lucide-react
npm install motion   # 복잡한 애니메이션 컴포넌트에만 필요

# 2) lib/utils.ts 생성
#    cn() 유틸이 모든 컴포넌트의 전제조건

# 3) tsconfig.json 경로 별칭
#    { "baseUrl": ".", "paths": { "@/*": ["./*"] } }

# 4) 설치 (shadcn CLI 권장 — co-located css까지 한 번에)
npx shadcn@latest add https://animata.design/r/text/roll-text.json
```

```ts
// lib/utils.ts
import { type ClassValue, clsx } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

**B. 이 사이트 자체를 로컬에서 돌리는 경우**

```bash
git clone https://github.com/bmshin94/animata
cd animata
corepack enable && corepack prepare pnpm@11.5.0 --activate
pnpm install

pnpm dev         # Next(3000) + Storybook(6006) + velite watch
pnpm storybook   # 컴포넌트 작업대만
pnpm build       # 프로덕션 빌드 (레지스트리 + OG + llms.txt 포함, 오래 걸림)
pnpm lint:fix    # Biome 자동 수정
pnpm animata:new # 새 컴포넌트 스캐폴딩
```

### 3.2 플러그인인가, 스킬인가, MCP인가?

**셋 다 아니다.** 순수 Next.js 웹앱 + React 컴포넌트 소스 모음이다.

| 종류 | 해당 여부 | 비고 |
| --- | --- | --- |
| MCP 서버 | 아니오 | MCP 관련 코드 없음 |
| Claude Skill | 아니오 | `.claude/skills/` 없음 |
| 플러그인 | 아니오 | 호스트 앱 확장 아님 |
| Next.js 웹앱 + 컴포넌트 모음 | **예** | 실제 정체 |

헷갈리는 이유:

1. `CLAUDE.md` 존재 → Claude Code의 "프로젝트 메모리"(규칙 문서)이지 스킬이 아님
2. shadcn 레지스트리 URL(`/r/{category}/{name}.json`) → API처럼 보이지만 정적 JSON
3. `llms.txt` → AI 크롤러용 텍스트 파일이며 MCP와 무관

### 3.3 API 토큰이 필요한가?

**컴포넌트만 쓴다면 토큰 불필요.** 공개 URL에서 JSON을 받는 구조라 인증이 없다.

사이트를 직접 운영할 때만 환경변수가 쓰이며, 전부 선택사항이다.

| 환경변수 | 서비스 | 없을 때 |
| --- | --- | --- |
| `NEXT_PUBLIC_APP_URL` | 자기 도메인 | OG/레지스트리 URL이 부정확 |
| `NEXT_PUBLIC_POSTHOG_KEY` / `_HOST` | PostHog | 분석만 비활성 |
| `NEXT_PUBLIC_SUPABASE_URL` / `_ANON_KEY` | Supabase | DB 기능만 비활성 |
| `NEXT_PUBLIC_PLUNK_API_KEY` | Plunk | 이메일 구독 폼만 비활성 |
| `CLOUDFLARE_API_TOKEN` / `_ACCOUNT_ID` | Cloudflare Pages | CI 배포만 실패 |

`pnpm install && pnpm dev`는 토큰 없이 동작한다.

> 보안 참고: `.github/workflows/deploy.yml`이 `pull_request_target` 트리거를 사용한다.
> 포크 PR에 시크릿이 노출될 수 있는 패턴이므로, 포크를 실제 운영할 경우 워크플로 검토를 권장한다.

### 3.4 왜 GitHub에서 유명한가?

1. **진입장벽 제로** — 설치·회원가입·토큰 불필요, MIT 라이선스
2. **비주얼 임팩트** — 애니메이션은 스크린샷 한 장으로 설득됨, 애니메이션 SVG 로고
3. **shadcn 생태계 편승** — 레지스트리 표준에 완벽 호환
4. **Hacktoberfest 전략** — `content/blog/hacktoberfest-2024.mdx`, 10월 기여자 유입
5. **SEO/AEO 적극 활용** — 경쟁사 비교 블로그(`aceternity-ui-vs-magic-ui-vs-animata.mdx`),
   롱폼 가이드, OG 이미지 자동 생성, `llms.txt`로 AI 검색 대응
6. **기여 마찰 최소화** — 이슈 템플릿 3종, PR 템플릿, 기여 문서 10종,
   스캐폴딩 CLI, PR마다 Cloudflare 프리뷰 URL 자동 댓글
7. **꾸준함 + 최신성** — 체인지로그 13개월치, 항상 최신 프레임워크 버전 유지

교훈: 별을 만드는 것은 "좋은 코드"보다 **"발견되기 쉽고 기여하기 쉬운 코드"**다.

### 3.5 로컬 에이전트 구축에 도움이 되는가?

직접적 도움은 제한적(UI 영역), 간접적 도움은 크다.

**도움이 되지 않는 부분**
- 컴포넌트는 순수 UI로, 에이전트 로직·LLM 호출·툴 실행과 무관
- MCP 서버, 툴 정의, 에이전트 루프 없음

**도움이 되는 부분**

1. **에이전트 UI 구축** — 채팅(text/), 로딩(preloader/, skeleton/),
   실행 로그(list/, progress/), 대시보드(bento-grid/, graphs/, widget/)
2. **`CLAUDE.md` 작성법의 모범 사례** — 개요 → 파일 규칙 → 금지사항 →
   체크리스트 → 파일맵 → 실행법 구조. 에이전트 지시서에 그대로 응용 가능
3. **`llms.txt`** — AI가 읽기 좋은 문서 산출 패턴
4. **MDX → JSON 파이프라인** — 문서 순회 → frontmatter 파싱 → 코드블록 추출 →
   의존성 자동 감지 및 버전 pin → 구조화 JSON. RAG 인덱싱 파이프라인과 구조가 동일
5. **검증 습관** — `validate-*.js`로 빌드 산출물이 실제로 동작하는지 CI에서 검증.
   에이전트 산출물 검증에도 동일 원칙 적용

요약: "에이전트의 뇌"는 배울 수 없지만, "에이전트의 얼굴(UI)"과
"에이전트를 다루는 규율(규칙 문서 / 파이프라인 / 검증)"은 우수한 참고 자료다.

### 3.6 React나 PHP로 만들 수 있는가?

**React — 이미 React다. 100% 가능.**

| 환경 | 가능 여부 | 비고 |
| --- | --- | --- |
| Next.js 15/16 | 완전 호환 | `"use client"` 이미 적용 |
| Vite + React | 완전 호환 | `@/` 별칭 설정 필요 |
| CRA | 가능 | Tailwind v4 설정 필요 |
| React Native | 불가 | 웹 CSS 기반 |
| Vue / Svelte | 부분 | CSS만 재활용, JSX는 재작성 |

**PHP — 가능하되 포팅 작업이 필요하다.**

애니메이션은 브라우저의 CSS/JS가 수행한다. PHP는 HTML을 출력할 뿐이므로,
마크업 + CSS를 옮기고 상호작용만 JS로 보완하면 된다.

| 난이도 | 비율 | 대상 | 방법 |
| --- | --- | --- | --- |
| 쉬움 | ~70% | CSS keyframes 기반 (marquee, roll-text 등) | 마크업 + CSS 그대로 복사 |
| 보통 | ~25% | 간단한 상태/호버 토글 | Alpine.js 추가 |
| 어려움 | ~5% | Motion, tsParticles 등 물리 기반 | JS 라이브러리 필수, PHP 단독 불가 |

| 스택 | 추천도 | 방법 |
| --- | --- | --- |
| Laravel + Inertia + React | 최상 | PHP 백엔드 + React 그대로 사용 |
| Laravel + Blade + Alpine.js | 최상 | Blade 컴포넌트로 변환, 대부분 복사 수준 |
| Laravel + Livewire | 상 | 서버 상태 + Alpine 하이브리드 |
| WordPress 테마 | 중 | `wp_enqueue_style` + PHP 함수화 |
| 순수 PHP | 하 | `include` + CSS 복사 |

---

## 4. 수익화 아이디어

| # | 아이디어 | 난이도 | 초기비용 | 예상 월수익 | 추천도 |
| --- | --- | --- | --- | --- | --- |
| 1 | 한국어 현지화 사이트 | 하 | ~0 | 30~200만 | ★★★★★ |
| 2 | 프리미엄 템플릿 판매 | 중 | ~0 | 50~500만 | ★★★★★ |
| 3 | Pro 컴포넌트 구독 | 상 | 월 3만 | 100~1000만 | ★★★★ |
| 4 | 광고 + 스폰서 | 하 | 0 | 10~100만 | ★★★ |
| 5 | Figma 디자인 키트 | 중 | 0 | 30~150만 | ★★★ |
| 6 | 제휴 마케팅 | 하 | 0 | 5~50만 | ★★ |
| 7 | 랜딩 제작 외주 | 하 | 0 | 300~1000만 | ★★★★★ |
| 8 | AI 컴포넌트 생성 SaaS | 상 | 월 10만 | 변동 | ★★★★ |
| 9 | 강의 / 전자책 | 중 | 0 | 50~300만 | ★★★★ |
| 10 | Laravel/Vue 포팅 | 중 | 0 | 30~200만 | ★★★ |

> 수익 추정치는 국내 시장 기준의 경험적 추정이며 보장 수치가 아니다.

### 4.1 한국어 현지화 사이트

- 국내에 한국어 React 애니메이션 컴포넌트 사이트가 거의 없어 경쟁이 적다
- MIT 라이선스라 번역·재배포가 합법
- 차별화 포인트: **한글 자모 분해 타이포 애니메이션**, 네이버/카카오/토스 스타일 버튼,
  국내 이커머스 UI, 명절/시즌 이펙트 — 영어권에서 만들 수 없는 영역
- SEO 키워드: "리액트 애니메이션 컴포넌트", "테일윈드 애니메이션"

### 4.2 프리미엄 템플릿/블록 판매

무료 개별 컴포넌트로 유입 → 완성된 섹션·페이지·템플릿을 유료 판매.

| 상품 | 가격(예시) | 구성 |
| --- | --- | --- |
| 섹션 팩 | $19 | 히어로 10종 / 가격표 8종 / FAQ 6종 |
| 랜딩 템플릿 | $49 | SaaS 랜딩 풀페이지 (반응형 + 다크모드) |
| 대시보드 킷 | $79 | 관리자 UI 전체 |
| 올인원 번들 | $149 | 전체 + 평생 업데이트 |

판매 채널: Gumroad, LemonSqueezy(세금 자동 처리), Polar.sh, 크몽/탈잉.
개발자는 "컴포넌트"보다 "조립이 끝난 완성 페이지"에 지갑을 연다.

### 4.3 Pro 컴포넌트 구독 (레지스트리 게이팅)

shadcn CLI는 인증 헤더를 지원하므로 유료 레지스트리 구현이 가능하다.

```
Next.js API Route (/api/r/pro/[...slug])
  → Authorization 헤더에서 라이선스 키 추출
  → Supabase(이미 의존성에 포함)에서 구독 상태 조회
  → Stripe/Paddle 웹훅으로 구독 관리
  → 유효하면 레지스트리 JSON 반환, 아니면 402
```

기존 `build-registry.js`를 확장하는 형태로 구현 가능하다.
가격 예시: 월 $12 / 연 $99 / 평생 $249.

### 4.4 랜딩 제작 외주

포크를 자기 브랜드로 운영하면 사이트 자체가 포트폴리오이자 영업자료가 된다.
컴포넌트가 이미 갖춰져 있어 실제 제작 기간이 크게 단축된다.
영업 경로: 크몽·위시켓·프리모아, 개발자 커뮤니티, 자사 사이트 제작 문의 버튼.

### 4.5 AI 컴포넌트 생성 SaaS

자연어 → 애니메이션 컴포넌트 생성. Animata가 좋은 출발점인 이유:

- 203개 고품질 컴포넌트가 few-shot 예제 데이터셋이 됨
- `llms.txt`로 이미 정리된 문서
- Storybook 미리보기 인프라 완비
- `CLAUDE.md`에 코딩 컨벤션이 명문화되어 일관된 출력 유도 가능

리스크: LLM API 비용 관리, v0/Lovable 등 경쟁 서비스.

### 4.6 기타

- **광고/스폰서** — `components/ads.tsx`가 이미 존재. EthicalAds/Carbon Ads, GitHub Sponsors
- **Figma 디자인 키트** — 컴포넌트를 Figma로 변환해 Figma Community/Gumroad 판매
- **제휴 마케팅** — Vercel/Cloudflare/Framer/Supabase 레퍼럴.
  원본 README 뱃지에 이미 `?ref=animata.design`가 적용되어 있음
- **강의/전자책** — "React 애니메이션 마스터", "Tailwind v4 + Motion 실전".
  203개 컴포넌트가 그대로 커리큘럼이 됨
- **타 프레임워크 포팅** — "Animata for Laravel(Blade)", "for Vue", "for Svelte"

### 4.7 로드맵

```
1~2개월차  한국어 포크 사이트 런칭 + 한글 자모 애니메이션 5개, 광고/스폰서 연결
3~4개월차  프리미엄 템플릿 3종 출시, 블로그 SEO 콘텐츠 10편 → 첫 수익
5~6개월차  외주 수주 시작, Pro 레지스트리 구독 베타 → 안정적 수익
7개월~     AI 생성 SaaS 또는 강의로 확장
```

### 4.8 준수 사항

- MIT 라이선스 원문 및 저작권 고지 유지
- 원작자(https://github.com/codse/animata) 크레딧 명시
- "Animata" 상표 그대로 사용하지 말고 새 브랜드명 사용
- 내가 추가한 가치(번역·현지화 컴포넌트·템플릿·서비스)를 판매할 것
- 원본을 그대로 복사해 유료화하는 것은 기술적으로 합법이나 커뮤니티 평판에 치명적

핵심 원칙: **코드는 무료로, 시간·편의·완성도를 판매한다.**

---

## 5. 참고 링크

- 현재 저장소: https://github.com/bmshin94/animata
- 원본 저장소: https://github.com/codse/animata
- 원본 기여자: https://github.com/codse/animata/graphs/contributors
- 라이선스: https://github.com/codse/animata/blob/main/LICENSE.md
- 이슈: https://github.com/codse/animata/issues
- 공식 사이트: https://animata.design
- Discord: https://discord.gg/STYEh3UW
