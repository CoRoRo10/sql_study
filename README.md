## SQL 기본

### SQL 문의 종류

---

<table>
  <tr> <th/>종류 <th/>구문 <th/>설명 </tr>
  <tr>
    <td rowspan="5">DML</td>
    <td>SELECT</td>
    <td rowspan="5"> 테이블에 저장된 데이터를 조작(조회, 입력, 수정, 삭제)하기 위한 구문</td>
  </tr>
  <tr> <td>INSERT</td> </tr>
  <tr> <td>UPDATE</td> </tr>
  <tr> <td>DELETE</td> </tr>
  <tr> <td>MERGE</td> </tr>

  <tr>
    <td rowspan="3">TCL</td>
    <td>COMMIT</td>
    <td rowspan="3"> DML문에 의한 데이터의 변경 사항을 데이터베이스에 영구히 반영하거나 취소하기 위해 Transaction을 제어하는 구문</td>
  </tr>
  <tr> <td>ROLLBACK</td> </tr>
  <tr> <td>SAVEPOINT</td> </tr>

  <tr>
    <td rowspan="5">DDL</td>
    <td>CREATE</td>
    <td rowspan="5">테이블, 인덱스와 같은 데이터베이스 오브젝트의 구조를 정의(생성, 변경, 삭제)하기 위한 구문</td>
  </tr>
  <tr> <td>ALTER</td> </tr>
  <tr> <td>DROP</td> </tr>
  <tr> <td>RENAME</td> </tr>
  <tr> <td>TRUNCATE</td> </tr>

  <tr>
    <td rowspan="2">DCL</td>
    <td>GRANT</td>
    <td rowspan="2">데이터에 대한 권한을 부여하거나 취소하기 위한 구문</td>
  </tr>
  <tr> <td>REVOKE</td> </tr>
</table>

<br>

> 기억 )  
> 데이터베이스에서 user 및 user에 대한 권한을 부여하는 SQL 문장들의 종류를 DATA CONTROL LANGUAGE, 데이터 제어어 라고 한다. CREATE USER 문을 통해 새로운 user를 생성하고 DROP USER 문을 통해 기존 user를 삭제할 수 있다. GRANT 문을 통해 user에게 시스템 권한 및 객체 권한을 부여할 수 있으며, 반태로 REVOKE 문을 통해 부여했던 권한을 회수할 수도 있다. 또한 권한들의 모음인 ROLE을 생성(CREATE ROLE)하거나 삭제(DROP ROLE)할 수도 있다.

<br>

```
DCL 예시
---------
CREATE USER NEW_USER IDENTIFIED BY 1234;
CREATE ROLE RL_BASE;
GRANT CONNECT, RESOURCE TO RL_BASE;
GRANT RL_BASE TO NEW_USER;
```

<br>

### QUERY 순서

---

> 문법 작성 순서

1. SELECT
2. FROM
3. WHERE
4. GROUP BY
5. HAVING
6. ORDER BY

<br>

> 실행 순서

1. FROM
2. ON
3. JOIN
4. WHERE
5. GROUP BY
6. CUBE | ROLLUP
7. HAVING
8. SELECT
9. DISTINCT
10. ORDER BY
11. TOP

```
자주 사용 => FROM - WHERE - GROUP BY - HAVING - SELECT - ORDER BY
```

<br>

### NULL 관련 함수

---

**NVL** | NVL 함수는 A가 null이 아니면 A, null이면 B를 반환한다.

```
NVL(A,B)
```

**NULLIF** | NULLIF 함수는 A와 B가 다르면 A, 같으면 null을 반환한다.

```
NULLIF(A,B)
```

<br>

### 조건 우선순위

---

조건은 우선순위에 따라 평가된다.  
| 우선순위 | 조건 |
| :--- | :--- |
| 1 | 연산자 |
| 2 | 비교 조건(=, <>, >, <, >=, <=>) |
| 3 | IN 조건, LIKE 조건, BETWEEN 조건, NULL 조건|
| 4 | 논리 조건(NOT) |
| 5 | 논리 조건(AND) |
| 6 | 논리 조건(OR) |
<br>

> 주의 | AND와 OR 조건

```
SELECT
    SUM(COL_C) AS A1
FROM TA
WHERE COL_A >= 10
    OR COL_B IN (1, 2)
    AND COL_C NOT BETWEEN 500 AND 600;
```

위 코드는 조건 우선순위에 따라 아래와 같이 순서가 정해진다.

```
SELECT
    SUM(COL_C) AS A1
FROM TA
WHERE COL_A >= 10
    OR ( COL_B IN (1, 2)
    AND COL_C NOT BETWEEN 500 AND 600 );
```

<br>

### 그룹 함수

---

GROUP BY 절에 사용하는 함수는 전체 합계와 동시에 그룹 별 합계(소그룹의 집계)를 구할 수 있다.

#### 1) ROLLUP

- GROUP BY ROLLUP(col1)
  <br>전체 합계, 컬럼 col1 소계
  <br><br>
- GROUP BY ROLLUP(col1, col2)
  <br>전체 합계, 컬럼 col1 소계, 컬럼 col1, col2 조합 소계
  <br><br>
- GROUP BY ROLLUP(col1,col2,col3)
  <br>전체 합계, 컬럼 col1 소계, 컬럼 col1, col2 조합 소계, 컬럼 col1, col2, col3 조합 소계
  <br><br>
- GROUP BY ROLLUP(col1, (col2,col3))
  <br>전체 합계, 컬럼 col1 소계, 컬럼 col1, (col2, col3) 조합 소계

#### 2) CUBE

- GROUP BY CUBE(col1)
  <br>전체 합계, 컬럼 col1 소계
  <br><br>
- GROUP BY CUBE(col1, col2)
  <br>전체 합계, 컬럼 col1 소계, 컬럼 col2 소계, 컬럼 col1, col2 조합 소계
  <br><br>
  > 가능한 모든 조합의 소계 및 합계를 생성하기 때문에 시스템에 무리가 갈 수 있다.

#### 3) GROUPING SETS

- GROUP BY GROUPING SETS(col1)
  <br>컬럼 col1 소계
  <br><br>
- GROUP BY GROUPING SETS(col1, col2)
  <br>컬럼 col1 소계, 컬럼 col2 소계
  <br><br>

### ROW LIMITING 절

---

| 항목      | 설명                                    |
| :-------- | :-------------------------------------- |
| OFFSET    | 건너뛸 행의 개수를 지정                 |
| FETCH     | 반환할 행의 개수나 백분율을 지정        |
| ONLY      | 지정된 행의 개수나 백분율만큼 행을 반환 |
| WITH TIES | 마지막 행에대한 동순위를 포함해서 반환  |

<br><br>

> 참고

- youtube 쏭즈캠퍼스 https://www.youtube.com/channel/UC7Ax79mT-s1qErBZnH0Kk-A
- 책 국가공인 SQLD 자격검정 핵심노트 (디비안)
