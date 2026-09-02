# KLIC 유지보수 스킬 팩

공공기관·기업 유지보수 조직을 위한 Claude Code / Codex / Gemini 스킬 모음.
**KLIC/GIDO 철학**(최소수정·근본원인·증거기반·계층별 추적) 기반.
eGovFrame · Spring · MyBatis · JSP 장애 흐름에 최적화.

> 외부에서 다운로드한 스킬을 설치 전 검수하려면 `klic-skill-security` 사용.

---

## 스킬 목록

| 스킬 | 용도 | 주요 트리거 |
|------|------|-------------|
| **klic-spec-intake** | 오류/개선 요청 접수 → EARS 요구사항 → Acceptance → Task 변환 | 요청·이슈 접수, 요구사항 정리 |
| **klic-effort-router** | 작업 티어(S/M/L/XL) 판정 → 단계별(계획/검토/구현/리뷰) 모델·에포트 배정, 정의된 서브에이전트 파일로만 라우팅 | 작업 착수 전 티어 판정, 서브에이전트 위임, 보안 감사 준비 |
| **klic-bugfix-triage** | 오류 질문·버그 재현·원인분석·최소수정 범위 도출 | 버그 리포트, 장애 재현, 원인 추적 |
| **klic-refactor-scope** | 리팩터링 범위 통제·과추상화 방지 | 공통화, MyBatis/JSP 대량 변경 범위 판정 |
| **klic-maintenance-quality** | 스펙·코드품질·공공데이터 표준·검증 게이트 종합 | 유지보수 품질 게이트 |
| **klic-pr-review-gate** | PR·diff 계약·품질·보안 리뤴 게이트 | PR 리뷰, diff 검토 (XML Mapper·Spring 설정·JSP) |
| **klic-incident-postfix** | 장애 수정 후 증거·재발방지·학습 정리 | 장애 사후 정리, 재발방지 |
| **klic-public-data-naming** | 공공데이터 표준 용어 기반 네이밍 검토 | 테이블·컬럼·API·DTO·변수·JSP 라벨 |
| **klic-skill-security** | 외부 AI 스킬 설치 전 보안 검수 | 신뢰할 수 없는 출처 스킬 검증 |

### 유지보수 라이프사이클 매핑

```
접수        → klic-spec-intake
작업배분    → klic-effort-router (티어 판정·모델/에포트 배정)
분석        → klic-bugfix-triage
범위통제    → klic-refactor-scope
품질검증    → klic-maintenance-quality
병합검토    → klic-pr-review-gate
사후정리    → klic-incident-postfix
(공통)      → klic-public-data-naming (네이밍)
            → klic-skill-security (외부 스킬 검수)
```

> 티어 구분: klic-spec-intake의 MVP/Core/Product/RFP는 **프로젝트 티어**(스펙 범위),
> klic-effort-router의 S/M/L/XL은 **작업 티어**(실행 자원)다. 혼용하지 않는다.

---

## 설치

### Claude Code

```bash
# 스킬 디렉터리로 클론
git clone https://github.com/klic-co-kr/KLIC-eGovFrame-Skills.git
# 각 스킬을 개인 스킬 디렉터리에 복사
cp -r KLIC-eGovFrame-Skills/skills/klic-* ~/.claude/skills/

# klic-effort-router 서브에이전트 파일은 agents 디렉터리에 복사 (라우팅 대상)
mkdir -p ~/.claude/agents
cp KLIC-eGovFrame-Skills/skills/klic-effort-router/agents/*.md ~/.claude/agents/
```

또는 심볼릭 링크로 연결(업데이트 편의):

```bash
for d in KLIC-eGovFrame-Skills/skills/klic-*; do
  ln -s "$(pwd)/$d" ~/.claude/skills/"$(basename "$d")"
done

# klic-effort-router 서브에이전트는 심볼릭 링크 불가 — 복사 필수
mkdir -p ~/.claude/agents
cp KLIC-eGovFrame-Skills/skills/klic-effort-router/agents/*.md ~/.claude/agents/
```

업데이트 시: `git pull` 후 스킬 갱신에 더해 **에이전트 파일도 반드시 재복사**한다.
`~/.claude/agents/`는 링크가 아니므로 재복사 없으면 구판이 남아 라우팅이 깨진다.

제거(언인스톨):

```bash
rm ~/.claude/skills/klic-effort-router 2>/dev/null
rm -f ~/.claude/agents/klic-{plan-high,plan-xhigh,plan-adversary-xhigh,implement-med,implement-xhigh,core-xhigh,review-pr-high,review-pr-xhigh,security-audit,verify-max,coder-medium}.md
```

### Codex CLI / Gemini CLI

`~/.agents/skills/` 를 스킬 디렉터리로 사용:

```bash
cp -r KLIC-eGovFrame-Skills/skills/klic-* ~/.agents/skills/
```

> 각 스킬은 `SKILL.md` 하나로 자기완결. 별도 의존성 없음.
> **예외 — klic-effort-router**: Claude Code 전용. 서브에이전트 파일(`agents/`)과 effort frontmatter에 의존하므로 Codex/Gemini에서는 티어 판정·Output Contract만 유효하고 라우팅은 동작하지 않는다.

---

## 구조

```
.
├── README.md
├── skills/
│   ├── klic-bugfix-triage/SKILL.md
│   ├── klic-incident-postfix/SKILL.md
│   ├── klic-effort-router/
│   │   ├── SKILL.md
│   │   ├── agents/               # 서브에이전트 정의 11종 (~/.claude/agents/로 복사)
│   │   └── TESTS.md              # 검증 프로토콜·결과
│   ├── klic-maintenance-quality/SKILL.md
│   ├── klic-pr-review-gate/SKILL.md
│   ├── klic-public-data-naming/SKILL.md
│   ├── klic-refactor-scope/SKILL.md
│   ├── klic-skill-security/SKILL.md
│   └── klic-spec-intake/SKILL.md
└── backup/                      # 원본 .skill 팩 (gitignored)
```

---

## KLIC/GIDO 철학 요약

- **최소수정**: 증상 땜질 금지. 근본 원인만 건드린다.
- **근본원인**: `git diff/log`로 어떤 변경이 에러를 유발했는지 추적.
- **증거기반**: 수정 전 재현, 수정 후 검증. 출력은 사실 기반.
- **계층별 추적**: Controller→Service→Mapper→JSP 흐름 따라 원인 위치 특정.
- **과추상화 방지**: 리팩터링 범위 통제, 공통화 역효과 경계.

---

## 라이선스

MIT
