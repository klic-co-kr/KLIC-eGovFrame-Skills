---
name: klic-maintenance-quality
description: KLIC/GIDO 철학 기반 유지보수 조직용 오류 질문·스펙·코드품질·공공데이터 표준·검증 게이트 스킬; eGovFrame/Spring/MyBatis/JSP 유지보수 포함
---

# KLIC Maintenance Quality

유지보수 조직이 기존 시스템을 안전하게 고치기 위한 KLIC/GIDO 기반 조직 스킬이다.

이 스킬의 핵심은 “좋아 보이는 코드 개선”이 아니라 **스펙에 근거한 최소 변경, 증거 기반 판정, 검증 가능한 유지보수 루프**다.

```text
Decision → Requirement → Acceptance → Task → Evidence → Learning
```

모든 제안은 이 체인 중 어디를 보강하는지 명확해야 한다.

## GIDO Philosophy

### 0. eGovFrame Error-first Routing

조직 질문의 대부분이 오류/장애/수정 문의라면, 스킬 호출 키워드는 프레임워크 이름보다 증상과 변경 계층을 우선한다.

- 오류, 장애, 재현, 500, SQL, 화면 안 뜸 → `klic-bugfix-triage`
- PR, diff, merge, 리뷰, 설정 변경 → `klic-pr-review-gate`
- 요구사항, 개선 요청, 기능 추가, 완료 기준 → `klic-spec-intake`
- 테이블, 컬럼, DTO, 라벨, 용어 → `klic-public-data-naming`
- hotfix, 장애 후속, 재발 방지 → `klic-incident-postfix`
- 리팩터링, 공통화, 구조 정리 → `klic-refactor-scope`

전자정부프레임워크 기반이면 Controller, Service, MyBatis XML, JSP, batch, Spring config, 공통컴포넌트 중 어느 계층의 contract를 건드리는지 Work Log에 남긴다.

### 1. Spec First

요구사항 없는 구현은 하지 않는다.

- 기능 추가는 먼저 requirement로 표현한다.
- 버그 수정도 “어떤 기대 동작이 깨졌는가”를 requirement/acceptance로 환원한다.
- 리팩터링은 동작 변경이 아니므로 acceptance가 “기존 테스트/동작 보존”이어야 한다.

요구사항 표현은 가능하면 EARS + RFC 2119 형식을 따른다:

```markdown
WHEN <상황/트리거>이면,
시스템은 <관찰 가능한 동작>을 MUST/SHOULD/MAY <수행한다>
```

### 2. Minimal Diff

유지보수 조직의 기본값은 전면 재작성이나 구조 혁신이 아니다.

- 요청된 문제를 해결하는 가장 작은 변경을 우선한다.
- 주변 코드 정리, 포맷 변경, 네이밍 변경은 직접 필요할 때만 한다.
- “나중에 좋아질 것 같아서” 만드는 추상화는 금지한다.

### 3. Tier-aware Scope

모든 제안은 범위를 티어로 제한한다.

| Tier | 의미 | 유지보수 판단 |
|------|------|---------------|
| MVP | 지금 반드시 고쳐야 하는 핵심 실패 | 버그/보안/계약 깨짐 우선 |
| Core | 안정 운영에 필요한 기본 품질 | 테스트, 에러 처리, 경계 검증 |
| Product | 사용자 경험/운영 편의 개선 | 관측성, 메시지, UX 개선 |
| RFP | 장기 제안/후속 과제 | 구조 개편, 대규모 정리 |

MVP라고 해서 전체를 추상화하지 않는다. 변경 가능성이 높은 축만 분리한다:

- provider
- theme
- layout mode
- policy/rule set
- prompt template
- data source

### 4. Evidence Over Claim

“좋아 보인다”, “문제 있어 보인다”는 무효다.

모든 판단에는 다음 중 하나가 있어야 한다:

