# QuartzBlog — 프로젝트 헌장

> Claude Code 세션 시작 시 이 문서를 먼저 읽어 프로젝트의 목적과 제약을 파악할 것.
> ⚠️ **이 노트는 블로그 발행 대상이 아님** — frontmatter에 `publish` 키를 두지 않거나 `publish: false`로 관리한다.

---

## 현재 단계 요약

| 항목 | 내용 |
|------|------|
| **완료 Phase** | (없음) |
| **진행 중** | Phase 1 — 로컬 Quartz 빌드 확인 |
| **상세 할 일** | [TODO](TODO.md) 참조 |

---

## 왜 만드는가

### 현재 문제
- Obsidian 볼트에 쌓인 학습 노트·기술 정리가 **개인 디바이스 안에만 갇혀** 있어 외부에서 열람·공유가 불가능하다.
- 블로그를 따로 운영하려면 노트를 다시 마크다운으로 옮겨 적는 **이중 작성**이 필요하다.
- 노트의 wikilink·백링크 구조(지식 그래프)가 일반 블로그 플랫폼에서는 소실된다.

### 해결 목표
- 볼트에서 `publish: true`인 노트만 골라 **별도 작성 없이** 정적 블로그로 자동 발행한다.
- wikilink·백링크·검색 등 Obsidian의 지식 그래프 경험을 웹에서도 유지한다.
- push 한 번으로 빌드·배포까지 자동화하여 운영 부담을 최소화한다.

---

## 누가 쓰는가

| 항목 | 내용 |
|------|------|
| **사용자** | 개인 블로그 운영자 1인 (작성자 = 독자 = 운영자) |
| **사용 환경** | 작성: Obsidian (Windows 데스크탑 + macOS 맥북). 열람: 웹 브라우저 (외부 공개) |
| **사용 빈도** | 작성은 수시, 발행은 노트 push 시점마다 자동 |

---

## 핵심 제약

| 제약 | 내용 | 근거 |
|------|------|------|
| 멀티 PC | Windows(사무실) + macOS(맥북) 양쪽에서 Quartz repo 작업 가능해야 함 | 사용자 환경 — 하드코딩 경로 금지, Node.js 양쪽 설치 |
| 콘텐츠 소스 분리 | 콘텐츠는 `woonmo/obsidian-vault`(private), 빌드 설정은 별도 repo | 볼트는 자동커밋 운영 중 — 빌드 설정과 섞으면 충돌 |
| private repo 접근 | Quartz Actions가 obsidian-vault를 읽으려면 인증 필요 (deploy key 또는 PAT) | private repo이므로 익명 checkout 불가 |
| 발행 필터 | `publish: true` 노트만 발행 — 기획·업무·개인 노트는 절대 새지 않아야 함 | 볼트엔 회사·개인 민감 노트가 다수 존재 |
| 이관 친화 | baseUrl·도메인 변경만으로 GitHub Pages → Cloudflare Pages 전환 가능 | 추후 커스텀 도메인 이관 예정 |

---

## 요구사항 요약

### 기능 요구사항

| ID | 요구사항 | 필요 이유 | 상태 | FEATURE_SPEC |
|----|---------|----------|:----:|:---:|
| REQ-01 | `publish: true` frontmatter 노트만 필터링 발행 | 민감 노트 유출 방지 | ⬜ | [FEATURE_SPEC §F-01](FEATURE_SPEC.md) |
| REQ-02 | Quartz v4로 정적 사이트 빌드 | 지식 그래프 경험 유지 | ⬜ | [FEATURE_SPEC §F-02](FEATURE_SPEC.md) |
| REQ-03 | GitHub Actions로 woonmo.github.io 자동 배포 | 운영 부담 최소화 | ⬜ | [FEATURE_SPEC §F-03](FEATURE_SPEC.md) |
| REQ-04 | wikilink·백링크·검색 지원 | 노트 간 탐색 경험 | ⬜ | [FEATURE_SPEC §F-04](FEATURE_SPEC.md) |
| REQ-05 | 커스텀 도메인(Cloudflare) 이관 친화 구조 | 추후 도메인 이관 | ⬜ | [FEATURE_SPEC §F-05](FEATURE_SPEC.md) |

### 비기능 요구사항

| ID | 항목 | 기준 | 근거 |
|----|------|------|------|
| NFR-01 | 빌드 시간 | 노트 수백 개 기준 Actions 1회 빌드 5분 이내 | 무료 Actions 분 절약 |
| NFR-02 | 멀티 PC 재현성 | Windows/macOS 양쪽에서 동일 명령으로 로컬 빌드 성공 | 사용자 환경 |
| NFR-03 | 시크릿 보안 | private repo 접근 토큰은 GitHub Secrets로만 관리, repo에 평문 노출 금지 | 민감 정보 보호 |

