## SQL 기본

#### SQL 문의 종류

---

<table>
  <tr> <th/>종류 <th/>구문 <th/>설명 </tr>
  <tr>
    <td rowspan="5">DML 문</td>
    <td>SELECT</td>
    <td rowspan="5"> 테이블에 저장된 데이터를 조작(조회, 입력, 수정, 삭제)하기 위한 구문</td>
  </tr>
  <tr> <td>INSERT</td> </tr>
  <tr> <td>UPDATE</td> </tr>
  <tr> <td>DELETE</td> </tr>
  <tr> <td>MERGE</td> </tr>

  <tr>
    <td rowspan="3">TCL 문</td>
    <td>COMMIT</td>
    <td rowspan="3"> DML문에 의한 데이터의 변경 사항을 데이터베이스에 영구히 반영하거나 취소하기 위해 Transaction을 제어하는 구문</td>
  </tr>
  <tr> <td>ROLLBACK</td> </tr>
  <tr> <td>SAVEPOINT</td> </tr>

  <tr>
    <td rowspan="5">DDL 문</td>
    <td>CREATE</td>
    <td rowspan="5">테이블, 인덱스와 같은 데이터베이스 오브젝트의 구조를 정의(생성, 변경, 삭제)하기 위한 구문</td>
  </tr>
  <tr> <td>ALTER</td> </tr>
  <tr> <td>DROP</td> </tr>
  <tr> <td>RENAME</td> </tr>
  <tr> <td>TRUNCATE</td> </tr>

  <tr>
    <td rowspan="2">DCL 문</td>
    <td>GRANT</td>
    <td rowspan="2">데이터에 대한 권한을 부여하거나 취소하기 위한 구문</td>
  </tr>
  <tr> <td>REVOKE</td> </tr>
</table>

<br>

> 기억 )
> 데이터베이스에서 user 및 user에 대한 권한을 부여하는 SQL 문장들의 종류를 DATA CONTROL LANGUAGE, 데이터 제어어 라고 한다. CREATE USER 문을 통해 새로운 user를 생성하고 DROP USER 문을 통해 기존 user를 삭제할 수 있다. GRANT 문을 통해 user에게 시스템 권한 및 객체 권한을 부여할 수 있으며, 반태로 REVOKE 문을 통해 부여했던 권한을 회수할 수도 있다. 또한 권한들의 모음인 ROLE을 생성(CREATE ROLE)하거나 삭제(DROP ROLE)할 수도 있다.

<BR>

```
DCL 예시
---------
CREATE USER NEW_USER IDENTIFIED BY 1234;
CREATE ROLE RL_BASE;
GRANT CONNECT, RESOURCE TO RL_BASE;
GRANT RL_BASE TO NEW_USER;
```
