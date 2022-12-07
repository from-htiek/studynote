# JPA VS. MyBatis
면접관님이 JPA와 MyBatis의 차이가 뭐냐고 물었는데 할 수 있는 말은 JPA가 좀 더 객체 지향적이고 직접 SQL을 쓰는 경우가 더 적다........ 라는 굉장히 부족하디 부족한 답변을 하면서 :sweat_smile: 공부를 더 해봐야겠다는 생각을 했다. 생각해보면 둘 다 써봤는데, 명확한 차이는 생각을 안해본 것 같다. JPA가 DB의 종속성을 제외하는거 정도 밖에는? 공부하자 공부.   
+) 이 글은 유독.... 그대로 가져온 부분이 많은 것 같다.   
내 코드를 제외하고는 대부분인듯. 참고자료로 남겨놓긴 했지만 문제가 되면 바로 삭제하는 걸로
<br/>

---
데이터들이 프로그램이 종료되어도 사라지지 않고 어떤 곳에 저장되는 개념을 **영속성(Persistence)** 이라고 한다.  
자바에서는 데이터의 영속성을 위한 JDBC를 지원해주는데, 이는 매핑 작접을 개발자가 일일이 해야하는 번거로움이 있다. **SQL Mapper**와 **ORM**은 개발자가 직접 JDBC 프로그래밍을 하지 않도록 기능을 제공해주는 **Persistence Framework** 종류이다.   
기존 JDBC 만의 사용으로 쿼리문을 만들어 요청하는 과정은 쿼리문이 조금만 길어져도 관리가 힘들거 번거롭다. 따라서 **JPA**와 **MyBatis** 라이브러리를 사용하여 문제를 해결할 수 있다.
> `SQL Mapper` : **Object와 SQL의 필드**를 매핑하여 데이터를 객체화   
> `ORM` : **Object와 DB 테이블**을 매핑하여 데이터 객체화

<br/>

그럼 먼저 JPA와 MyBatis를 사용하지 않았을 때 코드를 보자.  
주택정보 제공 프로젝트에서 프레임워크를 쓰지 않고 구현한 적이 있었는데 그 때 코드이다. 참고로 dbUtil은 데이터소스를 가져오는 코드가 있는 클래스였다. 
```java
@Override
public int regist(MemberDto dto) throws SQLException {

	int result = 0;
	Connection conn = null;
	PreparedStatement pstmt = null;
	try {
		conn = dbUtil.getConnection();
		pstmt = conn.prepareStatement("insert into member (memId, memPw, memName, memEmail, memTel, signupDate)"
		+ "values (?, ?, ?, ?, ?, now())" );
		pstmt.setString(1, dto.getMemId());
		pstmt.setString(2, dto.getMemPw());
		pstmt.setString(3, dto.getMemName());
		pstmt.setString(4, dto.getMemEmail());
		pstmt.setString(5, dto.getMemTel());
		result = pstmt.executeUpdate();

	} finally {
		dbUtil.close(pstmt, conn);
	}

	return result;

}
```
<br/>

## JPA
`Java Persistence API`의 약자로 Java ORM 기술에 대한 API 표준 명세를 말한다. JPA는 단순한 명세이기 때문에 JPA만 가지고는 어떤 구현 기술을 사용할 수 없다. 실제로 우리가 사용하는 Repository는 Spring Data JPA로 부터 제공되는 기술이다. Spring Data JPA는 JPA를 간편하게 사용하도록 만들어놓은 오픈 소스일 뿐이다. 이와 비슷한 기술로 Spring Data JPA, Spring Data Redis, Spring Data MongoDB 등과 같은 다양한 라이브러리가 존재 한다. 그리고 JPA를 사용하다 보면 Hibernate를 많이 사용하게 되는데 Hibernate는 JPA의 구현체라고 할 수 있다. Hibernate 이외에도 DataNucleus, EclipseLink 등 다양한 JPA 구현체가 존재한다. 그래도 우리가 Hibernate를 사용하는 것은 가장 범용적으로 다양한 기능을 제공하기 때문이다.

### JPA의 장점
- **DB에 종속적이지 않으므로** 특정 쿼리를 사용하지 않아 추상적으로 기술 구현 가능
- 개발 초기에는 쿼리에 대한 이해가 부족해도 코드 레벨로 어느 정도 커버 가능
- 객체 지향적으로 데이터 관리 가능
- 컴파일 타임에 오류 확인 가능
- **Entity로 관리**되므로 스키마 변경시 Entity만 수정하게 되면 Entity를 사용하는 관련 쿼리는 자동으로 변경된 내역이 반영됨
- 코드 레벨로 관리 되므로 사용하기 용이하고 생산성이 높음
- 1차캐시, 쓰기지연, 변경감지, 지연로딩을 제공하여 성능상 이점이 있음
- CRUD 메소드 기본 제공

