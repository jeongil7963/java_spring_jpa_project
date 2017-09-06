# JAVA와 Sprin Framework를 이용한 JPA 프로젝트

## Document

### JPA 설명

-   JPA 개요

        자바의 객체와 데이터베이스의 테이블이 정확하게 일치하지 않는다. 
        따라서 둘 사이를 매핑하기 위해 많은 SQL 구문과 자바코드가 필요하다. 
        ORM은 이렇게 정확하게 일치하지 않는 자바 객체와 테이블 사이를 매핑해준다. 
        ORM 프레임워크의 가장 큰 특징이자 장점은 DB 연동에 필요한 SQL을 자동으로 생성한다는 것이다. 
        이러한 ORM 프레임워크들에 대한 표준화 작업이 필요했고 그런 노력의 결과가 바로 JPA인 것이다.

-   JPA 특징

        JPA는 모든 ORM 구현테들의 공통 인터페이스를 제공한다. 
        JDBC 특정 DBMS에 종속되지 않는 DB 연동 구현을 지원한다. 
        따라서 DBMS가 변경되는 상황에서도 드라이버만 변경하면 JDBC API를 이용하는 API를 이용하는 애플리케이션은 수정하지 않는다. 
        실제 서비스가 오픈될 때는 DBMS를 변경할 수 있다.


### JPA 시작하기 

- 엔티티 클래스 매핑  

        데이터베이스의 테이블과 매핑될 영속 클래스를 작성하여 매핑 관련 설정을 추가하는 것이다. 
        특별한 제약조건이나 규칙이 있는 것은 아니므로 일반적인 VO 클래스를 만들 때처럼 작성하면 된다. 
        @Enitity : 테이블과 매핑된다.
        @Table : 엔티티와 관련된 테이블을 매핑한다.
        @Id : 특정 변수를 테이블 기본 키와 매핑한다.
        @GeneratedValue : 선언된 필드에 기본 키를 자동으로 생성하여 할당할 때 사용한다.
        @Temporal : 날짜 타입의 변수에 선언하여 날짜 타입을 매핑할 때 사용한다.

### JPA 환경 설정

-   영속성 유닛 설정

        영속성 유닛은 연동할 테이터베이스당 하나씩 등록하며, 영속성 유닛에 설정된 이름은 나중에 DAO 클래스를 구현할 때 EntityManagerFactory 객체 생성에 사용된다.
        

-   엔티티 클래스 등록

        영속성 유닛 설정에서 가장 먼저 엔티티 클래스를 등록하는데, 이 엔티티 클래스는 JPA 프로젝트에서 JPA Entity 클래스를 작성하는 순간 자동으로 xml 파일에 등록된다. 

-   영속성 유닛 프로퍼티 설정

        엔티티 클래스를 등록했으면 이제 엔티티 클래스의 영속성 관리에 필요한 프로퍼티들을 설정해야 하는데, 이중에서 가장 기본이면서 중요한 것이 DB 커넥션 관련 설정이다. 

-   Dialect 클래스 설정

        ORM 프레임워크의 가장 중요한 특징이자 장점은 애플리케이션 수행에 필요한 SQL 구문을 자동으로 생성한다는 것이다. 
        이 SQL을 아무리 표준에 따라서 잘 작성하다고 해도 DBMS에 따라서 키 생성 방식도 다르고 지원되는 함수도 조금씩 다르다. 
        따라서 DBMS가 변경되면 이런 세세한 부분을 개발자가 적절하게 수정해야 한다.
        JPA는 특정 DBMS에 최적화된 SQL을 제공하기 위해서 DBMS마다 다른 Dialect 클래스가 해당 DBMS에 최적화된 SQL 구문을 생성한다.

- JPA 구현체 관련 속성 설정

		show_sql : 생성된 sql을 콘솔에 출력한다.
		format_sql : sql을 출력할 때, 일정한 포멧으로 보기 좋게 출력한다.
		use_sql_comments : sql에 포함된 주석도 가이 출력한다.
		id.new_generator_mappings : 새로운 키 생성 전략을 사용한다.
		hbm2ddl.auto : 테이블 생성이나 수정, 삭제 같은 DDL 구문을 자동으로 처리할지를 지정한다.
		
