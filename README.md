# SQLD 공부 기록

목표: 2027년 3월 SQLD 합격
방식: 가상 회사 DB(Tucanova, PostgreSQL)에서 직접 쿼리 돌려 보며 "예상 먼저 → 실행 → 왜 달랐는지"
시험 문법은 Oracle 기준 → Oracle 전용 문법은 따로 표시

| 주차 | 범위 | 기록 |
| --- | --- | --- |
| 1주차 | 실행 순서 · GROUP BY/HAVING · NULL · JOIN · 서브쿼리 · 집합 연산 | [week1.md](week1.md) |
| 실전 연습 | 실제 데이터로 비율·평균·객단가 (분모 함정) | [practice.md](practice.md) |
| 함수 | 문자·날짜·CASE·DECODE·NULL 함수 (노랭이 질문) | [functions.md](functions.md) |
| 노랭이 1~60 | 질문 노트 · 긴 지문 연습 · 집합 연산·NULL 정리 | [noraengi.md](noraengi.md) |
| SQL 활용 | ROLLUP/CUBE · 윈도우 · Top N · 계층형 · PIVOT · 정규식 | [sql-utilization.md](sql-utilization.md) |
| 노랭이 61~70 | 계층형 조건 위치 · 셀프 조인 누적 · 서브쿼리 연결 조건 | [noraengi-61-70.md](noraengi-61-70.md) |
| 노랭이 71~80 | 뷰+NULL · 윈도우 범위 · ROLLUP 긴 보기 푸는 법 · GROUPING vs NVL | [noraengi-71-80.md](noraengi-71-80.md) |
| 노랭이 81~98 | CUBE 괄호 덩어리 · LAG/LEAD · 윈도우 기본 개념 · 권한 | [noraengi-81-98.md](noraengi-81-98.md) |
| 관리 구문 | DML·DDL·DCL·TCL · 제약조건 · FK 참조 동작 · MERGE · 트랜잭션 | [management.md](management.md) |

## 점수 변화
| 날짜 | 시험 | 결과 |
| --- | --- | --- |
| 11-09 | 진단 12문항 | 3/12 |
| 11-10 | 1주차 실전 12문항 | 9/12 |
| 11-11 | 1주차 복습 10문항 (13분) | 8/10 |
| 11-12 | 실전 10문항 | 7/10 |
| 11-14 | 긴 지문 10문항 (40분) | 7/10 |
| 11-14 | 긴 지문 추가 8문항 | 5/8 |
| 11-15 | 집합 연산 긴 지문 8문항 | 6/8 |
| 11-15 | 노랭이 51~60 (SQL 활용) | 6/10 |
| 11-16 | 51~60 맞춤 11문항 (33분) | 10/11 |
| 11-16 | 61~70 맞춤 10문항 (35분) | 6/10 |
| 11-17 | 노랭이 71~80 | 2/10 |
| 11-17 | ROLLUP 긴 보기 6문항 (설명 후) | 6/6 |
| 11-18 | 윈도우 기본 O/X | 4/6 |
| 11-18 | 관리 구문 맞춤 20문항 (25분) | 17/20 |
