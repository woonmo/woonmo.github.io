# 블로그 · 가든 투트랙 운영 전략 + 가이드

> lazycorn.net 콘텐츠 생태계의 운영 방향과 "무엇을 어디에 올릴지" 판단 기준 + 발행 가이드.
> ⚠️ 발행 대상 아님(프로젝트 운영 문서). 인프라 상세는 `~/.claude/memory/reference_infra.md`·`~/.claude/memory/reference_infra.md`, 도메인 메모는 dotfiles `reference-domain-lazycorn.md`.
>
> 📌 이 문서는 [CHARTER](CHARTER.md)의 **단일 Quartz 계획을 대체**한다 (아래 "원 계획과의 관계" 참조).

---

## 한 줄 요약

**완성된 글 = 블로그(Ghost) @ `lazycorn.net` / 자라는 노트망 = 가든(Quartz) @ `garden.lazycorn.net`.** 원본은 Obsidian 볼트(SoT), 두 곳은 발행 대상.

---

## 두 트랙 비교

| | **블로그 (Ghost)** | **가든 (Quartz)** |
|---|---|---|
| **URL** | `lazycorn.net` (apex) | `garden.lazycorn.net` |
| **호스팅** | 서울 OCI Docker (Ghost+MySQL), CF Tunnel 경유 | GitHub Pages (`woonmo/woonmo.github.io`) |
| **성격** | 스트림 — **완성된 글**, 시간순 발행 | 스톡 — **계속 자라는 노트망** |
| **구조** | 글 단위·선형·발행 의식 있음 | 위키링크·백링크·그래프뷰·검색 |
| **대상** | 외부 독자 (SEO·구독·AdSense) | 정리·학습·연결 (나 + 탐색자) |
| **태도** | 다듬어서 "내놓는" 결과물 | 미완성도 OK, 자라나는 "정원" |
| **기능** | 댓글(Isso, 익명·스레드)·뉴스레터·(추후 AdSense) | 그래프·백링크·검색 (Quartz 기본) |
| **디자인** | Claude톤 CSS (Code injection) | Quartz 테마 (제목·로고 정비 예정) |

---

## 왜 나눴나 (스트림 vs 스톡)

- 블로그는 "이건 완성했다" 싶은 걸 발행하는 **결과물 공간**, 가든은 옵시디언 노트를 거의 그대로 엮어 올리는 **과정 공간**. 성격이 달라 한 플랫폼에 욱여넣기보다 분리.
- 한쪽(블로그)은 **방문자에게 보여주는 면**, 다른쪽(가든)은 **지식이 연결되며 쌓이는 면**. 둘을 섞으면 둘 다 어정쩡해진다.
- **락인 회피**: 원본은 Obsidian 마크다운(SoT). 가든은 거의 그대로 미러, 블로그는 다듬어 발행 → 플랫폼 갈아타도 **재발행만** 하면 됨.

---

## 콘텐츠 흐름

```
Obsidian 볼트 (원본 = SoT)
   ├─→ 가든(Quartz): 발행 표시 노트 → GitHub Actions 빌드 → garden.lazycorn.net
   │      (노트끼리 위키링크로 연결되며 자람)
   └─→ 블로그(Ghost): 그중 잘 여문 주제를 "글"로 다듬어 → lazycorn.net 발행
```

- 토픽 축: **러닝 / 책 / 기술** (멀티토픽)

---

## 무엇을 어디에? (판단 기준)

| 이런 거면 | → 어디 |
|----------|--------|
| 하나의 완결된 주장·이야기·튜토리얼, 처음~끝이 있는 글 | **블로그** |
| 외부 독자에게 읽히고 싶다 / SEO·공유 의식 | **블로그** |
| 단편 노트·메모·용어 정리·링크 모음, 계속 덧붙일 것 | **가든** |
| 다른 노트와 연결(백링크)이 핵심인 지식 조각 | **가든** |
| 아직 미완성이지만 공개해도 되는 것 | **가든** |

> 애매하면: **가든에 먼저** 쌓고, 여러 노트가 한 주제로 여물면 그때 **블로그 글**로 묶어 발행 → 글 하단에서 가든 노트로 링크.

---

## 운영 가이드

