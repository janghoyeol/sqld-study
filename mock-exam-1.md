# SQLD 최종 모의고사 1회

- 문항 수: 50문항 (1과목 10 · 2과목 40), 4지선다
- **시험 시간: 90분**
- 합격 기준: 60점 이상 (문항당 2점), 과목별 40% 미만 과락 (1과목 4문항, 2과목 16문항 미만)
- 별도 언급이 없으면 DBMS는 Oracle 기준

시작 시각: ______  종료 시각: ______

---

## 공통 테이블 (2과목에서 사용)

**[EMP]**

| EMPNO | ENAME | JOB | MGR | SAL | COMM | DEPTNO |
| --- | --- | --- | --- | --- | --- | --- |
| 1001 | KING | PRESIDENT | NULL | 5000 | NULL | 10 |
| 1002 | BLAKE | MANAGER | 1001 | 2850 | NULL | 30 |
| 1003 | CLARK | MANAGER | 1001 | 2450 | NULL | 10 |
| 1004 | JONES | MANAGER | 1001 | 2975 | NULL | 20 |
| 1005 | SCOTT | ANALYST | 1004 | 3000 | NULL | 20 |
| 1006 | SMITH | CLERK | 1005 | 800 | NULL | 20 |
| 1007 | ALLEN | SALESMAN | 1002 | 1600 | 300 | 30 |
| 1008 | WARD | SALESMAN | 1002 | 1250 | 500 | 30 |
| 1009 | MARTIN | SALESMAN | 1002 | 1250 | 1400 | 30 |
| 1010 | MILLER | CLERK | 1003 | 1300 | NULL | NULL |

**[DEPT]**

| DEPTNO | DNAME | LOC |
| --- | --- | --- |
| 10 | ACCOUNTING | NEW YORK |
| 20 | RESEARCH | DALLAS |
| 30 | SALES | CHICAGO |
| 40 | OPERATIONS | BOSTON |

---

# 1과목 데이터 모델링의 이해

**1.** 데이터 모델링의 특징으로 적절하지 않은 것은?

① 추상화 ② 단순화 ③ 명확화 ④ 구체화

**2.** 데이터 모델링 단계에 대한 설명으로 가장 적절한 것은?

① 개념적 데이터 모델링은 특정 DBMS의 데이터 타입과 인덱스를 정의한다.
② 논리적 데이터 모델링 단계에서 정규화를 수행하며, 재사용성이 높다.
③ 물리적 데이터 모델링은 추상화 수준이 가장 높다.
④ 개념적 데이터 모델링은 업무 담당자와 의사소통하기 어려운 형태로 작성된다.

**3.** ANSI-SPARC 3층 스키마 중 외부 스키마에 대한 설명으로 가장 적절한 것은?

① 모든 사용자 관점을 통합한 조직 전체의 데이터베이스 구조
② 물리적 저장 장치 관점에서 본 데이터베이스 구조
③ 개별 사용자나 응용 프로그램이 바라보는 데이터베이스의 일부
④ 데이터 간의 관계와 제약조건을 모두 포함한 전체 구조

**4.** 엔터티의 특징으로 적절하지 않은 것은?

① 업무에서 필요하고 관리하고자 하는 정보여야 한다.
② 유일한 식별자에 의해 식별이 가능해야 한다.
③ 인스턴스가 하나 이상 존재하면 엔터티로 볼 수 있다.
④ 엔터티는 반드시 속성을 가져야 한다.

**5.** 다음 설명에 해당하는 엔터티 유형은?

> 업무를 수행함에 따라 발생하며, 비교적 발생량이 많고 각종 통계 자료에 이용될 수 있다.

① 유형 엔터티 ② 개념 엔터티 ③ 사건 엔터티 ④ 기본 엔터티

**6.** 속성에 대한 설명으로 적절하지 않은 것은?

① 하나의 인스턴스에서 하나의 속성은 한 개의 속성값만 가진다.
② 파생 속성은 데이터 정합성을 유지하기 위해 가급적 적게 정의하는 것이 좋다.
③ 설계 속성은 업무상 필요한 데이터 외에 설계 과정에서 새로 만든 속성으로, 코드나 일련번호 등이 있다.
④ 도메인은 엔터티 안에 존재할 수 있는 인스턴스 수의 범위를 말한다.

**7.** 다음 ERD(IE 표기법)에 대한 설명으로 가장 적절한 것은?

```
  [고객] ||———————o< [주문]
```

