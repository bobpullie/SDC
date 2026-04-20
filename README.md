# SDC — Subagent Delegation Contract

**Claude Code Skill Plugin** — Opus 4.7 본체가 Sonnet 서브에이전트에 실행 작업을 위임할 때 교환하는 **계약**. Design by Contract(Hoare logic) 구조적 동형.

## 왜 SDC가 필요한가

"맥락 추측하지 마라, 계약서에 명시된 것만 수행하라" — 서브에이전트 실패 모드의 근본 원인은 **모호한 프롬프트**. SDC는 이를 5-slot 구조로 강제한다.

## 5항목 Contract Slots

| 슬롯 | Contract 대응 | 역할 |
|------|-------------|------|
| 1. 목적 (WHAT) | Specification intent | 배경·이유 1~2문장 |
| 2. 필수 게이트 | Precondition | 도메인 규칙 원문 발췌 (요약 금지) |
| 3. 제약 | Invariant | 건드리지 말 것 / 바꾸지 말 것 |
| 4. 완료 기준 | Postcondition | 관측 가능한 조건 |
| 5. 보고 형식 | Output interface | 결과 요약 포맷 |

## 분업 매트릭스 (Opus/Sonnet)

- **본체 (Opus 4.7):** 아키텍처 설계 · TEMS 규칙 분류 · Phase 전환 판정 · 핸드오버 서술 · 팀 델리게이션 · trivial edit · 긴급 의사결정
- **Sonnet 서브에이전트:** 코드 구현·패치 · Phase 이식 · 규칙 재분류 · DVC case · smoke test · 광범위 탐색 · 독립 병렬 과제
- **Opus 서브에이전트 (편향 차단):** `superpowers:code-reviewer` / `advisor`. 혼용 금지.

## Install (프로젝트 로컬 스킬)

**Option A — 단일 파일 download (경량):**

```bash
cd <AGENT_PROJECT_ROOT>
mkdir -p .claude/skills
curl -o .claude/skills/SDC.md https://raw.githubusercontent.com/bobpullie/SDC/main/SKILL.md
```

**Option B — 서브모듈/clone (pull-based 업데이트):**

```bash
cd <AGENT_PROJECT_ROOT>
git clone https://github.com/bobpullie/SDC.git .claude/skills/SDC
# SKILL.md 경로: .claude/skills/SDC/SKILL.md
```

## Updating

```bash
# Option A (single file)
curl -o <AGENT_PROJECT_ROOT>/.claude/skills/SDC.md https://raw.githubusercontent.com/bobpullie/SDC/main/SKILL.md

# Option B (clone)
git -C <AGENT_PROJECT_ROOT>/.claude/skills/SDC pull origin main
```

위상군/리얼군/타 에이전트 누구든 upstream에 기여 → 전체 에이전트가 업데이트 수신.

## 에이전트별 커스터마이제이션

이 레포의 `SKILL.md` 는 **위상군(TEMS/아키텍처 도메인) 기준의 canonical template**. 각 에이전트는 Template 1~5의 도메인을 자기 영역으로 교체하여 사용:

| 에이전트 | Template 도메인 예 |
|---------|-----------------|
| 위상군 | TEMS 구현 / Phase 이식 / 규칙 재분류 / 탐색 / Audit |
| 리얼군 | Blueprint / C++ / Asset·Material / 네트워크 분석 / Audit |
| 코드군 | Quant 코드 / 데이터 파이프라인 / 백테스트 / 탐색 / Audit |

유지할 공통 자산:
- 필수 5항목 구조
- 분업 매트릭스
- 호출 방식 (`Agent(subagent_type=..., model="sonnet", prompt="...")`)
- 결과 검증 (trust-but-verify)

## 호출 방식

```python
Agent(
  description="간결한 3~5단어 설명",
  subagent_type="general-purpose",  # 또는 "Explore" / "superpowers:code-reviewer" / "advisor"
  model="sonnet",
  prompt="(SDC 5항목 채운 마크다운)"
)
```

## 결과 검증 (trust-but-verify)

서브에이전트 보고의 사실 주장(경로·파일 존재·테스트 통과)은 bash/grep으로 **반드시** 실증. 위임이 효율적일수록 검증 책임이 증대한다.

## Related Plugins

- [TEMS](https://github.com/bobpullie/TEMS) — LLM 기억 시스템 (hook)
- [TWK](https://github.com/bobpullie/TWK) — LLM Wiki 3-Layer (skill)
- [DVC](https://github.com/bobpullie/DVC) — 결정론적 빌드 검증 (skill)

## Naming History

- v0: `subagent-brief` (2026-04-20 S34 도입)
- v1: **SDC** (2026-04-20 S35 rename) — Anthropic 충돌 회피 · TEMS/DVC/TWK 3-letter 리듬 · Contract = Hoare logic 수학적 정합성

## License

MIT — see [LICENSE](LICENSE).
