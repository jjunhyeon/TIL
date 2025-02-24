

## 📌 ** 목적 **

### 기본적인 내용이지만 용어를 혼란스럽게 사용하는것을 방지하고 명확히 개념을 다잡기 위해 정리합니다.


## 📌 ** 내용 **

1. DML(Data Maniulation Language) : 데이터 조작어

* Purpose 
기본적인 내용이지만 용어를 혼란스럽게 사용하는것을 방지하고 명확히 개념을 다잡기 위해 정리합니다.

1. DML(Data Maniulation Language) : 데이터 조작(Manipulation)어, Manipulation 이라는 단어를 생각
- SELECT : 데이터베이스의 데이터를 조회하거나 검색하기 위한 명령어
- INSERT,UPDATE,DELETE : 데이터베이스의 테이블에 들어 있는 데이터에 변형을 가하는 종류(데이터 삽입, 수정 , 삭제) 의 명령어

=> 흔히 SQL이라고도 하며 개발자 레벨에서 가장 많이 접하게 되는 명령어이다.


2. DDL(Data Definition Language) : 데이터 정의(Definition)어
- CREATE, ALTER, DROP, RENAME, TRUNCATE : 테이블과 같은 데이터 구조를 정의하는데 사용되는 명령어로(생성, 변경, 삭제) 와 같은 
데이터 구조와 관련된 명령어들을 말함

3. DCL(Data Control Language) : 데이터베이스에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령어들을 말한다.
- Grant
 : ex) Grant SELECT,INSERT,UPDATE,DELETE ON A.TABLE TO developer;
- Revoke
 : ex) Revoke SELECT, INSER, UPDATE, DELETE ON A.TABLE TO developer;

4. TCL(Trnsaction Control Language) : 논리적인 작업의 단위를 묶어서 DML에 의해 조작된 결과를 트랙잭션 단위로 제어하는 명령어
- COMMIT, ROLLBACK 등


## ✅  기타

### ** TRUNCATE? **
- 정리하다가,  DELETE ALL = TRUNCATE인데 왜 DDL에 속하는걸까?

=> 결과적으로 말하면, TRUNCATE는 단순한 데이터 삭제가 아닌 전체 테이블을 초기화하기 때문이다.

* DELETE 
- 데이터를 삭제하지만, 테이블의 IDENTITY 정보는 유지한다.
=> 이전, IDENTTY 속성의 컬럼 값이 10이었다면 데이터를 DELETE 하더라도 그 다음값은 1이 아닌 11이다.

* TRUNCATE
- 테이블을 초기화한다.
=> 데이터 삭제는 물론, IDENTTY 값도 초기화하여 위와 같은 속성의 컬럼이 있다면 그 다음값은 1이 된다.
=> 테이블의 구조적 변화를 동반하는 명령어이므로 단순히 데이터를 조작하는 DML이 아닌 DDL에 포함된다.
