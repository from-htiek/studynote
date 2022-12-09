# Spring
-   엔터프라이즈 급 어플리케이션을 만들기 위한 모든 기능을 종합적으로 제공하는 경량화 된 솔루션
-   JEE가 제공하는 다수의 기능을 지원하고기 때문에, JEE를 대체하는 framework로 자리잡고 있음
-   그 외 DI나 AOP와 같은 기능도 지원
<br/>


## Spring Framework 구조
Spring 삼각형 = Enterparise Application 개발 시 복잡함을 해결하는 Spring의 핵심
1.  **POJO (Plain Old Java Object)**
    -   특정 환경이나 기술에 종속적이지 않은 자바 객체지향 원리에 충실한 자바객체
    -   특정 기술에 종속적이지 않기 때문에 생산성, 이식성 향상
    -   테스트하기 용이하며, 객체지향 설계를 자유롭게 적용할 수 있음
    -   Plain : component `interface를 상속받지 않는 특징` (특정 framework에 종속되지 않는)
    -   Old : EJB 이전의 java class를 의미
2.  **PSA (Portable Service Abstraction)**
    -   환경과 세부기술의 변경과 관계없이 일관된 방식으로 기술에 접근할 수 있게 해주는 설계 원칙
    -   트랜잭션 추상화, OXM 추상화, 데이터 액세스의 Exception 변환 기능 등 기술적인 복잡함은 추상화를 통해 `Low Level의 기술 구현 부분과 기술을 사용하는 인터페이스로 분리`
        ex) 데이터베이스에 관계 없이 동일하게 적용할 수 있는 트랜잭션 처리방식
3.  **IoC/DI (Dependency Injection)**
    -   DI는 유연하게 확장 가능한 객체를 만들어두고 객체 간의 의존관계는 외부에서 다이나믹하게 설정
        : 더 이상 new 사용 X, 미리 만들어 놓고 주입
4.  **AOP (Aspect Oriented Programming)**
    -   관심사의 분리를 통해서 소프트웨어의 모듈성을 향상
    -   공통 모듈을여러 코드에 쉽게 적용 가능

<br/>


## Spring Framework 특징
-   경량 컨테이너
    -   스프링은 자바객체를 담고 있는 컨테이너
    -   스프링 컨테이너는 이들 자바 객체의 생성, 소멸과 같은 **라이프 사이클 관리**
    -   언제든지 스프링 컨테이너로부터 필요한 객체를 가져와 사용할 수 있음
-   DI (의존성 지원) 패턴 지원
    -   스프링은 설정 파일이나, 어노테이션을 통해서 객체 간의 의존 관게 설정할 수 있음   
        ⇒ **객체는 의존하고 있는 객체를 직접 생성하거나 검색할 필요 없음**
-   AOP(관점 지향 프로그래밍) 지원
    -   문제를 바라보는 관점을 기준으로 프로그래밍 하는 기법
    -   **문제를 해결하기 위한 핵심 관리 사항**과 **전체에 적용되는 공통 관심 사항**을 기준으로 프로그래밍 함으로써 모듈을 여러 코드에서 쉽게 적용할 수 있도록 함
    -   스프링은 자체적으로 프록시 기반의 AOP를 지원하므로 트랙잭션이나 로깅, 보안과 같이 여러 모듈에서 공통으로 필요하지만 실제 모듈의 핵심이 아닌 기능등을 분리하여 각 모듈에 적용 가능
-   POJO 지원
    -   특정 인터페이스를 구현하거나 또는 클래스를 상속하지 않는 일반 자바 객체 지원
    -   스프링 컨테이너에 저장되는 자바객체는 특정한 인터페이스를 구현하거나, 클래스 상속 없이도 사용 가능
    -   일반적인 자바 객체를 칭하기 위한 별칭 개념
-   IoC(제어의 반전)
    -   자바의 객체 생성 및 의존 관계에 있어 모든 제어권은 개발자에게 있음 BUT Servlet과 EJB가 나타나면서 **기존의 제어권이** Servlet Container 및 EJB Container에게 **넘어가게 됨** (단, Servlet과 EJB에 대한 제어권을 제외한 나머지 객체 제어권은 개발자들이 담당)
    -   스프링에서도 객체에 대한 생성과 생명주기를 관리할 수 있는 기능 제공하고 있는데 이런 이유로 Spring Container 또는 IoC Container라고 부름
-   트랜잭션 처리를 위한 일관된 방법 제
    -   JDBC, JTA 또는 컨테이너가 제공하는 트랜잭션을 사용하든, 설정파일을 통해 트랜잭션 관련 정보를 입력하기 때문에 트랜잭션 구현에 상관 없이 동일한 코드를 여러 환경에서 사용 가능
-   영속성과 관련된 다양한 API 지원
    -   JDBC를 비롯하여, iBatis, MyBatis, Hibernate, JPA등 DB 처리를 위해 널리 사용되는 라이브러리와 연동 지원
-   다양한 API에 대한 연동 지원
<br/>


## SpringFramework Module

|모듈|설명|
|----------|---|
|Spring Core|Spring Framework의 핵심 기능 제공, Core 컨테이너의 주요 컴포넌트는 Bean Factory|
|Spring Context|Spring을 컨테이너로 만든 것이 Spring core의 BeanFactory라면, Spring을 Framework로 만든 것은 Context module이며, 이 module은 국제화된 메세지, Application 생명 주기 이벤트, 유효성 검증 등을 지원함으로써 BeanFactory의 개념을 확장함|
|Spring AOP|설정 관리 기능을 통해 AOP 기능을 Spring Framework와 직접 통합 시킴|
|Spring DAO|Spring JDBC DAO 추상 레이어는 다른 데이터베이스 벤더들의 예외 핸들링과 오류 메세지를 관리하는 중요한 예외 계층 제공|
|Spring ORM|Web Context Module은 Application Context module 상위에 구현되어, Web 기반 Application에 context 제공|
|Spring Web MVC|Spring Framework는 자체적으로 MVC 프레임워크를 제공하고 있으며, 스프링만 사용해도 MVC 기반의 웹 어플리케이션을 어렵지 않게 개발 가능|
