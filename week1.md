# 1주차 (2026-11-09 ~ 11-11)

## 1일 · 실행 순서
- 쓰는 순서: SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY
- **실행 순서: FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY**
- SELECT 별칭(alias)은 WHERE·HAVING에서 못 씀 / ORDER BY에서는 씀
- WHERE에는 집계 함수(count·sum·avg…) 못 씀 → 묶음 조건은 HAVING (= 묶음에 거는 WHERE)
- GROUP BY 하면 SELECT에는 **GROUP BY 열 + 집계 함수만**
  - stat_cd로 묶었는데 ord_no를 같이 보면? 60번 묶음 안에 ord_no가 수만 개 → 뭘 보여줄지 모름 → 오류
- WHERE 안 서브쿼리는 바깥 WHERE가 값이 필요할 때 실행, 괄호 안도 같은 실행 순서

## 2일 · NULL과 집계
- `count(*)` = 줄 수 / `count(열)` = 그 열에 값이 있는 줄 수
- `where x = null` → 오류 아님, 0행 (NULL = NULL 은 "알 수 없음", WHERE는 참만 통과) → `is null`
- 평균 세 가지: avg(x) ≠ sum(x)/count(*) = avg(coalesce(x,0))  ← **분모 차이**
- NULL도 GROUP BY에서 한 묶음 (ctgr_cd NULL 610개 → count(*) 610, count(ctgr_cd) 0)
- Oracle: `NVL(x, 0)` = coalesce

## 3일 · JOIN
- INNER: 짝 있는 것만 / LEFT: 왼쪽 전부 + 짝 없으면 NULL / FULL: 양쪽 전부 / CROSS: 모든 조합(5 × 8 = 40, 카티션 곱)
- prd LEFT JOIN ctgr > INNER (카테고리 없는 상품만큼)
- ctgr 쪽 짝 없음 = 0행 → "없다"가 답 (쿼리가 틀린 게 아님)
- **FULL = LEFT + RIGHT − INNER** (예: 12 + 15 − 10 = 17)
- Oracle `(+)`: 붙은 쪽 = 없어도 되는 쪽 → **반대편이 기준**

## 4일 · 서브쿼리
- 평균보다 비싼 품목이 절반보다 적은 건 이 데이터(비싼 소수가 평균을 끌어올림)라서 → 중앙값을 쓰는 이유
- 단일행(=, >, <) vs 다중행(IN, ANY, ALL) — `= (여러 줄)`은 오류, IN은 언제나 안전
- EXISTS: 결과 **행이 있는지만** 봄 → select 1, 0, 100 다 같음 / 바깥 열 참조 = 연관 서브쿼리
- **NOT IN 괄호 안에 NULL 하나만 있어도 결과 0** (오류 아님!) → NOT EXISTS 또는 is not null
- 서브쿼리 안 ORDER BY: WHERE 안 ❌ / FROM 안(인라인 뷰) ⭕ (Oracle)

## 5일 · 집합 연산
- UNION ALL = 중복 그대로 (99,471 + 3,095 = 102,566) / UNION = 중복 제거 (27개 주)
- INTERSECT 23 / 고객만 있는 주 4 / 판매자만 있는 주 0 → 27 = 23 + 27 − 23
- EXCEPT(PostgreSQL) = MINUS(Oracle), **앞 표 − 뒤 표** (순서 바꾸면 답이 다름)
- 위아래 열 개수 같아야 함, 열 이름은 첫 SELECT, ORDER BY는 맨 끝 한 번
- 중복 제거(UNION·DISTINCT·GROUP BY)에서는 NULL끼리 같은 것으로 침 → `select null union select null` = 1행

## 틀린 것 / 다시 볼 것
- [ ] GROUP BY 결과 행 수 = **묶음(조합) 개수**, count(*)는 묶음 안에서 셈 (복습 4번)
- [ ] NULL 계산은 0이 아니라 NULL (`1 + NULL = NULL`, 전부 NULL이면 SUM도 NULL)
- [x] `(+)` 방향 (1차 틀림 → 복습 맞음)
- [x] NOT IN + NULL = 0 (1차 틀림 → 복습 NOT EXISTS 맞음)
- [ ] MINUS는 표 순서 짚고 풀기
