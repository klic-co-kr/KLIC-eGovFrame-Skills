---
name: klic-bugfix-triage
description: KLIC/GIDO 철학 기반 유지보수 조직용 오류 질문·버그 재현·원인분석·최소수정 범위 도출 스킬; eGovFrame/Spring/MyBatis/JSP 장애 흐름 추적
---

# KLIC Bugfix Triage

버그를 바로 고치는 대신 재현 조건, 기대 동작, 깨진 계약, 최소 수정 범위를 먼저 확정하는 조직 스킬이다.

```text
Decision → Requirement → Acceptance → Task → Evidence → Learning
```

## When to Use

- 버그 리포트가 들어왔을 때
- 재현 조건이 불명확할 때
- 원인 파일을 좁혀야 할 때
- regression test 기준이 필요할 때
- 임시 땜질과 근본 수정 중 선택해야 할 때

## eGovFrame Error Routing

오류 질문은 기술명보다 증상과 계층으로 라우팅한다.

- 화면 오류, 버튼 동작, 표시값 불일치 → JSP/validation message/Controller binding 확인
- 404/500, request parameter, session/auth 오류 → Controller/Spring config/auth boundary 확인
- 비즈니스 결과 불일치 → Service rule/transaction boundary 확인
- SQL 오류, 조회 결과 누락, 타입 매핑 오류 → MyBatis XML/sql id/resultMap/parameterMap 확인
- 배치 실패, 스케줄 누락 → batch job/config/log evidence 확인
- 용어·컬럼·라벨 불일치 → 공공데이터 표준 용어와 DB/API/UI contract 확인

## Core Rules

1. 재현 조건 없는 수정 제안은 하지 않는다.
2. 기대 동작을 requirement/acceptance로 환원한다.
3. 원인 후보와 반증 조건을 같이 적는다.
4. 최소 수정 범위를 먼저 제안한다.
5. 기존 동작 보존을 acceptance에 포함한다.
6. Evidence는 redacted output만 허용한다.

## Process

### 1. Symptom

```markdown
- Reported symptom:
- Affected user / role:
- Environment:
- Frequency: always | intermittent | unknown
```

### 2. Reproduction

```markdown
- Steps:
- Expected:
- Actual:
- Missing info:
```

### 3. Contract Mapping

```markdown
Requirement:
WHEN <상황>이면,
시스템은 <기대 동작>을 MUST <수행한다>.

Acceptance:
- AC-001: <재현 입력>에서 <기대 결과>를 확인한다.
- AC-002: 기존 <보존 동작>은 유지된다.
```

### 4. Root Cause Candidates

```markdown
| Candidate | Evidence | Counter-evidence | Confidence |
|-----------|----------|------------------|------------|
| | | | low/medium/high |
```

### 5. Minimal Fix

```markdown
- Files likely to touch:
- Files not to touch:
- Smallest safe change:
- Regression test:
```

## Output Contract

```markdown
## KLIC Bugfix Triage
- Verdict: READY_TO_FIX | NEEDS_REPRO | BLOCKED
- Symptom:
- Reproduction:
- Requirement:
- Acceptance:
- Root Cause Candidate:
- Minimal Fix Scope:
- Regression Evidence Needed:
- Do Not Touch:

## Work Log
- Decision:
- Reason:
- Files inspected:
- Contract checked:
- Risk found:
- Verification planned/run:
- Out-of-scope:
```

## Hard Stops

- 운영 DB/배포/권한 변경이 필요하면 중단한다.
- secret/PII/customer data 원문은 로그에 남기지 않는다.
- 재현 불가 상태에서 구현으로 점프하지 않는다.