---

## 성공 기준

| 기준 | 측정 방법 | 달성 |
|------|----------|:----:|
| 로컬에서 테스트 노트가 빌드된다 | `npx quartz build` 성공 + 로컬 프리뷰에서 노트 렌더 확인 | ⬜ |
| publish:true 노트만 발행된다 | 발행 사이트에 publish:false/무키 노트가 없음을 확인 | ⬜ |
| push 시 자동 배포된다 | obsidian-vault 또는 Quartz repo push → Actions 녹색 → woonmo.github.io 갱신 | ⬜ |
| wikilink·백링크·검색이 동작한다 | 발행 사이트에서 링크 클릭·백링크 패널·검색창 정상 동작 | ⬜ |
| 도메인 변경이 1개 설정으로 가능하다 | baseUrl 한 줄 변경 후 재배포로 새 도메인 반영 | ⬜ |

---

## 범위 외 (현재 버전)

- 댓글·반응 등 동적 기능 (Giscus 등은 Phase 3 이후 후보)
- CMS·관리자 페이지 (작성은 Obsidian에서만)
- 다국어 i18n
- RSS·뉴스레터 구독 (Quartz 기본 RSS는 켤 수 있으나 별도 정비는 범위 외)
- 콘텐츠 자체 작성 (헌장 범위는 발행 파이프라인 구축까지)

---

## 기술 스택 요약

| 항목 | 선택 | 근거 |
|------|------|------|
| 정적 사이트 생성기 | Quartz v4 | Obsidian wikilink·백링크·그래프 네이티브 지원, 마크다운 그대로 발행 |
| 빌드 런타임 | Node.js (LTS) | Quartz v4 요구사항 |
| CI/CD | GitHub Actions | repo와 동일 생태계, 무료 |
| 호스팅 (초기) | GitHub Pages | 무료, Actions deploy-pages 네이티브 |
| 호스팅 (이관 목표) | Cloudflare Pages + 커스텀 도메인 | CDN·도메인 친화, baseUrl만 변경 |
| 콘텐츠 소스 | woonmo/obsidian-vault (private) | 기존 볼트 자동커밋 운영 중 |

---

## Phase 진행 이력 및 계획

| Phase | 범위 | 상태 | 완료일 |
|-------|------|------|--------|
| **1** | 로컬 빌드 확인 — Quartz repo 생성·설치·config 작성·publish 필터·테스트 노트 로컬 빌드 | 🟡 진행 중 | |
| **2** | 자동 배포 파이프라인 — obsidian-vault 연동(인증) → Quartz 빌드 → GitHub Pages 자동 배포 | ⬜ 예정 | |
| **3** | 콘텐츠 정비 + 커스텀 도메인 이관 준비 — 테마·메타·SEO 정비, Cloudflare Pages 이관 사전 작업 | ⬜ 예정 | |

---

## Phase 범위 변경 규칙

| 변경 유형 | 기록 위치 |
|---------|---------|
| 작업 항목 이동·이연 (소규모 조정) | 헌장 Phase 테이블 인라인 메모 |
| 기술 선택·아키텍처 결정 (중요 설계) | ADR 파일 (`ADR/ADR-NNN-제목.md`) |
| Phase 순서 변경 (우선순위 재조정) | 헌장 Phase 테이블 직접 수정 + updated 날짜 갱신 |

---

## 미결 사항

> ◐ 부분 축(셀프 게이트)·향후 결정 필요 항목. 해소 시 본문 반영 후 줄 삭제.

- [ ] **[ADR 후보] private repo 접근 방식 — deploy key vs PAT** — obsidian-vault를 Actions에서 읽는 인증 수단. read-only deploy key가 권한 최소화 측면 유리하나 PAT가 설정 단순. Phase 2 착수 시 결정. (carry 0회)
- [ ] **[ADR 후보] 콘텐츠 트리거 방식** — obsidian-vault push 시 Quartz repo 빌드를 어떻게 트리거할지 (repository_dispatch vs 스케줄 vs submodule + Quartz repo push). Phase 2 착수 시 결정. (carry 0회)
- [ ] **[Phase 3 후보] 댓글 기능(Giscus 등) 도입 여부** — 범위 외이나 콘텐츠 정비 단계에서 재검토.

---

## 관련 문서

| 문서 | 설명 |
|------|------|
| QuartzBlog MOC(2026-07-28 폐지) | 프로젝트 허브 |
| [FEATURE_SPEC](FEATURE_SPEC.md) | 기능 명세서 |
| [TODO](TODO.md) | 개발 진행 현황 |
