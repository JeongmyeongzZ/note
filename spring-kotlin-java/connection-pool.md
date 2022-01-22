# 커넥션 풀이란

> 웹 컨테이너(WAS)가 실행되면서 DB와 미리 connection(연결)을 해놓은 객체들을 pool에 저장해두었다가 클라이언트 요청이 오면 connection을 빌려주고, 처리가 끝나면 다시 connection을 반납받아 pool에 저장하는 방식

- 물리적인 데이터베이스 connection(연결) 부하를 줄이고 연결 관리 한다.
- pool에 미리 connection이 생성되어 있기 때문에 connection을 생성하는 데 연결 시간이 소비되지 않는다.
- 커넥션을 계속해서 재사용하기 때문에 생성되는 커넥션 수를 제한적으로 설정할 수 있다.

PHP 에서도 `DB.getConnect()`  와 같은 코드를 찾아 볼 수 있다. 물론 커넥션을 종료하는 DB.closeConnect() 와 같은 코드는 보기 힘들다. (요청당  프로세스가 실행되고 종료되니..)
이런 인터프리터방식의 스크립트 언어인 PHP 는 커넥션 풀 개념을 찾아볼 수 없다. 프로세스가 살아있고 그 안에서 요청당 커넥션을 관리해주는 개념은 컴파일언어에선 사실 당연한 개념이다.


---

## JDBC (Java DataBase Connectivity)

> 자바에서 데이터베이스와 관련된 작업을 처리할때 사용하는 API

DBMS 종류(MySql, MsSql, Oracle...)에 상관 없이 하나의 JDBC API를 사용해서 데이터베이스 작업을 처리할수 있다. (Spring PSA)

Application <=> JDBC API <=> JDBC Driver <=> DB


1. JDBC 드라이버 LOAD
↓
2. Connection 객체 생성
↓
3. Statement 객체 생성
↓
4. Query 실행
↓
5. Result 객체로부터 데이터 추출
↓
6. Result 객체 Close
↓
7. Statement 객체 Close
↓
8. Connection 객체 Close

- Connection : DB 연결 객체
- Statement 또는 PreparedStatment 객체 : SQL 문 실행 객체
- ResultSet 객체 : select 문 결과를 가지는 객체


_참고_
- _https://kimvampa.tistory.com/44_