### 가든 (Quartz)
- Obsidian 볼트에서 발행 대상 노트 작성 → 볼트 push → GitHub Actions가 `woonmo/woonmo.github.io` 빌드·배포 → `garden.lazycorn.net` 반영.
- baseUrl·테마·플러그인 설정 = repo `quartz.config.default.yaml`. (로컬 클론: `~/Documents/01_Projects/Garden`)
- 빌드 파이프라인·발행 필터 상세는 [CHARTER](CHARTER.md) + repo 참조.
- **남은 정비**: 사이트 제목(현재 `lab404`) · 로고.

### 블로그 (Ghost)
- 관리자 `https://lazycorn.net/ghost/` 에서 글 작성·발행 (오너 `wonmo151@naver.com`).
- 원본 초안은 Obsidian `99_Publishing/_drafts/`에 두고 다듬어 Ghost로 옮김.
- 디자인 = Settings → Code injection (Claude톤 CSS). 헤더·바디 cream 통일됨.
- 댓글 = Isso (footer 임베드, 익명 허용·스레드 답글·크림/클레이 테마). 트랜잭션/구독 메일 = Brevo SMTP.
- 메일 송수신 구성 상세 = `~/.claude/memory/reference_infra.md` "Ghost 메일" 섹션.

---

## 상호링크 전략 (예정, 미구현)

- 블로그 글 하단: **"관련 노트 →"** 로 가든의 해당 주제 노트 링크
- 가든 노트: **"정리된 글 →"** 로 블로그 완성글 링크
- 효과: 방문자가 "완성글 ↔ 깊은 노트" 양방향 탐색. 블로그=입구, 가든=심화.

---

## 현재 라이브 구성 (2026-06-14)

| 항목 | 값 |
|------|------|
| 블로그 | Ghost @ `lazycorn.net` (apex). 서울 OCI Docker, CF Tunnel→NPM→`100.87.107.120:2368` |
| 가든 | Quartz v5 @ `garden.lazycorn.net`. GitHub Pages(`woonmo/woonmo.github.io`), CF DNS-only CNAME, HTTPS enforce |
| 댓글 | Isso @ `comments.lazycorn.net` (서울). 익명·스레드 답글, 크림/클레이 테마(Ghost code injection) |
| 메일 | 발신=Brevo SMTP / 수신=CF Email Routing(`no-reply@`·`support@`→Gmail) |
| 인프라 SoT | `~/.claude/memory/reference_infra.md` · `~/.claude/memory/reference_infra.md` |

---

## 브랜딩 (2026-06-14 확정)

- **이름/제목**: `lazycorn` (소문자·붙여쓰기 — "귀찮아서" 컨셉)
- **로고**: 껍질 1/3만 깐 채 INTP스럽게 시큰둥한(한쪽 눈썹↑·side-eye) 옥수수 마스코트. 팔레트=무광 세이지+골드+클레이.
- **자산 파일**: `QuartzBlog/assets/` — `lazycorn-logo.svg/.png`(투명, 헤더용) · `lazycorn-icon.svg/.png`(크림 배지, 파비콘용 512²)
- **적용**: 블로그 Ghost(Publication logo=투명 / icon=배지, 라이브✅) · 가든 Quartz(pageTitle=lazycorn + 파비콘=배지, 라이브✅). 가든 홈 `index.md` 제목도 lazycorn으로 수정(다음 리빌드 반영).

## 로드맵 / 남은 일

- [x] ~~가든 제목 + 로고~~ → 2026-06-14 완료 (위 브랜딩)
- [ ] 블로그↔가든 상호링크
- [ ] 콘텐츠 축적 (러닝/책/기술)
- [ ] AdSense (글 충분히 쌓인 뒤 — `ads.txt` + 쿠키 동의 배너)

---

## 원 계획과의 관계

[CHARTER](CHARTER.md)(2026-06-04)는 **단일 트랙** 계획이었다 — "Obsidian publish 노트 → Quartz v4 → GitHub Pages → (Cloudflare Pages 이관)". 이후 실제로는:
- **Ghost 블로그를 추가**해 **투트랙**으로 진화 (완성글=Ghost / 노트망=Quartz 가든)
- Quartz **v5**, **GitHub Pages 유지**(Cloudflare Pages 이관 안 함), **커스텀 도메인 `garden.lazycorn.net` 완료**

→ 운영 전략의 최신 기준은 **본 문서**. CHARTER는 가든(Quartz) 빌드 파이프라인의 초기 설계 기록으로 참고.
