# 노랭이 1~60 질문 노트 + 긴 지문 연습 (2026-11-13 ~ 11-15)

## 노랭이 1~50 (SQL 기본)에서 질문한 것
- **CASE로 정렬**: `ORDER BY (CASE WHEN id = 999 THEN 0 ELSE id END)` → THEN 0은 true/false가 아니라 **숫자 0** → 오름차순이면 999가 맨 앞
- **GROUP BY 있으면** SELECT·HAVING·ORDER BY 전부 GROUP BY 열 + 집계만 (없으면 ORDER BY는 표의 아무 열 OK). `ORDER BY count(*)` 가능
- **날짜 계산**: 1/N일 = 24÷N시간 → 1/24 = 1시간, **1/12 = 2시간**, 1/24/60 = 1분
- **NVL(a, b)** = a가 NULL이면 b / **NVL vs COALESCE**: NVL은 Oracle 전용·값 2개, COALESCE는 표준·여러 개, 2개일 땐 결과 같음
- **`표.열`의 표는 그 열을 진짜 가진 표**여야 함. 같은 이름 열이 두 표에 있으면 표 이름 필수
- `count(*) cnt` → **cnt는 별칭** (AS 생략). LIKE로도 조인 가능. **따옴표 안 값은 대소문자 구분** ('S%' ≠ 's%')
- **순수 관계 연산자 = 셀·프·조·디** (SELECT·PROJECT·JOIN·DIVIDE). 일반 집합 = UNION·INTERSECTION·DIFFERENCE·CARTESIAN PRODUCT. 관계대수 SELECT = SQL의 **WHERE**
- **JOIN 종류**: FULL OUTER(양쪽 전부) · SELF(같은 표 두 번, 별칭 필수) · NATURAL(같은 이름 열 자동, 공통 열에 표 이름 붙이면 오류) · USING(열 골라서, 표 이름 붙이면 오류) · CROSS(모든 조합 = 곱)
- **"가장 적절한 것"** = 요청한 만큼 정확히. "사원 없는 부서도" → LEFT OUTER (FULL은 부서 없는 사원까지 넘침)

## 긴 지문 연습
- 내 약점: **표 + 기준 SQL + 긴 보기 4개** (동일한 것 / 실행 결과 / 다른 하나)
- 요령: 보기끼리 **다른 부분만 밑줄** → 그 부분만 비교 (메모장 diff처럼)
- 1차 10문항 40분 **7/10**, 2차 8문항 **5/8**, 집합 연산 8문항 **6/8**

### 여기서 확실히 잡은 것
| 개념 | 핵심 |
| --- | --- |
| NOT IN + NULL | 괄호 안에 NULL 하나면 **결과 없음**. IN·EXISTS·NOT EXISTS는 영향 없음 |
| UNION vs UNION ALL | **UNION ALL만 중복 그대로**, UNION·INTERSECT·MINUS는 중복 제거 |
| UNION ALL 행 수 | 두 표 행 수를 그냥 더함 (7 + 4 = 11) |
| 집합 연산 | **꺼낸 열만** 합침 · 같은 자리끼리 · 열 개수·자료형 맞아야 · 열 이름은 첫 SELECT · **줄 전체가 같아야 중복** |
| NULL 비교 | **`=` 비교(WHERE·ON·EXISTS)에선 NULL = NULL은 알 수 없음 → 짝 아님** / **집합 연산에선 NULL끼리 같음 → 짝** |
| CASE WHEN | 조건이 UNKNOWN(NULL > 0)이면 THEN 건너뛰고 ELSE로 |
| NULLIF(a, b) | 같으면 NULL, 다르면 a |
| NVL2(a, b, c) | a 있으면 b, NULL이면 c (0은 NULL 아님) |
| ORDER BY 기본 | ASC (Oracle은 NULL이 맨 뒤) |
| LEFT JOIN 후 | 짝 여러 개면 줄이 불어남 → DISTINCT 필요 |

## 노랭이 51~60 (SQL 활용)
| 번호 | 결과 | 배운 것 |
| --- | --- | --- |
| 51 | ✅ | 다중 칼럼 서브쿼리 `WHERE (dept, sal) IN (SELECT dept, MAX(sal) …)` — 개수·순서 같아야 |
| 52 | ✅ | ROLLUP·CUBE·GROUPING SETS 묶음 외워서 맞힘 |
| 53·54·55 | ✅ | UNION 각 SELECT에 GROUP BY 가능, ORDER BY는 맨 끝 한 번 |
| 56 | ❌ | UNION ALL 뒤에 **UNION이 한 번이라도 오면 전체 중복 제거** |
| 57 | ✅ | `GROUP BY grade, ROLLUP(job)` = 부분 ROLLUP, **밖의 열은 항상 붙어 있어서 전체 합계 없음** |
| 58 | ❌ | RATIO_TO_REPORT = 급여 / 부서합. **OVER에 ORDER BY 들어가면 누적 합계** → 다시 풀 것 |
| 59 | ❌ | **EXCEPT = MINUS** (이름만 다름). 1:1 양쪽 필수면 INTERSECT 건수 = JOIN 건수 |
| 60 | ❌ | CONNECT BY PRIOR는 **연결 규칙**, ORDER SIBLINGS BY는 **형제끼리만**, 자식 먼저 끝까지 |

### 이름만 다른 것 (Oracle vs 표준/SQL Server)
| 기능 | Oracle | 표준 / SQL Server |
| --- | --- | --- |
| 차집합 | MINUS | EXCEPT |
| NULL 바꾸기 | NVL | COALESCE / ISNULL |
| 상위 N | ROWNUM, FETCH FIRST | TOP(n), FETCH FIRST |
| 바깥 조인 | (+) | LEFT/RIGHT OUTER JOIN |
| 문자 잇기 | `\|\|` | `+` |

### 계층형 문제 푸는 순서
1. START WITH로 시작 행 → 2. `PRIOR 부모쪽 = 자식쪽`으로 화살표 → 3. 나무 그림 → 4. SIBLINGS면 형제끼리만 정렬 → 5. **자식 먼저 끝까지 내려가며** 읽기
