## CH04. 데이터베이스 작업

### 데이터베이스란?

> 데이터베이스: 데이터를 보관하기 위한 `상자`



### 관계형 데이터베이스란?(RDB)

> 관계형 데이터베이스? : 데이터를 표 형식으로 표현하고, 여러 표에서 항목의 값 사이에 관계를 맺고 있는 `DB`



### 관계형 데이터베이스 종류(인기 순)

- Oracle
    -  Oracle Database는 관계형 모델을 객체-관계형 모델로 확장하여 복잡한 비즈니스 모델을 관계형 데이터베이스에 저장할 수 있도록 합니다.
- MySQL
    - 무료이다.
    - MySQL 소프트웨어는 오픈 소스입니다.
    - MySQL 데이터베이스 서버는 매우 빠르고 안정적이며 확장 가능하고 사용하기 쉽습니다.
    -  MySQL Server는 클라이언트/서버 또는 임베디드 시스템에서 작동합니다.
- SQL Server
    - C 및 C++로 작성되었다.
    - SQL Server는 Azure SQL Server로 Microsoft Azure 클라우드의 일부이기도 한다.
- PostgreSQL
    - 가장 복잡한 데이터 워크로드를 안전하게 저장하고 확장하는 많은 기능과 결합된 SQL 언어를 사용하고 확장하는 강력한 오픈 소스 객체 관계형 데이터베이스 시스템이다.
    - SQL 뿐만 아니라 JSON을 이용해 데이터에 접근할 수 있다.
    - 지정 시간에 복구하는 기능, 로깅, 접근 제어, 중첩된 트랜잭션, 백업 등을 할 수 있다.
- MariaDB
    - MySQL의 원래 개발자가 만들었고 오픈 소스를 유지하도록 보장한다.
    - 주목할만한 사용자로는 Wikipedia, WordPress.com 및 Google이 있다.
    -  빠르고 확장 가능하며 견고하며 스토리지 엔진, 플러그인 및 다양한 사용 사례에 매우 다재다능하게 만드는 기타 여러 도구의 풍부한 생태계를 갖추고 있기 때문에 사용된다.



물론 이 중에서 보면 MySQL, MariaDB 유명한 것들이 많지만 책에 나오는대로 `PostgreSQL`을 사용해보자.


### 테이블?

> - 테이블 : 실제로 규칙을 가진 데이터가 저장되는 상자를 칭함.
>   - Row(Record) : 테이블의 가로(행)
>   - Column: 테이블의 세로(열)


### 테이블의 제약 조건

| 제약 조건       | 개요                                                           | 
|-------------|--------------------------------------------------------------|
| NOT NULL    | NULL 입력을 허용하지 않는다.(필수 입력).                                   | 
| UNIQUE      | 중복값 입력을 허용하지 않는다(고유한 값).                                     |  
| CHECK       | 지정한 조건을 만족하지 않는 값의 입력을 허용하지 않는다.                             |
| PRIMARY KEY | 테이블 안에서 레코드를 식별하는 기본키를 설정한다. 기본키는 NOT NULL과 UNIQUE가 함께 적용된다. | 
| FORIEGN KEY | 관련된 테이블을 연걸하는 설정이다. 외부 키라고도 부른다.                             | 
| DEFAULT     | 칼럼의 초기값을 설정한다.                                               | 



### SQL이란?

> SQL : 데이터베이스를 조작하기 위한 용어이다. Structured Query Language의 약어이다.

#### CRUD

> CRUD는 영속적으로 데이터를 취급하는 4개의 기본적인 기능인 Create, Read, Update, Delete의 머리글자를 따서 만든 단어이다. 



| CRUD        | 개요              | 
|-------------|-----------------|
| 생성          | INSERT : 데이터 등록 | 
| 읽기          | SELECT : 데이터 참조 |  
| 갱신          | UPDATE : 데이터 갱신 |
| 삭제          | DELETE : 데이터 삭제 | 


| CRUD        | 구문                                                                               | 
|-------------|----------------------------------------------------------------------------------|
| 생성          | INSERT INTO 테이블명 (칼럼명, 칼럼명...) VALUES(값, 값, ...);                                | 
| 읽기          | SELECT 칼럼명 FROM 테이블명;                                                            |  
| 갱신          | UPDATE 테이블명 SET 칼럼명 = 값 WHERE 갱신할_레코드를_특정하는_조건; (WHERE로 조건을 지정하지 않으면 모든 레코드가 삭제) |
| 삭제          | DELETE FROM 테이블명 WHERE 삭제할_레코드를_특정하는_조건   (WHERE로 조건을 지정하지 않으면 모든 레코드가 삭제)       | 


### 엔티티?

> 엔티티 : 데이터를 담아두는 객체 -> 데이터베이스 테이블의 한 행(record)에 대응하는 객체.

이런 개념 속 엔티티에 해당하는 필드는 테이블의 칼럼값에 대응이 된다.

```java
/**
 * Member 테이블 : 엔티티
 */

public class Member {

  private Integer id;

  private String name;

  public Integer getId() {
    return id;
  }

  public void setId(Integer id) {
    this.id = id;
  }

  public String getName() {
    return name;
  }


  public void setName(String name) {
    this.name = name;
  }
}

```


