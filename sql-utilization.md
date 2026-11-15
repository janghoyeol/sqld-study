# SQL 활용 개념 정리 (2026-11-15)

예제 표 SAL: (10, A, 300) (10, B, 200) (20, C, 500) (20, D, 500) (20, E, 100)

## 1. 그룹 함수: ROLLUP · CUBE · GROUPING SETS
(dept, sal) 기준 묶음: 🅰 상세(dept, sal) · 🅱 dept 소계 · 🅲 sal 소계 · 🅳 전체 합계

| | 🅰 | 🅱 | 🅲 | 🅳 | SAL 표 줄 수 |
| --- | --- | --- | --- | --- | --- |
| GROUP BY dept, sal | ✅ | | | | 4 |
| ROLLUP(dept, sal) | ✅ | ✅ | | ✅ | 7 |
| CUBE(dept, sal) | ✅ | ✅ | ✅ | ✅ | 11 |
| GROUPING SETS(dept, sal) | | ✅ | ✅ | | 6 |

- ROLLUP = GROUPING SETS((dept, sal), (dept), ()) / CUBE = GROUPING SETS((dept, sal), (dept), (sal), ())
- **ROLLUP만 열 순서 중요**. ROLLUP·CUBE **밖의 열은 모든 묶음에 항상 붙음** (부분 ROLLUP)
- GROUPING(열): 소계 때문에 생긴 NULL이면 1

## 2. 윈도우 함수
- GROUP BY는 줄을 **합침** (dept 2줄) / `OVER (PARTITION BY dept)`는 **줄 그대로** (5줄) + 옆에 부서 합계
- OVER 안 ORDER BY = **계산 순서** (결과 정렬은 맨 끝 ORDER BY가 따로)
- **OVER에 ORDER BY만 있으면 기본 RANGE → 같은 값은 같이 더함**
  - SUM(sal) OVER (ORDER BY sal): 100, 300, 600, **1600, 1600** (ROWS면 1100, 1600)
- 순위: RANK(1,1,3) · DENSE_RANK(1,1,2) · ROW_NUMBER(1,2,3, 동점 순서 보장 X)
- LAG(앞 줄) · LEAD(뒤 줄) · FIRST_VALUE · LAST_VALUE(기본 범위면 자기 자신)
- RATIO_TO_REPORT(내 몫) · PERCENT_RANK · CUME_DIST · **NTILE(n) = 덩어리로 나누고 나머지는 앞 그룹부터 하나씩** (10명 NTILE(4) → 3, 3, 2, 2)
  - 게임 확률의 '모듈러 편향'도 같은 원리 (나머지가 앞쪽 상자로 → 1%가 1.164%)
- 윈도우 함수는 WHERE에 못 씀 → 인라인 뷰로 감싸서

## 3. Top N
- **ROWNUM은 ORDER BY보다 먼저** 붙음 → 정렬은 인라인 뷰 안에서
- `ROWNUM <= n` OK / **`ROWNUM > 1`, `= 2`는 0줄** (첫 줄이 1을 못 받으면 아무도 2가 못 됨)
- 중간 순위: `SELECT ROWNUM AS rn …`으로 **별칭을 굳힌 뒤 바깥에서** `WHERE rn > 1`
- FETCH FIRST n ROWS **ONLY** = 딱 n줄 / **WITH TIES** = n번째 줄과 같은 값까지 더 / **OFFSET k** = k줄 건너뛰기
  - SAL을 sal 높은 순: FIRST 1 WITH TIES → C, D / FIRST 2 WITH TIES → C, D (A는 300이라 안 붙음)

## 4. 계층형 질의
```sql
SELECT LEVEL, ename FROM EMP
START WITH mgr IS NULL
CONNECT BY PRIOR empno = mgr;   -- 부모의 empno를 mgr로 가진 행 = 자식 → 위에서 아래로
```
- KING(1) → JONES·ALLEN·MILLER(2) → SCOTT, WARD·MARTIN(3)
- `PRIOR mgr = empno`면 아래에서 위로
- LEVEL = 몇 단계 내려왔나 · CONNECT_BY_ISLEAF · CONNECT_BY_ROOT · SYS_CONNECT_BY_PATH
- **ORDER SIBLINGS BY = 형제끼리만 정렬**, 나무 모양 유지. 읽을 땐 자식 먼저 끝까지
- **WHERE = 다 만든 뒤 줄만 지움 (자식 남음)** / **CONNECT BY에 조건 = 가지째 잘림**

## 5. PIVOT / UNPIVOT (아직 헷갈림 → 문제로 다시)
- PIVOT: 세로 → 가로. `FOR 분기 IN ('Q1' AS Q1, …)`의 값이 **열 이름**이 됨, 집계 함수 필수
  - ⚠️ **PIVOT에 안 쓴 열은 자동으로 GROUP BY** → 인라인 뷰로 필요한 열만 넣기 (안 그러면 줄 수 늘어남)
  - SUM(CASE WHEN 분기 = 'Q1' THEN 매출 END)와 같은 결과
- UNPIVOT: 가로 → 세로. 열 이름이 **값**으로 내려옴
  - ⚠️ 기본은 **NULL 칸 제외** (INCLUDE NULLS로 포함) · PIVOT한 걸 UNPIVOT해도 **원래대로 안 돌아옴** (SUM으로 합친 건 못 쪼갬)

## 6. 정규 표현식
- REGEXP_LIKE · REGEXP_SUBSTR · REGEXP_REPLACE · REGEXP_INSTR · REGEXP_COUNT
- `.` 아무 글자 · `*` 0번 이상 · `+` 1번 이상 · `?` 0~1번 · `^` 시작 · `$` 끝 · `[0-9]` · `[^0-9]` 제외 · `{n}` n번
- `^`는 대괄호 밖이면 시작, **대괄호 안 맨 앞이면 제외**
