# 노랭이 81~98 (2026-11-17 ~ 11-18) — 2과목 SQL 완주

## CUBE·ROLLUP 안의 괄호 = 한 덩어리
- `CUBE((a), (b), (a, b))` → 덩어리 3개의 2³ = 8조합을 펼친 뒤 열을 풀어 씀
  - (a,b) **5번** + (a) + (b) + () → Oracle은 중복 줄을 안 지움 (GROUP_ID로 구분)
  - `GROUPING SETS((a), (b), (a, b))`는 적은 것만 한 번씩 → 둘은 결과 다름
- `ROLLUP((a), (b))` = `ROLLUP(a, b)` (괄호 안 열이 1개면 의미 없음)
- `ROLLUP((a, b))` → (a,b), () / `ROLLUP(a, (b, c))` → (a,b,c), (a), ()

## LAG / LEAD
`LAG(열, n, 기본값) OVER (PARTITION BY … ORDER BY …)`
- LAG = **앞(이전)** 줄 값, LEAD = **뒤(다음)** 줄 값 ("lag behind = 뒤처진 = 이전")
- n 생략 = 1, 기본값 생략 = NULL → **중간 생략 불가** (`LAG(x,,0)` 오류, `LAG(x,1,0)`로)
- PARTITION 바뀌면 처음부터 → 각 묶음 첫 줄(LAG)·마지막 줄(LEAD)은 NULL(또는 기본값)
- `LAG … ORDER BY 월 DESC` = `LEAD … ORDER BY 월` (내가 찾은 것)
- 순서 함수라 **ROWS/RANGE 범위 못 씀**

예: 서울 1월 100 · 2월 150 · 3월 120 / 부산 1월 80 · 2월 90
| | 서1 | 서2 | 서3 | 부1 | 부2 |
| --- | --- | --- | --- | --- | --- |
| LAG(매출) | NULL | 100 | 150 | NULL | 80 |
| LEAD(매출, 1, 0) | 150 | 120 | **0** | 90 | **0** |

## 윈도우 함수 기본 개념
- 줄을 **합치지도 늘리지도 않음** (GROUP BY는 줄어듦, JOIN은 늘 수 있음)
- 구조: `OVER (PARTITION BY … ORDER BY … ROWS|RANGE BETWEEN … AND …)`
- 4분류: **순위**(RANK·DENSE_RANK·ROW_NUMBER) / **집계**(SUM·MAX·MIN·AVG·COUNT) / **순서**(FIRST_VALUE·LAST_VALUE·LAG·LEAD) / **비율**(RATIO_TO_REPORT·PERCENT_RANK·CUME_DIST·NTILE)
- 사용 위치: SELECT·ORDER BY ⭕ / WHERE·GROUP BY·HAVING ❌ / FROM 인라인 뷰 안 SELECT ⭕ → 바깥 WHERE로 거르기
- ORDER BY만 있으면 기본 범위 = **RANGE** UNBOUNDED PRECEDING ~ CURRENT ROW (ROWS와 같은 값 있으면 다름)
- 순위 함수·LAG·LEAD에는 ROWS/RANGE 범위 못 줌 / 윈도우 함수 중첩 불가

## 권한 (91번)
- WITH GRANT OPTION 받은 사용자는 옵션을 **붙이든 안 붙이든** 재허가 가능
- 객체 권한(WITH GRANT OPTION) REVOKE → **연쇄 취소** / 시스템 권한(WITH ADMIN OPTION) → 연쇄 안 됨
- 틀린 이유는 독해: "옵션과 함께 받았으면(조건) / 옵션 유무와 관계없이 허가(남에게 줄 때)" → 조건과 결과를 쪼개 읽기
