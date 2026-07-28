# project-docs

lazycorn 가든(`garden.lazycorn.net`) 운영 주체의 **프로젝트 문서**. Quartz 업스트림과 무관하다.

| 문서 | 내용 |
|------|------|
| [OPERATING_STRATEGY.md](OPERATING_STRATEGY.md) | 블로그(Ghost) · 가든(Quartz) 투트랙 운영 전략 + "무엇을 어디에 올릴지" 판단 기준. **CHARTER의 단일 Quartz 계획을 대체한다** |
| [CHARTER.md](CHARTER.md) | 프로젝트 헌장 (목적·요구사항 REQ-NN·Phase) |
| [FEATURE_SPEC.md](FEATURE_SPEC.md) | 기능 명세 (F-01~F-05) |
| [TODO.md](TODO.md) | Phase 단위 진행 현황 |
| `assets/` | 로고 원본. 사이트가 실제로 쓰는 것은 `quartz/static/icon.png`(= `lazycorn-logo.png`와 동일) |

## 왜 `docs/`가 아닌가

`docs/`는 **Quartz 업스트림 문서**다(`configuration.md`·`features/`·`getting-started/` 등). 여기에 프로젝트 문서를 섞으면 업스트림을 머지할 때 충돌한다. `content/`도 안 된다 — 배포 워크플로가 빌드 때마다 `rm -rf content` 후 볼트를 체크아웃하므로 내용이 사라진다.

## 배포 파이프라인 요약

```
Obsidian 볼트 (원본 SoT)
   └─ .github/workflows/deploy.yml
        content/ 비우기 → woonmo/obsidian-vault 체크아웃 → publish:true 필터 → 빌드 → Pages
```

main push + 매일 00:00 KST 크론으로 돈다. 즉 **가든에 글을 올리는 방법은 볼트 노트에 `publish: true`를 넣는 것**이고, 이 repo를 건드릴 일은 테마·설정 변경뿐이다.

## 이력

2026-07-28 Obsidian 볼트 `01_Projects/02_Personal/QuartzBlog/`에서 이관. 글로벌 ADR-001(2026-07-22 개정)로 볼트가 문서 저장 위치에서 제외된 데 따른 것이다. 위키링크는 상대 링크로 변환했고, 볼트 인프라 노트 참조는 `~/.claude/memory/reference_infra.md`로 재지정했다.