보통 엔티티를 만들고 고려할 때 다음 3가지에 대해서 고려해야 한다고 한다.

1. 클래스명
   - 클래스명은 대응하는 데이터베이스의 테이블명으로 하는 경우가 많다.
2. 데이터베이스에서 값 넘겨주는 것
   - 데이터베이스 값을 등록/갱신하는 경우에는 엔티티에 값을 넣어서 넘겨준다.
3. 데이터베이스에서 값 가져오는 것
  - 데이터베이스에서 값을 가져오는 경우 값을 엔티티에 넣어서 가져온다.



### 엔티티 유형

- 강한 엔티티 : 스스로 존재하며 독립적인 개체
  - 기본 키로 구성
- 약한 엔티티 : 자신의 존재가 없지만 강한 엔티티에 의존하여서 존재하게 되는 엔티티
  - 기본 키가 포함이되어있지 않다.
  - ex) 고객 엔티티의 주소(시,도로명주소, 나라)와 같은 것

### Repository

> Repository: 데이터베이스를 조작하는 클래스이다. 


책에서 말하는 대로 따라가자면 리포지토리를 생성하는 경우에는 반드시 인터페이스를 정의하고 구현해야 한다. 왜냐하면 인터페이스의 필드에 리포지토리 구현 클래스를 DI하여 특정 구현에 의존하는 것을 피할 수 있기 때문이다.

자바에서 그래서 가끔 예제들을 보면 뒷말에 나오는 `Impl` 을 인터페이스를 구현한 클래스의 접미사로 붙이게 되는 것을 알 수 있다.



### ORM

>ORM : 애플리케이션에서 사용하는 O(Object) 와 R(Relational) 관계형 데이터베이스의 데이터를 매핑하는 것. Object-Relational-Mapping의 약자이다.


### ORM을 쓰는 이유?

> - 가장 큰 이유로서는 패러다임의 불일치를 고려하지 않기 위해서이다.
> - 객체 지향 패러다임을 사용하여 데이터베이스에서 데이터를 쿼리하고 조작할 수 있는 기술입니다. 데이터베이스와 통신하는 데 필요한 코드를 캡슐화하므로 더 이상 SQL을 사용하지 않아도 된다.



### 스프링 데이터 JDBC

사실 이때까지 `Spring Data JPA`라는 단어를 더 들어서 어색할 수 있지만 `Spring Data JDBC`는 새로 나온 기술이라고 한다.

- Spring Data JDBC
  -  Spring 데이터 계열에 속함
  -  JDBC 기반 데이터 액세스 계층에 대한 추상화 제공
  -  ORM 프레임워크 제공
  - Lazy Loading, 엔티티 캐싱 같은 JPA 기능 생략

그래서 JPA보다는 어찌보면 고차원적이지는 않을 수 있다. 그래서 상황마다 잘 판단해서 어떤 걸 사용할지 구분해야 한다고 한다. 



### application.properties 설정

책에서는 이 이후에 실습을 바로 들어가지만 application.properties에서 스프링 부트 프로젝트의 환경 설정용 파일을 설정해주고 시작한다. 다음과 같은 항목들을 먼저 선정해준다.

| 항목                                  | 설명                                            | 
|-------------------------------------|-----------------------------------------------|
| spring.datasource.driver-class-name | JDBC 드라이버의 클래스명을 지정한다. 우리 코드에서는 Postgre를 지정한다. | 
| spring.datasource.url               | 데이터베이스의 '접속 URL'을 설정한다.                       |  
| spring.datasource.username          | 데이터베이스에 접속하는 '유저명'을 설정한다.                     |
| spring.datasource.password          | 데이터베이스에 접속하는 '패스워드'를 설정한다.                    |


위 설정을 해줘야 오류가 생기지 않는다. 그냥 실행해버리면 아마도 connection을 맺지를 못한다는 등의 오류나 어디로 연결해야하는지에 대한 url이 없다는 오류가 발생한다. 



### 모양이 다르다.

```java
public interface MemberCrudRepository extends CrudRepository<Member, Integer>{
    
}


```


애초에 `CrudRepository<Member, Integer>`보다 당연히 `JpaRepository<Member,Integer>`의 모양을 더 봤을 거다. 위에 설명한대로의 차이가 있으니 잘 구분하여 사용하자.

### 번외) application.properties drivernaeme

> 설정을 해줘야 하는것이 안해준다면 어떤 datasource를 사용할 지 모르기 때문에 꼭 postgre를 사용한다고 해줘야 한다.


### 번외) application.properties url 설정을 안한다면?
![application_properties_오류1.png](application_properties_%EC%98%A4%EB%A5%981.png)



### 번외) application.properties password 설정을 안한다면?
![application_properties_오류2.png](application_properties_%EC%98%A4%EB%A5%982.png)


> id도 다음과 비슷하게 오류가 발생한다.

### 출처 
- https://www.quora.com/What-is-the-difference-between-a-database-and-a-repository
- https://stackoverflow.com/questions/1279613/what-is-an-orm-how-does-it-work-and-how-should-i-use-one
- https://dev.to/anubhavitis/why-we-should-always-use-django-orm-2l6m
- https://cloud.google.com/learn/what-is-a-relational-database?hl=ko#section-4