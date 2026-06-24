---
name: klic-refactor-scope
description: KLIC/GIDO 철학 기반 유지보수 조직용 리팩터링 범위 통제·과추상화 방지 스킬; eGovFrame 공통화·MyBatis/JSP 대량 변경 범위 판정
---

# KLIC Refactor Scope

리팩터링을 해도 되는지, 어디까지 해야 하는지, 무엇을 하지 말아야 하는지 판정하는 조직 스킬이다.

```text
Decision → Requirement → Acceptance → Task → Evidence → Learning
```

## When to Use

- 코드 정리 범위를 정해야 할 때
- 리팩터링과 버그 수정이 섞이려 할 때
- 대규모 구조 변경 제안이 나왔을 때
- 과추상화가 의심될 때
- 테스트 없이 정리하려 할 때

## Rules

1. 리팩터링은 동작 변경이 아니다.
2. behavior preservation acceptance가 없으면 리팩터링하지 않는다.
3. 버그 수정과 리팩터링은 가능한 분리한다.
4. 변경 가능성이 높은 축만 분리한다.
5. 호출자가 하나뿐인 인터페이스/팩토리/전략 패턴은 기본적으로 금지한다.
6. 포맷팅/네이밍 정리는 요청 범위에 있을 때만 한다.

## eGovFrame Refactor Boundaries

전자정부프레임워크 유지보수에서는 공통화 자체가 목적이 되면 안 된다.

- 공통컴포넌트화는 2개 이상 실제 사용처와 같은 변경축이 확인될 때만 검토한다.
- MyBatis XML/JSP/DTO 대량 rename은 버그 수정에 섞지 않고 Product/RFP로 분리한다.
- Controller-Service-DAO 계층 이동은 behavior preservation acceptance와 regression evidence가 있을 때만 허용한다.
- XML 설정, properties, batch config 변경은 리팩터링이 아니라 contract 변경으로 본다.

## Change-prone Axes

분리 후보:

- provider
- theme
- layout mode
- policy/rule set
- prompt template
- data source
- public data term mapping

분리 금지 후보:

- 바뀔 근거 없는 내부 구현
- 단일 호출자용 추상화
- 테스트 없는 구조 변경
- PR 리뷰를 어렵게 만드는 대량 이동

## Verdict

| Verdict | Meaning |
|---------|---------|
| DO_NOW | 직접 관련 있고 테스트 가능 |
| DEFER | 가치 있지만 별도 작업 필요 |
| DO_NOT_TOUCH | 리스크가 가치보다 큼 |

## Output Contract

```markdown
## KLIC Refactor Scope
- Verdict: DO_NOW | DEFER | DO_NOT_TOUCH
- Scope: MVP | Core | Product | RFP
- Behavior Change: yes | no
- Requirement / Acceptance:
- Safe Minimal Refactor:
- Forbidden Changes:
- Required Tests:
- Risk:
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

## Hard Stops

- 테스트 없이 behavior-changing refactor를 제안하지 않는다.
- public contract 변경은 리팩터링으로 포장하지 않는다.
- MVP 버그 수정에 Product/RFP 구조 정리를 섞지 않는다.