### JPA의 단점
- JPA만 사용하여 복잡한 연산을 수행하기에는 다소 무리가 있음
- 초기에는 생산성이 높을 수 있으나 사용하다보면 성능상 이슈 발생 가능(N+1, FetchType, Proxy, 연관관계 등) => 추가로 공부해보자
- 고도화 될수록 학습 곡선이 높아질 수 있음
<br/>

JPA 로 구현했던 회원 등록 코드를 보자. 먼저 회원의 Entity가 있어야 한다. 뷰들뷰들 프로젝트에서 가져왔는데 너무 길어서 일부분만 가져왔다. 
```java
@Entity
@Getter
@Setter
@NoArgsConstructor
@Table(name="User")
public class User{
	
	@Id
	@GeneratedValue(strategy = GenerationType.IDENTITY)
	@Column(name="user_seq")
	int userSeq;
		
	@Column(name="user_email")
	String userEmail;

	@Column(name="user_name")
	String userName;

	@Column(name="user_password")
	String userPassword;
		
	@Column(name="user_reg_time")
	String userRegTime;
}
```
그리고 Service단에서는 save만 해주면 끝이다.
```java
@Override
public User createUser(String email, String name, String password, String password2){
	if(userRepository.findByUserEmail(email) != null){
		throw new AlreadyExistEmailException();
		} else if (!password.equals(password2)) {
		throw new NotMatchPasswordException();
	}

	User user = new User();
	user.setUserEmail(email);
	user.setUserPassword(passwordEncoder.encode(password));
	user.setUserName(name);
	return userRepository.save(user);
}
```
<br/>

## MyBatis
MyBatis는 원래 Apache Foundation의 iBatis였으나, 생산성, 개발 프로세스, 커뮤니티 등의 이유로 Google Code로 이전되면서 이름이 변경되었다. MyBatis는 record에 원시 타입과 Map 인터페이스, 그리고 자바 POJO를 설정해서 매핑하기 위해 xml과 Annotation을 사용할 수 있다. JPA는 ORM 기술로 분류되고, MyBatis는 SQL Builder 또는 SQL Mapper의 한 종류라 사실 JPA와 MyBatis는 비교 대상이 되지 않을 수 있다.

### MyBatis 장점
- **SQL 쿼리를 직접 작성**하므로 최적화된 쿼리를 구현할 수 있음 
- Entity에 종속받지 않고 **다양한 테이블을 조합**할 수 있음
- 복잡한 쿼리도 SQL 쿼리만 작성할 수 있다면 손쉽게 작성 가능

### MyBatis 단점
- 자바 클래스 코드와 직접 작성한 SQL 코드를 매핑해주어야 함
- 스키마 변경시 SQL 쿼리를 직접 수정해주어야 함
- 반복된 쿼리가 발생하여 반복 작업이 있음
- 런타임시에 오류를 확인할 수 있음 
- 쿼리를 직접 작성하기 때문에 DB에 종속적인 쿼리문이 발생할 수 있음
- CRUD 메소드를 직접 다 구현해야함
<br/>

마지막으로 MyBatis를 사용한 코드를 보자. 이 코드는 xml 코드만 적어놓긴했지만 `interface`도 만들어야 하고, 해당 mapper를 읽기위해 application.properties도 설정을 따로 해줬던 것 같다. 요즘 JPA를 많이 썼었는데 확실히 느낌이 굉장히 다르다. 
```java
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ssafy.happyhouse.mapper.MemberMapper">
	<insert id="regist" parameterType="MemberDto">
		insert into member (memId, memPw, memName, memEmail, memTel, signupDate)
		values (#{memId}, #{memPw}, #{memName}, #{memEmail}, #{memTel}, now())
	</insert>
</select>
```
<br/>
  
**참고자료** :    
https://incheol-jung.gitbook.io/docs/q-and-a/spring/jpa-vs-mybatis    
https://velog.io/@rladuswl/ORM%EC%9D%98-%EA%B0%9C%EB%85%90-JPA%EC%99%80-MyBatis-%EC%B0%A8%EC%9D%B4   
https://mangkyu.tistory.com/20?category=872426  





