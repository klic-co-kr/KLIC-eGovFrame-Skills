---
name: klic-incident-postfix
description: KLIC/GIDO 철학 기반 유지보수 조직용 운영 오류·장애 수정 후 증거·재발방지·학습 정리 스킬; eGovFrame 계층별 원인 기록
---

# KLIC Incident Postfix

장애나 운영 이슈 수정 후 원인, 수정, evidence, 재발 방지 gate, learning을 정리하는 조직 스킬이다.

```text
Decision → Requirement → Acceptance → Task → Evidence → Learning
```

## When to Use

- 장애/운영 이슈 수정 후
- hotfix 이후 정리
- 재발 방지책 도출
- 테스트/모니터링 gap 확인
- incident learning을 다음 작업에 반영해야 할 때

## eGovFrame Incident Classification

장애 원인은 다음 계층 중 어디에서 발생했는지 분리해서 기록한다.

- Controller binding/session/auth
- Service business rule/transaction
- MyBatis SQL/resultMap/parameterMap
- DB data/schema/index/migration
- JSP form/label/rendering
- batch schedule/job parameter/retry
- Spring context/properties/profile/config drift
- 공통컴포넌트 contract 변경

## Rules

1. 책임 추궁이 아니라 시스템 학습을 목적으로 한다.
2. 원인과 추측을 분리한다.
3. Evidence는 redacted excerpt만 사용한다.
4. 재발 방지책은 실행 가능한 gate/test/monitoring으로 표현한다.
5. 후속 작업은 MVP/Core/Product/RFP로 나눈다.

## Output Contract

```markdown
## KLIC Incident Postfix
- Incident Summary:
- Impact:
- Root Cause:
- Fix Applied:
- Requirement:
- Acceptance:
- Evidence:
- Preventive Gate:
- Follow-up Tasks:
  - MVP:
  - Core:
  - Product:
  - RFP:
- Learning:

## Work Log
- Decision:
- Reason:
- Files inspected:
- Contract checked:
- Risk found:
- Verification planned/run:
- Out-of-scope:
```

## Preventive Gate Examples

- regression test 추가
- alert threshold 조정
- invalid state 차단
- CLI/output contract QC 추가
- runbook 업데이트
- 공공데이터 표준 용어 drift check

## Hard Stops

- secret/PII/customer data 로그 원문을 반복하지 않는다.
- 운영 배포/권한 변경은 별도 승인 없이 지시하지 않는다.
- 증거 없는 root cause를 확정하지 않는다.