① 한 고객은 반드시 하나 이상의 주문을 가진다.
② 한 주문은 여러 고객에게 속할 수 있다.
③ 한 고객은 주문이 없을 수도 있다.
④ 한 주문은 고객이 없을 수도 있다.

**8.** 주식별자의 특징으로 적절하지 않은 것은?

① 유일성 ② 최소성 ③ 불변성 ④ 확장성

**9.** 식별 관계와 비식별 관계에 대한 설명으로 적절하지 않은 것은?

① 식별 관계에서는 부모 엔터티의 주식별자가 자식 엔터티 주식별자의 일부가 된다.
② 비식별 관계는 ERD에서 점선으로 표현한다.
③ 식별 관계만으로 계속 연결하면 하위 엔터티로 갈수록 주식별자 속성 수가 늘어난다.
④ SQL 문의 조인 횟수를 줄이려면 비식별 관계로 연결하는 것이 유리하다.

**10.** 다음 테이블은 최고 몇 정규형까지 만족하는가? (PK: 학번 + 과목코드, 과목코드가 정해지면 과목명이 정해진다)

| 학번 | 과목코드 | 성적 | 과목명 |
| --- | --- | --- | --- |
| 1001 | C01 | A | 데이터베이스 |
| 1001 | C02 | B | 인공지능 |
| 1002 | C01 | B | 데이터베이스 |

① 1정규형 ② 2정규형 ③ 3정규형 ④ BCNF

---

# 2과목 SQL 기본 및 활용

**11.** SELECT 문의 논리적 실행 순서로 가장 적절한 것은?

① FROM → WHERE → SELECT → GROUP BY → HAVING → ORDER BY
② FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
③ SELECT → FROM → WHERE → GROUP BY → HAVING → ORDER BY
④ FROM → GROUP BY → WHERE → HAVING → SELECT → ORDER BY

**12.** 다음 SQL의 결과로 가장 적절한 것은?

```sql
SELECT COUNT(*), COUNT(COMM), COUNT(DEPTNO) FROM EMP;
```

① 10, 3, 9 ② 10, 10, 10 ③ 10, 3, 10 ④ 3, 3, 9

**13.** 다음 중 결과가 나머지와 다른 하나는?

① `SELECT AVG(COMM) FROM EMP;`
② `SELECT SUM(COMM) / COUNT(COMM) FROM EMP;`
③ `SELECT AVG(NVL(COMM, 0)) FROM EMP;`
④ `SELECT SUM(COMM) / COUNT(*) FROM EMP WHERE COMM IS NOT NULL;`

**14.** 다음 SQL의 결과 행 수는?

```sql
SELECT ENAME FROM EMP
WHERE DEPTNO = 10 OR DEPTNO = 20 AND SAL >= 3000;
```

① 1 ② 2 ③ 5 ④ 3

**15.** 다음 SQL의 결과 행 수는?

```sql
SELECT ENAME FROM EMP WHERE ENAME LIKE '_A%';
```

① 2 ② 1 ③ 3 ④ 4

**16.** 다음 SQL의 결과로 가장 적절한 것은?

```sql
SELECT SUBSTR('SQLDEVELOPER', 4, 3), LENGTH(TRIM('  AB C  ')) FROM DUAL;
```

① LDE, 4 ② DEV, 4 ③ DEV, 6 ④ EVE, 4

**17.** 다음 SQL의 결과로 가장 적절한 것은?

```sql
SELECT ROUND(156.789, 1), TRUNC(156.789, -1), MOD(17, 5) FROM DUAL;
```

① 156.8, 150, 2 ② 156.7, 150, 2 ③ 156.8, 160, 2 ④ 156.8, 150, 3

**18.** 다음 SQL의 결과로 가장 적절한 것은?

```sql
SELECT TO_CHAR(TO_DATE('2026-11-20 10:00', 'YYYY-MM-DD HH24:MI') + 1/24*3,
               'YYYY-MM-DD HH24:MI')
FROM DUAL;
```

① 2026-11-20 10:03 ② 2026-11-23 10:00 ③ 2026-11-20 10:30 ④ 2026-11-20 13:00

**19.** 다음 SQL의 결과에서 GRADE 값이 'C'인 행의 수는?

```sql
SELECT ENAME, DECODE(DEPTNO, 10, 'A', 20, 'B', 'C') AS GRADE FROM EMP;
```

① 4 ② 6 ③ 0 ④ 5

**20.** 다음 중 결과가 나머지와 다른 하나는?

① `NVL(NULL, 0)`
② `COALESCE(NULL, NULL, 0)`
③ `NULLIF(0, 0)`
④ `NVL2(NULL, 1, 0)`