- 엔티티 클래스 기본 매핑

	JPA의 기본은 엔티티 클래스를 기반으로 관계형 데이터베이스에 저장된 데이터를 관리하는 것이다.
	@Entity : 특정 클래스를 JPA가 관리하는 엔티티 클래스로 인식하는 가장 중요한 어노테이션이다.
	@ID : 식별자 필드는 엔티티 클래스라면 무조건 가지고 있어야 하며 id를 이용하여 선언한다.
	@Table : 엔티티 클래스를 정의할 때 엔티티 클래스와 매핑되는 테이블 이름을 별도로 지정하지 않으며 엔티티 클래스 이름과 같은 이름의 테이블이 자동으로 매핑된다.
	@Column : 엔티티 클래스의 변수와 테이블의 칼럼을 매핑할 때 사용한다. 엔티티 클래스의 변수 이름이 다를 때 사용하며, 생략하면 기본으로 변수 이름과 칼럼 이름을 동일하게 매핑한다.
	@GeneratedValue : @Id로 지정된 식별자 필드에 Primary key 값을 생성하여 저장할 때 사용한다.
	@Transient : 엔티티 클래스의 변수들은 대부분 테이블의 칼럼과 매핑된다. 그러나 몇몇 변수는 매핑되는 칼럼이 없거나 아예 매핑에서 제외해야만 하는 경우도 있다.
	@Temporal : 날짜 데이터를 매핑할 때 사용한다. 

- JPA API

	엔티티 클래스에 기본적인 매핑을 설정했으면 JPA에서 지원하는 API를 이용하여 데이터베이스 연동을 처리할 수 있다.
		Persistence 클래스를 이용하여 영속성 유닛 정보가 저장된 JPA 메인 환경 설정 파일을 로딩한다.
		이 설정 정보를 바탕으로 EntityManager를 생성하는 공장 기능의 EntityManagerFactory 객체를 생성한다.
		이제 EntityManagerFactory로부터 필요한 EntityMaanager를 얻어서 사용하면 된다.
	
	JPQL은 기존에 사용하던 SQL과 거의 유사한 문법을 제공하므로 해석하는 데는 별 어려움이 없다. 
	하지만 검색 대상이 테이블이 아닌 엔티티 객체라서 작성하는 데 주의가 필요하다.


# 스프링과 JPA 연동

- 스프링과 JPA 연동 기초

	스프링과 JPA를 연동하려면 JPA 프로젝트로 변환해야 한다.
	라이브러리 내려 받기
	JPA 설정 파일 작성

- 엔티티 매핑 설정

	기존에 사용하던 클래스를 열어서 JPA가 제공하는 어노테이션으로 엔티티 매핑을 설정한다.

- 스프링과 JPA 연동 설정

	연동하기 위해서는 두 개의 클래스를 스프링 설정 파일에 <BEAN> 등록해야 한다.
	JPA 연동을 위해 가장 먼저 등록할 클래스는 JpaVendorAdapter이다.
	두번째로 등록할 클래스는 EntityManagerFactoryBean이다.
	우리가 JPA를 이용하여 DAO 클래스를 구현하려면 최종적으로 EntityManaer 객체가 필요하다.

- 트랜잭션 설정 수정

	JPA를 이용해서 DB 연동을 처리할 때 관리자를 JpaTransactionManager를 변경해야 한다.

- DAO 클래스 구현

	스프링과 JPA 연동에 필요한 모든 설정을 마무리 했으면 이제 JPA 기반의 DAO 클래스만 구현하면 된다.
	JPA를 이용해서 DAO 클래스를 구현할 때는 EntityManager 객체를 사용해야 하는 데 JPA Projectdpt에서는 EnitityManagerFactory로부터 EntityManager 객체를 직접 얻어냈었다.
	하지만 JPA를 단독으로 사용하지 않고 스프링과 연동할 때는 EnitiyManagerFactory에서 EntityManager를 직접 생성하는 것이 아니라 스프링 컨테이너가 제공하는 EntityManager를 사용해야만 한다.
	@PersistenceContext는 스프링 컨테이너가 관리하는 EntityManger 객체를 의존성 주입할 때 사용하는 어노테이션이다.