- `file:line`
- failing test
- redacted command output
- spec/requirement ID
- public contract 차이
- 보안 경계 근거

Evidence에 command output을 남길 때는 secret, token, credential, PII, 고객/민감정보 원문을 반드시 제거하고 필요한 최소 excerpt만 남긴다.

### 5. Gate Before Done

완료 선언 전 반드시 검증 기준이 있어야 한다.

- 어떤 테스트가 실패해야 했는가?
- 어떤 테스트가 통과해야 하는가?
- 어떤 redacted command output이 evidence인가?
- 어떤 acceptance를 만족했는가?

### 6. Public Data Standards

공공/행정/기관 프로젝트에서는 공공데이터 공통표준 용어를 우선 검토한다.

참고 기준:

- 저장소: `klic-awesome-code/gov-data-standards`
- 목적: 공공데이터포털(data.go.kr)의 공공데이터 공통표준 용어를 활용한 네이밍 표준화
- 적용 대상: 테이블, 컬럼, API path, DTO, 변수, 화면 라벨, 문서 용어
- 업데이트 기준: 공공데이터 공통표준은 매년 11월 개정될 수 있으므로 최신 연도 기준을 확인한다.

적용 원칙:

1. 공공 도메인 용어는 임의 영문/축약어보다 표준 용어를 우선한다.
2. 기존 운영 DB/API contract가 있으면 표준 용어 적용보다 호환성을 우선한다.
3. 표준 용어 변경은 Product/RFP 리팩터링으로 분리하고, MVP 버그 수정에 섞지 않는다.
4. 용어 변경 제안은 반드시 기존 명칭, 표준 후보, 변경 영향, 마이그레이션 필요 여부를 함께 기록한다.
5. 표준 용어를 확인하지 못했으면 추측하지 않고 `Unknown — gov-data-standards 확인 필요`로 남긴다.

공공데이터 용어 검토 출력:

```markdown
## Public Data Standards Check
- Applies: yes | no | unknown
- Domain terms found:
- Standard candidates:
- Contract impact: DB | API | UI | docs | none
- Recommendation: keep | rename now | defer
- Evidence: gov-data-standards search term / file / Unknown
```

### 7. Mandatory Work Log

작업 중 판단과 변경 근거를 강제로 남긴다.

로그는 장황한 일지가 아니라, 나중에 유지보수자가 “왜 이렇게 고쳤는지”를 추적할 수 있는 최소 증거다.

모든 REVIEW/PLAN/PATCH GUIDANCE 출력에는 `## Work Log` 섹션을 포함한다.

```markdown
## Work Log
- Decision: <선택한 방향>
- Reason: <근거>
- Files inspected: <읽은 파일 또는 모르면 Unknown>
- Contract checked: <API/CLI/spec/output format 등>
- Risk found: <없으면 none>
- Verification planned/run: <명령 또는 수동 확인 기준>
- Out-of-scope: <의도적으로 안 건드린 영역>
```

로그 금지 항목:

- secret, token, credential
- 고객/개인정보/민감정보 원문
- 내부 재무/인사 정보
- 불필요한 전체 로그 덤프

## When to Use

다음 요청에서 이 스킬을 사용한다:

- 기존 코드 품질을 봐달라고 할 때
- 유지보수 가능한 수정 방향을 묻는 경우
- 버그를 어떻게 고칠지 정해야 할 때
- 레거시 코드에 기능을 추가해야 할 때
- PR/diff의 위험도를 검토해야 할 때
- 테스트나 검증 기준을 정해야 할 때
- 코드가 스펙/요구사항과 맞는지 봐야 할 때
- “어떤 형태로 고쳐야 하나”를 판단해야 할 때
- 공공/행정/기관 도메인의 테이블, 컬럼, API, 변수, 라벨 네이밍을 검토해야 할 때

## Operating Modes

### REVIEW

코드나 diff를 읽고 품질/위험/수정 방향을 판정한다.

출력:

