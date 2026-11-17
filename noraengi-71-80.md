# 노랭이 71~80 + ROLLUP 긴 보기 연습 (2026-11-17)

## 노랭이 질문
### 뷰 + NULL (76번)
```sql
CREATE VIEW v_tbl AS SELECT * FROM tbl WHERE c1 = 'b' OR c1 IS NULL;
SELECT SUM(c2) FROM v_tbl WHERE c2 >= 200 AND c1 = 'b';
```
- tbl: (a,100) (b,200) (b,100) (b,NULL) (NULL,200)
- 뷰: b 3줄 + (NULL,200) = 4줄 → 바깥 WHERE: (b,NULL)은 `NULL >= 200` 알 수 없음, (NULL,200)은 `NULL = 'b'` 알 수 없음 → **(b,200)만 → 200**
- 뷰는 저장된 SELECT. 뷰 WHERE + 바깥 WHERE 둘 다 적용. **IS NULL에선 통과, = 에선 탈락**

### 윈도우 범위 (77번)
| 열 | 뜻 |
| --- | --- |
| `ROW_NUMBER() OVER (PARTITION BY 부서 ORDER BY 연봉 DESC)` | 부서마다 연봉 순 1, 2, 3 (부서 바뀌면 1부터) |
| `SUM(연봉) OVER (PARTITION BY 부서 ORDER BY 사원id ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW)` | 부서마다 사원id 순 **누적** |
| `MAX(연봉) OVER (ORDER BY 연봉 DESC ROWS CURRENT ROW)` | 범위가 **자기 한 줄** → 항상 자기 연봉 |
- OVER마다 순서를 따로 정할 수 있음 (col1은 연봉 순, col2는 사원id 순)
- `UNBOUNDED PRECEDING AND CURRENT ROW` 누적 / `CURRENT ROW` 자기만 / `1 PRECEDING AND 1 FOLLOWING` 앞뒤 1줄 / `UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING` 전체

### 서브쿼리 + GROUP BY (79번)
- `WHERE sal >= (SELECT MAX(sal) FROM emp GROUP BY deptno)` → **오류**
- MAX는 하나지만 **GROUP BY가 붙으면 묶음(부서) 수만큼 줄** → `>=`(단일행)에 여러 줄
- 고치기: GROUP BY 빼기 / `>= ALL (…)` (최댓값 기준) / `>= ANY (…)` (최솟값 기준)

### 72번
- 메인 쿼리에 값을 제공하려고 주로 쓰는 건 **비연관** 서브쿼리 (연관 서브쿼리는 바깥 행을 받아 조건 확인, EXISTS)
- 다중행 연산자(IN 등)는 단일행 서브쿼리에도 쓸 수 있음

## ROLLUP 긴 보기 (78번 유형) 푸는 법
1. **GROUP BY 줄부터** → a (x,y) / b (x) / c (y) / d () 중 어떤 묶음인지 → **줄 수 결정**
   - ROLLUP(x, y) = a b d · CUBE = a b c d · GROUPING SETS(x, y) = b c · `x, ROLLUP(y)` = a b
2. 묶음별로 **NULL이 든 표**를 먼저 그림
3. **GROUPING(열) = 묶음에서 빠진 열이면 1, 들어 있으면 0**
4. `CASE GROUPING(열) WHEN 1 THEN '합계' ELSE 열 END` = 소계 NULL에 이름표만 붙임 (줄 수는 안 바뀜)
5. 거꾸로 풀기: 결과 표의 '합계' 라벨을 NULL로 되돌리면 묶음 구조가 보임 → 보기 GROUP BY와 맞추기
- SELECT에 GROUP BY에 없는 열(지역명)은 `MIN(지역명)`으로 감싸야 함 (안 그러면 오류)

### GROUPING vs NVL
- 원래 데이터에 NULL이 있으면: 데이터 NULL은 GROUPING = **0**, 소계 NULL은 **1** → 구분 가능
- NVL(열, '합계')는 NULL이면 무조건 바꿔서 데이터 NULL도 '합계'가 됨 → **결과 다름** ("단, NULL 없음" 조건이 있으면 같음)

## 연습 결과
- ROLLUP 긴 보기 6문항: 처음엔 1·2·4번 "완전히 모르겠다" → 작은 예제로 단계 설명 들은 뒤 **6/6** (6번은 풀이 맞고 번호 옮기다 실수)
  - 2번은 표 없이 라벨을 NULL로 되돌려서 풀었음
- 노랭이 71~80: 2개만 맞음 → 그중 76·77·78·79는 질문으로 해결

## 다음에 다시 볼 것
- [ ] 답 고른 뒤 보기 내용과 한 번 더 대조 (6번 번호 실수)
- [ ] GROUP BY 붙은 서브쿼리는 여러 줄 → 단일행 연산자 오류
- [ ] OVER의 ROWS 범위 읽기
