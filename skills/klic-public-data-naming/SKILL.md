---
name: klic-public-data-naming
description: KLIC 공공·기관/eGovFrame 프로젝트용 공공데이터 표준 용어 기반 테이블·컬럼·API·DTO·변수·JSP 화면 라벨 네이밍 검토 스킬
---

# KLIC Public Data Naming

공공/행정/기관 프로젝트에서 테이블, 컬럼, API path, DTO, 변수, 화면 라벨, 문서 용어를 공공데이터 공통표준 용어 기준으로 검토하는 조직 스킬이다.

```text
Decision → Requirement → Acceptance → Task → Evidence → Learning
```

용어 변경도 구현 변경과 같다. 표준 후보를 찾는 데서 끝내지 않고, requirement/acceptance, contract impact, migration risk, evidence까지 연결한다.

참고 기준:

- `klic-awesome-code/gov-data-standards`
- 공공데이터포털(data.go.kr) 공공데이터 공통표준 용어
- 최신 연도 기준 우선, 매년 11월 개정 가능성 고려

## When to Use

- 공공/기관 도메인 모델링
- DB 테이블/컬럼 네이밍
- API path/DTO/변수명 검토
- 화면 라벨/문서 용어 통일
- 기존 용어를 표준 용어로 바꿀지 판단

## eGovFrame Naming Impact

전자정부프레임워크/MyBatis 프로젝트에서 명칭 변경은 한 계층에만 머물지 않는다.

- DB column rename은 MyBatis resultMap/DTO/JSP label/API docs까지 영향을 줄 수 있다.
- Java field rename은 JSP expression/form binding/JSON field와 contract를 깰 수 있다.
- 화면 라벨 변경은 공공데이터 표준 용어와 사용자 안내 문구를 함께 본다.
- 운영 DB/API contract가 있으면 표준 후보가 명확해도 `defer`를 우선 검토한다.

## Rules

1. 표준 용어를 확인하기 전 임의 축약어를 만들지 않는다.
2. 기존 운영 DB/API contract가 있으면 호환성을 우선한다.
3. rename은 영향 범위가 크므로 MVP bugfix에 섞지 않는다.
4. 표준 용어를 확인하지 못하면 `Unknown — gov-data-standards 확인 필요`로 남긴다.
5. 변경 제안에는 migration 필요 여부를 포함한다.

## Decision Matrix

| Recommendation | Condition |
|----------------|-----------|
| keep | 기존 contract 영향이 크거나 표준 후보 불확실 |
| rename now | 신규 설계이고 표준 후보 명확 |
| defer | 운영 contract 영향이 있어 별도 migration 필요 |

## Output Contract

```markdown
## KLIC Public Data Naming
- Applies: yes | no | unknown
- Domain Terms Found:
- Current Names:
- Standard Candidates:
- Contract Impact: DB | API | UI | docs | none
- Migration Risk: none | low | medium | high
- Recommendation: keep | rename now | defer
- Requirement / Acceptance:
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

## Hard Stops

- 표준 후보가 불확실하면 rename을 강제하지 않는다.
- 운영 DB/API 이름 변경은 별도 승인 없이 제안만 한다.
- 고객/개인정보 예시값은 출력하지 않는다.