**21.** 다음 중 오류가 발생하는 SQL은?

① `SELECT DEPTNO, SUM(SAL) FROM EMP GROUP BY DEPTNO ORDER BY SUM(SAL);`
② `SELECT DEPTNO, SUM(SAL) AS TOT FROM EMP GROUP BY DEPTNO HAVING TOT > 5000;`
③ `SELECT DEPTNO FROM EMP GROUP BY DEPTNO HAVING MAX(SAL) > 3000;`
④ `SELECT JOB, COUNT(*) FROM EMP WHERE SAL > 1000 GROUP BY JOB;`

**22.** 다음 SQL의 결과 순서로 가장 적절한 것은?

```sql
SELECT ENAME FROM EMP
WHERE DEPTNO IS NULL OR DEPTNO = 10
ORDER BY DEPTNO DESC, SAL;
```

① CLARK, KING, MILLER
② KING, CLARK, MILLER
③ MILLER, KING, CLARK
④ MILLER, CLARK, KING

**23.** 다음 SQL의 결과에 대한 설명으로 가장 적절한 것은?

```sql
SELECT D.DNAME, COUNT(E.EMPNO)
FROM DEPT D, EMP E
WHERE D.DEPTNO = E.DEPTNO(+)
GROUP BY D.DNAME;
```

① 결과는 3행이다.
② 결과는 4행이며, OPERATIONS의 COUNT 값은 1이다.
③ 결과는 4행이며, OPERATIONS의 COUNT 값은 0이다.
④ 결과는 5행이다.

**24.** 다음 SQL의 결과 값은?

```sql
SELECT COUNT(*)
FROM EMP E FULL OUTER JOIN DEPT D ON E.DEPTNO = D.DEPTNO;
```

① 9 ② 10 ③ 14 ④ 11

**25.** 다음 중 오류가 발생하는 SQL은?

① `SELECT E.ENAME, E.DEPTNO FROM EMP E JOIN DEPT D USING (DEPTNO);`
② `SELECT E.ENAME, DEPTNO FROM EMP E JOIN DEPT D USING (DEPTNO);`
③ `SELECT ENAME, DNAME FROM EMP NATURAL JOIN DEPT;`
④ `SELECT E.ENAME, D.DNAME FROM EMP E CROSS JOIN DEPT D;`

**26.** 다음 SQL의 결과 값은?

```sql
SELECT COUNT(*) FROM DEPT
WHERE DEPTNO NOT IN (SELECT DEPTNO FROM EMP);
```

① 0 ② 1 ③ 3 ④ 4

**27.** 다음 SQL의 결과로 가장 적절한 것은?

```sql
SELECT DNAME FROM DEPT D
WHERE NOT EXISTS (SELECT 1 FROM EMP E WHERE E.DEPTNO = D.DEPTNO);
```

① 결과 없음
② OPERATIONS
③ OPERATIONS, NULL
④ 오류 발생

**28.** 다음 SQL의 결과 행 수는?

```sql
SELECT ENAME FROM EMP E
WHERE SAL > (SELECT AVG(SAL) FROM EMP WHERE DEPTNO = E.DEPTNO);
```

① 4 ② 3 ③ 5 ④ 6

**29.** 다음 SQL의 결과 행 수는?

```sql
SELECT ENAME FROM EMP
WHERE SAL > ALL (SELECT SAL FROM EMP WHERE DEPTNO = 30);
```

① 1 ② 3 ③ 7 ④ 9

**30.** 서브쿼리에 대한 설명으로 적절하지 않은 것은?

① SELECT 절의 스칼라 서브쿼리는 한 행, 한 칼럼만 반환해야 한다.
② FROM 절에 사용하는 서브쿼리를 인라인 뷰라고 한다.
③ 단일 행 서브쿼리가 여러 행을 반환하면 = 비교에서 오류가 발생한다.
④ 인라인 뷰에서는 ORDER BY 절을 사용할 수 없다.

**31.** 다음 두 테이블에 대해 집합 연산을 수행했을 때, 연산과 결과 행 수의 연결로 적절하지 않은 것은?

| A.X |
| --- |
| 1 |
| 2 |
| 2 |
| 3 |
| NULL |

| B.X |
| --- |
| 2 |
| 3 |
| 3 |
| NULL |
| 4 |

① `SELECT X FROM A UNION ALL SELECT X FROM B` – 10행
② `SELECT X FROM A UNION SELECT X FROM B` – 5행
③ `SELECT X FROM A INTERSECT SELECT X FROM B` – 2행
④ `SELECT X FROM A MINUS SELECT X FROM B` – 1행

