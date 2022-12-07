# JPA
JPA란 Java Persistence API의 약자로 자바의 ORM(Object Relational Mapping) 표준 스펙을 정의  
JPA의 스펙은 자바의 객체와 데이터베이스를 어떻게 매핑하고 동작해야 하는지를 정의 하고 있음
  
   
## Hibernate
ORM 프레임워크 중 하나로 'JAP Provider'라고도 부름  
JPA의 실제 구현체 중 하나이며 많이 사용되는 프레임워크 중 하나  
  
  
### Persistence Context  
영속성 컨텍스트(Persistence Context)는 JPA가 관리하는 엔티티 객체의 집합  
엔티티 객체가 영속성 컨텍스트에 들어오게 되면 JPA는 엔티티 개개체의 매핑 정보를 가지고 DB에 반영  
엔티티 객체가 영속 컨텍스트에 들어오게 되어 관리 대상이 되면 그 객체를 영속 객체라고 부름  
영속성 컨텍스트는 세션 단위로 생명주기를 갖고 있음(세션이 생기면서 만들어지고, 세션이 종료되면 없어짐)  
영속성 컨텍스트에 접근하기 위해 EntityManager를 사용  
EntityManager 동작 구성  
- EntityManager todtjd(EntityManagerFactory를 통해 생성)
- EntityManager가 가지고 있는 트랜잭션(Transaction)을 시작
- EntityManager를 통해 영속 컨텍스트에 접근하고 객체를 작업
- 트랜잭션을 커밋(Commit)하여 DB에 반영
- EntityManager 종료
  
  
### Entity 클래스
JPA 어노테이션을 활용하여 클래스 정의  
Entity 클래스의 이름은 DB의 테이블 이름, 필드 변수는 테이블의 칼럼, 값들은 데이터로 생각할 수 있음  
주요 어노테이션  
- @Entity : 해당 클래스가 JAP Entity 클래스라고 정의
- @Table : 해당 클래스가 DB의 어느 테이블에 매핑되는지 정의
- @Id : DB 테이블의 Primary Key 칼럼과 매핑
- @Column : 매핑할 DB의 칼럼 이름과 필드 변수의 이름이 다를 경우 매핑하기 위해 사용  
  
#### 참고자료
Youtube : 어라운드허브 스튜디오 JPA 강의  