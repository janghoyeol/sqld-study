# 3장 관리 구문 (DML · TCL · DDL · DCL) — 노랭이 99~126 (2026-11-18)

## 분류
| 분류 | 명령어 |
| --- | --- |
| DML | SELECT, INSERT, UPDATE, DELETE, MERGE |
| DDL | CREATE, ALTER, DROP, RENAME, **TRUNCATE** |
| DCL | GRANT, REVOKE |
| TCL | COMMIT, ROLLBACK, SAVEPOINT |

## DDL
- 제약조건: PK(NOT NULL + UNIQUE, 1개) · UNIQUE(**NULL 허용**) · NOT NULL · CHECK · FK(**NULL 허용**, 여러 개) · DEFAULT
- 복합 PK는 표 끝에 **한 번만** `CONSTRAINT 이름 PRIMARY KEY (a, b)` — 열마다 PRIMARY KEY 또는 PK 두 번 선언은 오류
- CREATE 후 `ALTER TABLE … ADD CONSTRAINT … PRIMARY KEY (…)`도 가능 (기존 데이터에 NULL·중복 없을 때)
- 이름 규칙: 문자로 시작 · A-Z a-z 0-9 _ $ # · 예약어 불가 · 테이블명 중복 불가 · **칼럼마다 데이터 유형 필수** · 테이블명 **단수형 권고**
- ALTER: 타입 변경 **Oracle `MODIFY (c …)` / SQL Server `ALTER COLUMN c …`**, 칼럼 삭제 `DROP COLUMN`, 이름 변경 Oracle `RENAME COLUMN a TO b` / SQL Server `sp_rename`

| | DELETE | TRUNCATE | DROP |
| --- | --- | --- | --- |
| 분류 | DML | DDL | DDL |
| 범위 | WHERE로 일부 | 전부 | 전부 + 구조 |
| ROLLBACK (Oracle) | ⭕ | ❌ | ❌ |

## FK 참조 동작
| 시점 | 동작 | 뜻 |
| --- | --- | --- |
| 부모 삭제 시 | CASCADE | 자식 **행째** 삭제 |
| | SET NULL | 자식 FK만 NULL |
| | SET DEFAULT | 자식 FK를 기본값 |
| | RESTRICT | 자식 있으면 삭제 **막음** |
| 자식 입력 시 | **DEPENDENT** | 부모 PK 없으면 입력 **막음** |
| | **AUTOMATIC** | 부모 PK를 **자동 생성** 후 입력 |
| | SET NULL / DEFAULT / CUSTOMIZED | NULL로 / 기본값으로 / 조건 만족 시 |
- FK는 칼럼 연결처럼 보여도 뜻은 "이 행은 저 부모 행에 속함" → CASCADE는 행째
- 규칙은 테이블 만들 때 붙여 두는 약속, 부모가 지워질 때 **자동 실행** → 사슬처럼 이어짐 (T 삭제 → S CASCADE → R SET NULL)

## DML
- **DATE 칼럼 INSERT**: SYSDATE ⭕ · SYSDATE−1 ⭕ · '20190301'(문자 → 자동 변환) ⭕ · **20190301(숫자) ❌** · 정석은 TO_DATE
- **UPDATE SET (서브쿼리)에 WHERE 없으면** 전체 행 대상 → 짝 없는 행은 서브쿼리 0줄 → **NULL로 덮어씀** → `WHERE EXISTS (같은 조건)`
- **MERGE**: MATCHED → UPDATE(+DELETE), NOT MATCHED → INSERT, 각각 WHERE 가능
  - **DELETE는 UPDATE된 행에만, UPDATE 후 값으로 판단**
  - INSERT에서 안 넣은 열은 NULL
- **TOP (SQL Server)**: `TOP(n)` · `TOP(n) PERCENT`(올림) · `TOP(n) WITH TIES`(ORDER BY 필수, n번째와 같은 값까지) · ORDER BY 이후에 자름 (ROWNUM과 다름)
- 중복 제거 = DISTINCT

## TCL · 트랜잭션
- 트랜잭션 = DB의 **논리적 작업 단위**
- ACID: 원자성(전부 or 전무) · 일관성(규칙 유지) · **고립성**(중간 결과를 남이 못 봄) · **지속성**(COMMIT 결과 영구)
- `ROLLBACK` = 마지막 COMMIT 이후 전부 / `ROLLBACK TO 저장점` = 그 뒤만
- **Oracle은 DDL 실행 시 앞 작업까지 자동 COMMIT** / SQL Server는 기본 AUTO COMMIT, DDL도 ROLLBACK 가능

## 연산자 우선순위
괄호 → 산술(`* /` → `+ -`) → 연결(`||`) → 비교 → **NOT → AND → OR**
- `a OR b AND c` = `a OR (b AND c)`

## 맞춤 20문항: 25분 17/20
- 틀림: 논리 연산자 우선순위 (2문제) · UNIQUE를 후보키와 혼동 (UNIQUE 제약조건은 NULL 허용)
- MERGE · SAVEPOINT + DDL 자동 커밋 · CASCADE 사슬 · UPDATE 서브쿼리 NULL 다 맞음

## 키 정리
| | NULL | 중복 | 개수 |
| --- | --- | --- | --- |
| PK | ❌ | ❌ | 1 |
| UNIQUE | ⭕ | ❌ | 여러 개 |
| FK | ⭕ | ⭕ | 여러 개 |
| 후보키(모델링) | ❌ | ❌ | PK 1 + 대체키 |
