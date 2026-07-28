# QuartzBlog — 기능 명세서 (FEATURE_SPEC)

> 헌장의 요구사항(REQ-NN)을 기능 단위(F-NN)로 구체화한다.
> ⚠️ 이 노트는 블로그 발행 대상이 아니다 (`publish` 키 없음).

---

## 시스템 개요

```
[Obsidian 볼트]
  └─ publish: true 노트 작성
        │  git 자동커밋 push
        ▼
[woonmo/obsidian-vault (private)]   ← 콘텐츠 SoT
        │  (Phase 2: Actions 트리거 → checkout with 인증)
        ▼
[woonmo/woonmo.github.io (Quartz)]  ← 빌드 설정 SoT
  └─ quartz.config.ts / .github/workflows/deploy.yml
        │  npx quartz build  (ExplicitPublish 필터)
        ▼
[정적 HTML]  → GitHub Pages 배포  → (추후) Cloudflare Pages
```

- **콘텐츠 SoT**: obsidian-vault (마크다운 노트)
- **빌드 설정 SoT**: Quartz repo (config·워크플로·테마)
- 두 repo를 분리하는 이유: 볼트 자동커밋과 빌드 설정 커밋이 섞이면 추적·롤백이 어려움

---

## F-01. 발행 필터링 (REQ-01)

볼트의 노트 중 frontmatter `publish: true`인 것만 발행한다.

| 항목 | 내용 |
|------|------|
| 동작 | Quartz `quartz.config.ts`의 `filters` 배열에 `Plugin.ExplicitPublish()` 등록 |
| 입력 | 볼트 마크다운 노트 (frontmatter `publish` 키) |
| 발행 대상 | `publish: true`인 노트만 |
| 제외 | `publish: false`, `publish` 키 없음, 비공개 폴더(`_private` 등) |

**필터링 규칙**
- `publish: true` → 발행
- 그 외 모두 → 비발행 (안전한 기본값 = 발행 안 함)
- 발행 노트가 비발행 노트를 wikilink로 참조하면 → 깨진 링크가 아닌 플레인 텍스트로 처리(링크 미생성)되는지 확인 필요 (엣지 케이스 E-2)

**검증 (verify)**: 테스트 노트 3종(publish:true / publish:false / 무키)을 빌드 → 결과물에 true 노트만 존재.

---

## F-02. Quartz 빌드 (REQ-02)

Quartz v4로 마크다운을 정적 HTML로 변환한다.

| 항목 | 내용 |
|------|------|
| 빌드 명령 | `npx quartz build` (로컬 프리뷰: `npx quartz build --serve`) |
| 콘텐츠 위치 | Quartz repo의 `content/` 디렉토리 (Phase 1=로컬 복사 테스트, Phase 2=Actions에서 볼트 checkout) |
| 출력 | `public/` 디렉토리 (정적 HTML/CSS/JS) |
| 런타임 | Node.js LTS (멀티 PC 동일 버전 `.nvmrc` 또는 문서 명시) |

**검증 (verify)**: Windows·macOS 양쪽에서 `npx quartz build` 종료코드 0 + `public/index.html` 생성.

---

## F-03. 자동 배포 (REQ-03)

push 시 GitHub Actions가 빌드 → GitHub Pages 배포까지 자동 수행한다.

| 항목 | 내용 |
|------|------|
| 워크플로 | `.github/workflows/deploy.yml` |
| 단계 | checkout → setup-node → (볼트 콘텐츠 가져오기) → `npx quartz build` → `actions/upload-pages-artifact` → `actions/deploy-pages` |
| 트리거 | Quartz repo push (Phase 1 기본) + obsidian-vault 변경 연동(Phase 2 — 미결: dispatch/submodule/스케줄 중 결정) |
| 콘텐츠 인증 | private obsidian-vault checkout 시 deploy key 또는 PAT (미결 — ADR 후보) |
| 권한 | `permissions: pages: write, id-token: write` |

**검증 (verify)**: push → Actions 녹색 → woonmo.github.io 접속 시 최신 노트 반영.

**엣지 케이스**
- E-1: 시크릿 누락/만료 → checkout 실패 → 워크플로 빨강. 실패 시 기존 배포본 유지(Pages는 마지막 성공 산출물 서빙).
- E-3: 빌드 에러(깨진 마크다운) → 배포 단계 진입 안 함 → 이전 사이트 유지.

---

## F-04. 탐색 기능 — wikilink·백링크·검색 (REQ-04)

| 기능 | Quartz 플러그인/설정 | 비고 |
|------|---------------------|------|
| wikilink | `Plugin.CrazyLinks` / 기본 마크다운 변환 | `[[노트]]` → 링크 변환 |
| 백링크 | `Component.Backlinks()` (기본 레이아웃 포함) | 페이지 하단 표시 |
| 검색 | `Component.Search()` (기본 포함) | 클라이언트 사이드 풀텍스트 |
| 그래프 뷰(선택) | `Component.Graph()` | 기본 제공, 켜기/끄기 선택 |

**검증 (verify)**: 발행 사이트에서 링크 클릭 이동·백링크 패널 표시·검색창 키워드 매칭 동작.

---

## F-05. 이관 친화 구조 (REQ-05)

GitHub Pages → Cloudflare Pages 전환을 설정 최소 변경으로 가능하게 한다.

| 항목 | 내용 |
|------|------|
| baseUrl 단일화 | `quartz.config.ts`의 `baseUrl` 한 곳에서만 도메인 관리 |
| 하드코딩 금지 | 절대경로 링크·도메인 문자열을 코드/노트에 박지 않음 |
| 빌드 산출물 호환 | `public/` 정적 산출물은 Pages·Cloudflare 양쪽 동일하게 서빙 가능 |
| 이관 시 변경점 | baseUrl 값 + 도메인 DNS/Pages 프로젝트 연결만 (Quartz 빌드 자체는 불변) |

**검증 (verify)**: baseUrl을 새 도메인으로 바꿔 재빌드 시 내부 링크가 새 도메인 기준으로 생성됨.

---

## 권한·접근

| 대상 | 접근 |
|------|------|
| 발행 사이트 | 공개 (외부 누구나 열람) — 단 publish:true 노트만 |
| obsidian-vault | private — Actions 토큰만 read 접근 |
| Quartz repo | public 가능(빌드 설정만) 또는 private. 시크릿은 Secrets에만 |

---

## 테스트 전략

| 구분 | 케이스 |
|------|--------|
| Happy path | publish:true 노트 빌드·발행 / push → 자동배포 녹색 |
| Edge case | publish 무키·false 노트 미발행 / 발행→비발행 wikilink 처리 / 시크릿 만료 시 기존 배포 유지 / 빌드 에러 시 이전 사이트 유지 |
| 멀티 PC | Windows·macOS 양쪽 로컬 빌드 동일 결과 |
| 제외 | Quartz 내부 렌더링 로직(프레임워크 신뢰), 단순 CSS 스타일 |

---

## 관련 문서

| 문서 | 설명 |
|------|------|
| [CHARTER](CHARTER.md) | 프로젝트 헌장 |
| [TODO](TODO.md) | 진행 현황 |
| QuartzBlog MOC(2026-07-28 폐지) | 프로젝트 허브 |
