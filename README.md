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
분석        → klic-bugfix-triage
범위통제    → klic-refactor-scope
품질검증    → klic-maintenance-quality
병합검토    → klic-pr-review-gate
사후정리    → klic-incident-postfix
(공통)      → klic-public-data-naming (네이밍)
            → klic-skill-security (외부 스킬 검수)
```

---

## 설치

### Claude Code

```bash
# 스킬 디렉터리로 클론
git clone https://github.com/klic-co-kr/klic-e-GovFrame-Skills.git
# 각 스킬을 개인 스킬 디렉터리에 복사
cp -r klic-e-GovFrame-Skills/skills/klic-* ~/.claude/skills/
```

또는 심볼릭 링크로 연결(업데이트 편의):

```bash
for d in klic-e-GovFrame-Skills/skills/klic-*; do
  ln -s "$(pwd)/$d" ~/.claude/skills/"$(basename "$d")"
done
```

### Codex CLI / Gemini CLI

`~/.agents/skills/` 를 스킬 디렉터리로 사용:

```bash
cp -r klic-e-GovFrame-Skills/skills/klic-* ~/.agents/skills/
```

> 각 스킬은 `SKILL.md` 하나로 자기완결. 별도 의존성 없음.

---

## 구조

```
.
├── README.md
├── skills/
│   ├── klic-bugfix-triage/SKILL.md
│   ├── klic-incident-postfix/SKILL.md
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