**32.** 다음 SQL의 결과 행 수는?

```sql
SELECT DEPTNO, JOB, SUM(SAL)
FROM EMP
WHERE DEPTNO IS NOT NULL
GROUP BY ROLLUP (DEPTNO, JOB);
```

① 7 ② 11 ③ 10 ④ 13

**33.** 다음 SQL의 결과 행 수는?

```sql
SELECT DEPTNO, JOB, SUM(SAL)
FROM EMP
WHERE DEPTNO IS NOT NULL
GROUP BY GROUPING SETS (DEPTNO, JOB);
```

① 8 ② 5 ③ 11 ④ 16

**34.** 다음 SQL 결과의 마지막 행으로 가장 적절한 것은?

```sql
SELECT CASE GROUPING(DEPTNO) WHEN 1 THEN '전체' ELSE TO_CHAR(DEPTNO) END AS 부서,
       SUM(SAL) AS 합계
FROM EMP
WHERE DEPTNO IS NOT NULL
GROUP BY ROLLUP (DEPTNO)
ORDER BY GROUPING(DEPTNO), DEPTNO;
```

① NULL, 21175
② 전체, 21175
③ 전체, 22475
④ 30, 6950

**35.** 다음 SQL의 결과에서 BLAKE 행의 R, D 값은?

```sql
SELECT ENAME,
       RANK()       OVER (ORDER BY SAL) AS R,
       DENSE_RANK() OVER (ORDER BY SAL) AS D
FROM EMP
WHERE DEPTNO = 30;
```

① 4, 4 ② 4, 3 ③ 3, 3 ④ 3, 4

**36.** 다음 SQL의 결과에서 WARD 행의 CUM 값은?

```sql
SELECT ENAME, SAL, SUM(SAL) OVER (ORDER BY SAL) AS CUM
FROM EMP
WHERE DEPTNO = 30;
```

① 1250 ② 3750 ③ 2500 ④ 4100

**37.** 다음 SQL의 결과에서 JONES 행의 CUM 값은?

```sql
SELECT ENAME,
       SUM(SAL) OVER (PARTITION BY DEPTNO ORDER BY EMPNO
                      ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS CUM
FROM EMP
WHERE DEPTNO IN (10, 20);
```

① 2975 ② 10425 ③ 5975 ④ 9950

**38.** 다음 SQL의 결과에서 JONES, SMITH 행의 PREV 값은?

```sql
SELECT ENAME, LAG(SAL, 1, 0) OVER (ORDER BY EMPNO) AS PREV
FROM EMP
WHERE DEPTNO = 20;
```

① 0, 3000 ② NULL, 3000 ③ 0, 2975 ④ 2975, 800

**39.** 다음 SQL의 결과에서 G 값이 1인 행의 수는?

```sql
SELECT ENAME, NTILE(3) OVER (ORDER BY SAL DESC) AS G FROM EMP;
```

① 3 ② 2 ③ 5 ④ 4

**40.** 다음 중 결과가 0건인 SQL은?

① `SELECT * FROM EMP WHERE ROWNUM <= 3;`
② `SELECT * FROM EMP WHERE ROWNUM = 1;`
③ `SELECT * FROM EMP WHERE ROWNUM = 2;`
④ `SELECT * FROM EMP WHERE ROWNUM < 2;`

**41.** 다음 SQL의 결과에서 LEVEL 값이 3인 행의 수는?

```sql
SELECT LEVEL, ENAME
FROM EMP
START WITH MGR IS NULL
CONNECT BY PRIOR EMPNO = MGR;
```

① 5 ② 3 ③ 4 ④ 6

**42.** 다음 두 SQL의 결과 행 수를 순서대로 나열한 것은?

```sql
-- (가)
SELECT ENAME FROM EMP
START WITH MGR IS NULL
CONNECT BY PRIOR EMPNO = MGR AND ENAME <> 'JONES';

-- (나)
SELECT ENAME FROM EMP
WHERE ENAME <> 'JONES'
START WITH MGR IS NULL
CONNECT BY PRIOR EMPNO = MGR;
```

① 7, 9 ② 9, 7 ③ 7, 7 ④ 9, 9

**43.** 다음 SQL의 결과에 대한 설명으로 가장 적절한 것은?

```sql
SELECT *
FROM (SELECT DEPTNO, JOB, SAL FROM EMP WHERE DEPTNO IN (10, 20))
PIVOT (SUM(SAL) FOR JOB IN ('MANAGER' AS MGR, 'CLERK' AS CLK));
```

