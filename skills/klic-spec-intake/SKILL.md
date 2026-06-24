---
name: klic-spec-intake
description: KLIC/GIDO 철학 기반 유지보수 조직용 오류/개선 요청 접수·EARS 요구사항·Acceptance·Task 변환 스킬; 공공기관/eGovFrame 산출물 기준 포함
---

# KLIC Spec Intake

운영자, 기획자, 개발자의 자연어 요청을 GIDO식 스펙 체인으로 바꾸는 조직 스킬이다.

```text
Decision → Requirement → Acceptance → Task → Evidence → Learning
```

## When to Use

- 요청이 애매할 때
- 기능 추가 전에 스펙을 잡아야 할 때
- 버그 수정의 기대 동작을 명확히 해야 할 때
- 유지보수 작업을 task로 쪼개야 할 때
- 완료 기준과 evidence를 먼저 정해야 할 때

## eGovFrame Intake Routing

오류·개선 요청은 먼저 산출물/계층으로 분리한다.

- 화면: JSP, form field, validation message, 접근성/라벨
- API/Web: Controller mapping, request/response, session/auth
- 업무: Service rule, transaction, policy/rule set
- 데이터: MyBatis mapper, SQL, table/column, migration risk
- 배치: job parameter, schedule, restart/retry 기준
- 설정: Spring context, properties, profile, 공통컴포넌트 설정
- 표준: 공공데이터 용어, 기관 문서 용어, 화면 라벨

## Intake Rules

1. 한 번에 구현으로 점프하지 않는다.
2. 요청을 Decision과 Requirement로 분리한다.
3. Requirement는 관찰 가능한 동작으로 쓴다.
4. Acceptance는 테스트/검증 가능해야 한다.
5. Task는 Acceptance를 만족시키는 최소 단위여야 한다.
6. MVP/Core/Product/RFP 티어를 명시한다.
7. 불확실한 내용은 assumption으로 공개한다.

## EARS Template

```markdown
WHEN <상황/트리거>이면,
시스템은 <관찰 가능한 동작>을 MUST/SHOULD/MAY <수행한다>.
```

## Scope Control

| Tier | Include | Exclude |
|------|---------|---------|
| MVP | 핵심 실패/핵심 요구 | 대규모 구조 변경 |
| Core | 안정 운영 기본 품질 | UX polish |
| Product | 운영 편의/사용자 경험 | 아키텍처 전면 교체 |
| RFP | 장기 제안 | 즉시 구현 |

## Output Contract

```markdown
## KLIC Spec Intake
- Decision:
- Assumptions:
- Tier: MVP | Core | Product | RFP
- Requirement:
- Acceptance Criteria:
- Task Candidates:
- Evidence Needed:
- Out of Scope:
- Open Questions:

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

- acceptance를 만들 수 없으면 구현 계획을 만들지 않는다.
- public API/DB/auth 정책 변경은 별도 decision으로 분리한다.
- secret/PII/customer data 원문은 요구사항에 넣지 않는다.
