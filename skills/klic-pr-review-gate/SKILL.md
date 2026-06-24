---
name: klic-pr-review-gate
description: KLIC/GIDO 철학 기반 유지보수 조직용 PR·diff 계약·품질·보안 리뷰 게이트 스킬; eGovFrame XML Mapper·Spring 설정·JSP·공통컴포넌트 변경 검토
---

# KLIC PR Review Gate

PR 또는 diff를 merge 가능한 상태인지 판정하는 조직 스킬이다. 목적은 취향 리뷰가 아니라 계약, 스펙, 테스트, 보안, 유지보수 위험을 근거로 판정하는 것이다.

```text
Decision → Requirement → Acceptance → Task → Evidence → Learning
```

## When to Use

- PR 리뷰 전/중
- 큰 diff의 위험도 판단
- merge block 여부 판단
- 테스트 누락 확인
- public contract 변경 확인
- docs/spec/plugin drift 확인

## eGovFrame Review Triggers

다음 변경은 오류 질문/PR 리뷰에서 자동으로 contract risk로 본다.

- Spring context/properties/security/session 설정 변경
- Controller request mapping, parameter binding, response format 변경
- Service transaction boundary 또는 business rule 변경
- MyBatis XML sql id/resultMap/parameterMap/동적 SQL 변경
- JSP 화면 라벨, validation message, form field name 변경
- batch job, schedule, reader/processor/writer 설정 변경
- 공통컴포넌트 호출부, 확장부, override 변경

## Review Axes

### Contract

- public API/CLI/options/output format이 깨졌는가?
- DB schema/API path/event contract 변경이 있는가?
- 문서와 구현이 drift 났는가?

### Spec Alignment

- 요구사항 없는 기능이 추가됐는가?
- acceptance를 만족하는 evidence가 있는가?
- scope가 MVP/Core/Product/RFP 중 어디인가?

### Safety

- injection, path traversal, XSS, SQL/shell 위험이 있는가?
- secret/PII/customer data 노출이 있는가?
- auth/permission 정책이 변경됐는가?

### Maintainability

- 최소 diff인가?
- 리팩터링과 동작 변경이 섞였는가?
- 불필요한 추상화가 추가됐는가?

## Verdict

| Verdict | Meaning |
|---------|---------|
| APPROVE | Must Fix 없음, evidence 충분 |
| APPROVE_WITH_NOTES | Must Fix 없음, Should Fix만 있음 |
| REQUEST_CHANGES | Must Fix 있음 |
| BLOCK | 보안/데이터/계약 파괴 위험 있음 |

## Output Contract

```markdown
## KLIC PR Review Gate
- Verdict: APPROVE | APPROVE_WITH_NOTES | REQUEST_CHANGES | BLOCK
- Scope: MVP | Core | Product | RFP
- Contract Risk:
- Spec Alignment:
- Test Evidence:
- Security/Safety:
- Public Data Standards:
- Must Fix:
- Should Fix:
- Do Not Fix:
- Merge Recommendation:

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

- evidence 없이 approve하지 않는다.
- secret/PII가 보이면 원문을 반복하지 말고 redacted evidence로 보고한다.
- 운영 배포/권한/DB 변경은 별도 승인 없이는 approve하지 않는다.
