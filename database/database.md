

## 데이터베이스 기본 개념 정리


### 1. Data
### 2. SQL Optimizer
### 3. Data Model
### 4. Normalization

<br>

---

##  1. Data

*Data* 
: 정성적 또는 정량적 변수 값의 집합

- 1. Structured Data
    - 고정된 스키마를 따르며, 행과 열로 구분된 테이블 형식
    - 사용 기술 : RDBMS 데이터베이스(Oracle, Mysql등)

- 2. Semi - Structured Data
    - 고정된 스키마는 없으나, 태그나 구분자가 있는 데이터
    - 사용 기술 : NoSQL, JSON, XML, 로그 파일

- 3. Unstructured Data
    - 스키마가 없고, 분석 및 처리하기 어려운 데이터
    - 사용 기술 : Hadoop, Spark, NLP등

<br>

*Database* 
: 조직화된 데이터의 집합

<br>

*DBMS* 
: 데이터베이스를 관리하기 위한 하나의 어플리케이션 프로그램
- Relational DBMS
    - 구조 : 데이터를 테이블 형식으로 저장(행겨 열)
    - 사용 사례 : 전통적 애플리케이션
- Key-Value DBMS
    - 구조 : 키- 값 쌍으로 데이터를 저장
    - 사용 사례 : 캐시, 세션 데이터
- Document DBMS : 
    - 구조: JSON, BSON과 같은 문서 형식으로 데이터 저장
    - 사용 사례 : 웹 애플리케이션, 콘텐츠 관리     


<br>
<br>

**SQL** 

| 종류 | 설명 |
|--- | ---|
| DML | - Data Manipulation Language <br> - SELECT, INSERT, UPDATE ,DELETE 
| DDL | - Data Definition Language <br> - CREATE, ALTER, DROP, TRUNCATE, COMMIT  
| DCL | - Data Control Language <br> - GRANT, REVOKE
|


<br>
<br>

---

## 2. SQL Optimizer

### 2.1 SQL 옵티마이저

**SQL** 
- **Structured Query Language**

**SQL Optimizer**
- 원하는 결과를 구조적, 집합적으로 선언하지만, 그 결과집합을 만드는 과정은 절자척일 수 밖에 없음. 즉, 프로시저가 필요한데 그런 프로시저를 만들어 내느 **DBMS 내부 엔진**


<br>

### 2.2 SQL 처리 단계

1 .SQL 파싱
 - 파싱 트리 생성
 - Syntax 체크
    - 문법상 오타등의 체크(ex: talbe, craete등)
 - Semantic 체크
    - 실제 입력한 테이블이 존재하는지등 확인

2. SQL 최적화
  - 미리 수집한 시스템 및 오브젝트 통계정보를 바탕으로 다양한 실행경로를 생성해서 비교한 후 가장 효율적인 하나를 선택하는 단계

3. 로우 소스 생성
  - SQL Optimizer가 선택한 실행경로를 실제 실행 가능한 코드 또는 프로시저 형태로 포밋탱 하는 단계

<br>

### 2.3 SQL 최적화

- 옵티마이저
    - 사용자가 원하는 작업을 가장 효율적으로 수행할 수 있는 최적의 데이터 액세스, 경로를 선택해 주는 DBMS의 핵심 엔진

- 최적화단계
    1. 사용자로부터 전달받은 쿼리를 수행하는 데 후보군이 될만한 실행계획들을 선정
    2. Data Dictionary에 미리 수집해 둔 오브젝트 통계 및 시스템 통계정보를 이용해 각 실행계획의 예상비용 산정
    3. 최저 비용을 나타내는 실행계획 선택

<br>

### 2.4 실행계획과 비용
  - 실행계획
    - SQL 실행경로 미리보기
    - SQL Optimizer가 생성한 처리 절차를 사용자가 확인할 수 있게 트리 구조로 표현한 것
     
  - 비용
    - I / O 비용 모델(가장 비용이 많이 발생)
        - I/ O Call 횟수
    - CPU 비용 모델
        - Single Block I / O 를 기준으로 한 상대적 시간 개념

<br>

### 2.5 옵티마이저 힌트    
- 사용자가 직접 더 효율적인 액세스, 경로를 찾아냈을 때, 데이터 액세스, 경로를 바꿀 수 있도록 하는 기능
- (EX : /* + INDEX(e.emp_last_name_idx */))