① 결과는 2행이며, DEPTNO 10의 CLK 값은 NULL이다.
② 결과는 2행이며, DEPTNO 10의 CLK 값은 0이다.
③ 결과는 4행이다.
④ IN 절에 없는 JOB(PRESIDENT, ANALYST)의 급여는 MGR 칼럼에 더해진다.

**44.** 다음 SQL의 결과로 가장 적절한 것은?

```sql
SELECT REGEXP_SUBSTR('abc123def45', '[0-9]+') FROM DUAL;
```

① 12345 ② 123 ③ 45 ④ 1

**45.** 테이블 T와 S가 다음과 같을 때, MERGE 수행 후 `SELECT SUM(VAL) FROM T;`의 결과는?

| T.ID | T.VAL |
| --- | --- |
| 1 | 10 |
| 2 | 20 |

| S.ID | S.VAL |
| --- | --- |
| 2 | 25 |
| 3 | 30 |

```sql
MERGE INTO T
USING S ON (T.ID = S.ID)
WHEN MATCHED THEN UPDATE SET T.VAL = S.VAL
WHEN NOT MATCHED THEN INSERT (ID, VAL) VALUES (S.ID, S.VAL);
```

① 55 ② 60 ③ 65 ④ 85

**46.** 비어 있는 테이블 T(C NUMBER)에 다음 SQL을 순서대로 수행한 후 `SELECT COUNT(*) FROM T;`의 결과는?

```sql
INSERT INTO T VALUES (1);
SAVEPOINT SP1;
INSERT INTO T VALUES (2);
SAVEPOINT SP2;
INSERT INTO T VALUES (3);
ROLLBACK TO SP1;
INSERT INTO T VALUES (4);
COMMIT;
```

① 1 ② 3 ③ 4 ④ 2

**47.** EMP.DEPTNO가 DEPT.DEPTNO를 참조하는 외래키이며 `ON DELETE SET NULL` 옵션이 지정되어 있다. 다음 SQL 수행 후 `SELECT COUNT(*) FROM EMP WHERE DEPTNO IS NULL;`의 결과는?

```sql
DELETE FROM DEPT WHERE DEPTNO = 30;
```

① 1 ② 4 ③ 5 ④ 0

**48.** Oracle에서 EMP 테이블의 ENAME 칼럼 데이터 타입을 VARCHAR2(30)으로 변경하는 SQL로 가장 적절한 것은?

① `ALTER TABLE EMP ALTER COLUMN ENAME VARCHAR2(30);`
② `ALTER TABLE EMP MODIFY (ENAME VARCHAR2(30));`
③ `ALTER TABLE EMP CHANGE ENAME VARCHAR2(30);`
④ `MODIFY TABLE EMP (ENAME VARCHAR2(30));`

**49.** 다음 SQL을 순서대로 수행했을 때의 설명으로 가장 적절한 것은?

```sql
-- 사용자 A (EMP 테이블 소유자)
GRANT SELECT ON EMP TO B WITH GRANT OPTION;
-- 사용자 B
GRANT SELECT ON A.EMP TO C;
-- 사용자 A
REVOKE SELECT ON EMP FROM B;
```

① C의 SELECT 권한은 그대로 유지된다.
② B와 C 모두 SELECT 권한을 잃는다.
③ B만 권한을 잃고, C의 권한은 A가 따로 REVOKE해야 한다.
④ WITH GRANT OPTION을 받은 B는 C에게 권한을 줄 수 없다.

**50.** SQL 관리 구문에 대한 설명으로 적절하지 않은 것은?

① TRUNCATE는 테이블 구조는 남기고 데이터를 모두 삭제하며, Oracle에서 ROLLBACK할 수 없다.
② DELETE는 WHERE 절로 조건에 맞는 일부 행만 삭제할 수 있다.
③ Oracle에서 DDL 문을 실행하면 자동으로 COMMIT된다.
④ UNIQUE 제약조건이 지정된 칼럼에는 NULL을 입력할 수 없다.

---

## 답안지

| 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| | | | | | | | | | |

| 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| | | | | | | | | | |

| 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| | | | | | | | | | |

| 31 | 32 | 33 | 34 | 35 | 36 | 37 | 38 | 39 | 40 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| | | | | | | | | | |

| 41 | 42 | 43 | 44 | 45 | 46 | 47 | 48 | 49 | 50 |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| | | | | | | | | | |
