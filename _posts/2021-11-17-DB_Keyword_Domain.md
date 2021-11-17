##### Domain(도메인)

Domain이란 엔티티의 속성들이 가질 수 있는 값들의 집합으로, 관계형 이론에서 도메인은 실제로는 구현이 어렵기 때문에, 대부분의 DBMS에서 도메인이란 속성에 대응하는 컬럼에 대한 데이터 타입(Data Type)과 길이를 의미한다.

두 속성의 도메인이 같다라는 것은 두 속성의 데이터 타입과 길이가 같다는 것을 의미한다.

정의된 도메인 명은 일반적인 데이터 타입처럼 사용할 수 있다.



Domain은 데이터 모델링 관점에 있어 데이터 표준화 측면에서 매우 중요한 요소이다.

도메인이 체계화 및 표준화되지 않으면 물리 모델 구현 관점에서 속성과 엔티티의 이름이 표준화되지 않을 수 있고 데이터 타입의 정확성까지 잃을 수 있기 때문이다.

*개발 부석들마다 테이블을 생성하다 서로 외래키로 참조해야하는 상황에서 컬럼 데이터 타입의 불일치 상황등*

컬럼에 데이터 타입을 작성하는 방법은 다음과 같다.

- 직접 데이터 타입을 지정

일반적으로 테이블 생성 시 사용하는 방법으로 DDL 작성 시에 테이블 생성 때 컬럼의 데이터 타입을 지정해주는 방법이다.

```sql
CREATE TABLE EMP (
  EMPID INT(7),
  EMP_NAME VARCHAR(20),
  MANAGER INT(7),
  ADDRESS VARCHAR(100),
  SEX CHAR(1)
);
```

- 도메인을 이용한 컬럼 데이터 타입 지정

아래와 같이 도메인을 생성하여 성별을 'M', 'F' 와 같이 정해진 1개의 문자로 표현하여 컬럼의 데이터 타입을 설정할 수도 있다.

```sql
CREATE DOMAIN SEX CHAR(1)
	DEFAULT 'M'
	CONSTRAINT VALD-SEX CHECK (VALUE IN('M', 'F'));
	
CREATE TABLE EMP (
  EMPID INT(7),
  EMP_NAME VARCHAR(20),
  MANAGER INT(7),
  ADDRESS VARCHAR(100),
  SEX CHAR(1)
);
```

다음은 도메인 생성 시에 사용되는 문법이다.

```sql
CREATE DOMAIN 도메인명 데이터_타입
	[DEFAULT 기본값]
	[CONSTRAINT 제약조건명 CHECK (범위 값)];
```

- 데이터 타입 : SQL에서 지원하는 데이터 타입
- 기본 값 데이터를 입력하지 않았을 때 자동으로 입력되는 값



출처

https://velog.io/@gillog/DBDomain-Data-Dictionary