# QuartzBlog — 진행 현황 (TODO)

> Phase 단위 작업 추적. 완료 시 `- [x]` 체크 + 헌장 Phase 테이블·MOC 동기 갱신.
> ⚠️ 이 노트는 블로그 발행 대상이 아니다 (`publish` 키 없음).

---

## Phase 1 — 로컬 빌드 확인 🟡

> 목표: 로컬에서 Quartz가 publish:true 노트를 빌드·프리뷰하는 것까지 확인.
> verify: Windows·macOS 양쪽에서 `npx quartz build` 종료코드 0 + 테스트 노트 렌더.

- [ ] Quartz repo 생성 (`woonmo/woonmo.github.io` 또는 `woonmo/quartz-blog` — 이름 결정 필요)
- [ ] Node.js LTS 양쪽 PC 설치 + 버전 고정(`.nvmrc` 또는 README 명시)
- [ ] `npx quartz create`로 Quartz v4 초기화
- [ ] `quartz.config.ts` 작성 — baseUrl, 사이트 타이틀, 한국어 설정
- [ ] `filters`에 `Plugin.ExplicitPublish()` 등록 (publish:true만 발행)
- [ ] 테스트 노트 3종 작성 (publish:true / publish:false / 무키) → `content/`에 배치
- [ ] `npx quartz build --serve` 로컬 프리뷰 → true 노트만 렌더 확인
- [ ] wikilink·백링크·검색 로컬 동작 확인 (F-04)
- [ ] macOS·Windows 양쪽 빌드 재현 확인 (NFR-02)

---

## Phase 2 — 자동 배포 파이프라인 ⬜

> 목표: obsidian-vault 콘텐츠를 Actions가 가져와 빌드·GitHub Pages 자동 배포.
> verify: push → Actions 녹색 → woonmo.github.io에서 최신 publish 노트 확인.

- [ ] **[ADR-001] private repo 접근 방식 결정** — deploy key vs PAT (미결, 헌장 미결사항)
  - verify: 결정 후 ADR 작성 → 본 항목 해소
- [ ] **[ADR-002] 콘텐츠 트리거 방식 결정** — repository_dispatch vs submodule vs 스케줄 (미결)
  - verify: 결정 후 ADR 작성 → 본 항목 해소. ⚠️ 본 항목 미결 시 deploy.yml 콘텐츠 가져오기 단계 확정 불가
- [ ] GitHub Secrets에 인증 토큰/키 등록 (평문 노출 금지, NFR-03)
- [ ] `.github/workflows/deploy.yml` 작성 — checkout → setup-node → 볼트 콘텐츠 가져오기 → build → deploy-pages
- [ ] `permissions: pages: write, id-token: write` + GitHub Pages 소스 = Actions 설정
- [ ] obsidian-vault 변경 → 자동 빌드 트리거 연결 (트리거 방식 결정 반영)
- [ ] 첫 자동 배포 성공 확인 (Actions 녹색 + 사이트 접속)
- [ ] 실패 케이스 확인 — 시크릿 만료/빌드 에러 시 기존 배포 유지 (E-1·E-3)

---

## Phase 3 — 콘텐츠 정비 + 도메인 이관 준비 ⬜

> 목표: 발행 품질 정비 + Cloudflare Pages 이관 사전 작업.
> verify: baseUrl 변경만으로 새 도메인 링크 생성 확인 (F-05).

- [ ] 테마·레이아웃 정비 (다크모드·폰트·한글 가독성)
- [ ] 메타데이터·SEO 기본 (title·description·OG 태그)
- [ ] baseUrl 단일 관리 검증 — 도메인 문자열 하드코딩 없음 확인
- [ ] Cloudflare Pages 이관 절차 문서화 (DNS·Pages 프로젝트 연결 단계)
- [ ] (선택) RSS·그래프뷰 on/off 결정
- [ ] (Phase 3 후보) 댓글 기능(Giscus 등) 도입 여부 재검토 — 헌장 미결사항

---

## Backlog (Phase 미배정)

- [ ] [Phase 3 후보] 댓글·반응 기능 (Giscus)
- [ ] 발행 노트 자동 인덱스/태그 페이지 정비

---

## 진행 경과

| 날짜 | 내용 |
|------|------|
| 2026-06-04 | 프로젝트 착수 — 헌장·FEATURE_SPEC·TODO·MOC 작성 |