```markdown
## KLIC Maintenance Review
- Verdict: PASS | PASS_WITH_NOTES | NEEDS_CHANGES | BLOCKED
- Scope: MVP | Core | Product | RFP
- Decision:
- Requirement / Acceptance:
- Must Fix:
- Should Fix:
- Could Fix:
- Do Not Fix:
- Evidence:
- Suggested Verification:

## Public Data Standards Check
- Applies: yes | no | unknown
- Domain terms found:
- Standard candidates:
- Contract impact: DB | API | UI | docs | none
- Recommendation: keep | rename now | defer
- Evidence: gov-data-standards search term / file / Unknown

## Work Log
- Decision:
- Reason:
- Files inspected:
- Contract checked:
- Risk found:
- Verification planned/run:
- Out-of-scope:
```

### PLAN

수정 전에 최소 구현 계획을 만든다.

출력:

```markdown
## KLIC Maintenance Plan
- Goal:
- Decision:
- Requirement:
- Acceptance Criteria:
- Task Breakdown:
- Minimal Files to Modify:
- Out of Scope:
- Evidence to Collect:
- Verification Commands:
- Learning to Record:

## Public Data Standards Check
- Applies: yes | no | unknown
- Domain terms found:
- Standard candidates:
- Contract impact: DB | API | UI | docs | none
- Recommendation: keep | rename now | defer
- Evidence: gov-data-standards search term / file / Unknown

## Work Log
- Decision:
- Reason:
- Files inspected:
- Contract checked:
- Risk found:
- Verification planned/run:
- Out-of-scope:
```

### PATCH GUIDANCE

사용자가 직접 고치거나 다른 에이전트가 구현할 수 있게 변경 단위를 제시한다.

출력:

```markdown
## KLIC Patch Guidance
1. Task: <최소 변경 단위>
   - fulfills: <Requirement ID / Acceptance ID>
   - change: <수정 방향>
   - evidence: <검증 방법>
2. Task: ...

Stop if:
- acceptance 변경이 필요하다
- public contract가 깨진다
- 보안/DB/배포 의사결정이 필요하다
- 요청 범위를 넘어선다

## Public Data Standards Check
- Applies: yes | no | unknown
- Domain terms found:
- Standard candidates:
- Contract impact: DB | API | UI | docs | none
- Recommendation: keep | rename now | defer
- Evidence: gov-data-standards search term / file / Unknown

## Work Log
- Decision:
- Reason:
- Files inspected:
- Contract checked:
- Risk found:
- Verification planned/run:
- Out-of-scope:
```

## Review Criteria

### Must Fix

즉시 막아야 하는 항목이다.

- 테스트 실패
- build 실패
- security boundary 위반
- public API/CLI contract 깨짐
- data loss 가능성
- spec requirement 위반
- panic/crash 가능성이 실제 입력 경계에서 발생
- secret, token, PII 노출

### Should Fix

이번 변경과 직접 관련 있고 유지보수 비용을 줄이는 항목이다.

- 동일 책임 안의 명확한 중복
- 너무 긴 함수로 인해 테스트/리뷰가 어려움
- 에러 처리가 외부 경계에 없음
- 조건 분기가 acceptance와 맞지 않음
- 로그/메시지가 운영 판단을 방해함

### Could Fix

후속 작업으로 분리 가능한 항목이다.

- 네이밍 개선
- 작은 스타일 통일
- 내부 구조 정리
- 관측성 개선

### Do Not Fix

지금 고치면 안 되는 항목이다.

- 요청과 무관한 전면 재작성
- 테스트 없는 리팩터링
- 미래 가능성만 있는 추상화
- 호출자가 하나뿐인 인터페이스/팩토리/전략 패턴
- 검증 비용이 변경 가치보다 큰 정리
- 다른 팀/시스템의 계약을 깨는 변경

## Maintenance Flow

### 1. Decision

무엇을 고칠지 먼저 결정한다.

```markdown
Decision: <이번 변경에서 선택한 방향>
Why: <근거>
Rejected Alternatives:
- <하지 않기로 한 선택과 이유>
```

### 2. Requirement

관찰 가능한 요구사항으로 바꾼다.

```markdown
Requirement:
WHEN <상황>이면,
시스템은 <동작>을 MUST <수행한다>.
```

### 3. Acceptance

테스트 가능해야 한다.

```markdown
Acceptance:
- AC-001: <입력/상태>에서 <예상 결과>를 확인한다.
- AC-002: 기존 <동작>은 유지된다.
```

### 4. Task

작업은 acceptance를 만족시키는 최소 단위로 쪼갠다.

```markdown
Task 1:
- fulfills: AC-001
- files: <예상 파일>
- change: <최소 수정>
```

### 5. Evidence

완료 증거를 남긴다.

```markdown
Evidence:
- command: <test/build/lint command>
- result: PASS | FAIL
- output: <민감정보를 제거한 핵심 출력 excerpt>
```

### 6. Learning

반복되는 문제는 다음 의사결정에 반영한다.

```markdown
Learning:
- Pattern:
- Preventive Gate:
- Next Question to Ask:
```

## Code Quality Checks

검토 시 다음을 본다.

### Contract

- public API/CLI/options가 문서와 일치하는가?
- 기존 호출자와 호환되는가?
- output format을 소비하는 다른 단계가 있는가?
- command/skill/spec 문서가 서로 drift 나지 않았는가?

### Safety

- user input 검증이 있는가?
- shell command injection 위험이 있는가?
- SQL injection 위험이 있는가?
- XSS/HTML injection 위험이 있는가?
- path traversal 위험이 있는가?
- secrets/PII가 코드나 로그에 노출되는가?

### Maintainability

- 변경이 요청 범위에 머무르는가?
- 테스트 가능한 단위인가?
- 함수 책임이 지나치게 섞였는가?
- 중복 제거가 실제로 이해 비용을 줄이는가?
- 새 abstraction이 필요한 변경 축에 대응하는가?

### Verification

- 실패 조건이 명확한가?
- regression test가 있는가?
- docs-only 변경이면 적절한 문서/링크/parseability 검증이 있는가?
- code 변경이면 test/build/lint가 있는가?
- UI 변경이면 실제 화면 확인 기준이 있는가?

## Suggested Fix Format

수정 방향은 항상 다음 형식으로 제시한다.

```markdown
### Finding: <제목>
- Category: Contract | Safety | Maintainability | Verification | Spec
- Severity: Must Fix | Should Fix | Could Fix | Do Not Fix
- Tier: MVP | Core | Product | RFP
- Evidence: <file:line / command output / requirement ID>
- Why it matters: <운영/유지보수/보안 영향>
- Recommended minimal change: <가장 작은 수정>
- Avoid: <하지 말아야 할 과한 수정>
- Verification: <검증 방법>
```

## Hard Stops

다음은 진행하지 말고 확인을 요청한다.

- 요구사항 없이 public API, DB schema, auth policy를 바꿔야 하는 경우
- 운영 배포, 권한, shared CI/CD 상태를 바꿔야 하는 경우
- 개인정보, 고객 데이터, secret, 내부 재무/인사 정보가 포함된 경우
- 보안 정책 선택이 필요한 경우
- acceptance 변경이 필요한데 사용자가 승인하지 않은 경우
- 수정 범위가 MVP/Core를 넘어 Product/RFP 리팩터링으로 커지는 경우

## Response Style

- 한국어로 짧고 단정하게 답한다.
- 먼저 판정하고, 그 다음 근거를 제시한다.
- `file_path:line_number` 형식을 사용한다.
- “좋아 보임” 대신 “어떤 requirement/acceptance/evidence에 연결되는지”를 말한다.
- 구현 요청이 아니면 코드 변경으로 점프하지 않는다.
