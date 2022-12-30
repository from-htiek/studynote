# REST

## OPEN API
-   Application Programming Interface
-   OPEN API는 프로그래밍에서 사용할 수 있는 개방되어 있는 상태의 인터페이스(약속)
-   네이버, 카카오 등 포털 서비스 사이트나 통계청, 기상청, 우체국 등과 같은 관공서, 공공 데이터 포털이 가지고 있는 데이터를 외부 응용 프로그램에서 사용할 수 있다록 OPEN API 제공 중
-   OPEN API와 함께 거론 되는 기술이 REST이며, 대부분의 OPEN API는 REST 방식으로 지원

<br/>

## REST

-   Representational State Transfer
-   **하나의 URI는 하나의 고유한 리소스(Resource)를 대표하도록 설계** 된다는 개념에 **전송방식을 결합**에서 원하는 작업 지정
    > **URI** + **GET / POST / PUT / DELETE**
-   HTTP URI를 통해 제어할 자원(Resource)를 명시하고, HTTP Method(GET, POST, PUT, DELETE)를 통해 해당 자원을 제어하는 명령을 내리는 방식의 아키텍처
    
-   REST 구성
    -   자원(Resource) - URI
    -   행위(Verb) - HTTP Method
    -   표현 (Representations)
> 💡 잘 표현된 HTTP URI로 리소스를 정의하고 HTTP 메소드로 리소스에 대한 행위를 정의한다. 리소스는 JSON, XML과 같은 여러가지 언어로 표현할 수 있다. (종속적이지 않음)

<br/>

## 기존 Service VS. REST Service
-   기존 : 요청에 대한 처리를 한 후 가공된 데이터를 이용하여 특정 플랫폼에 적합한 형태의 View로 만들어서 반환
-   REST : 데이터 처리만 하거나, 처리 후 반환 될 데이터가 있다면 JSON, XML 형식으로 전달. View에 대해서는 신경 쓸 필요가 없음(OPEN API에 많이 사용되는 이유)

<br/>

## REST 특징
-   기존의 전송방식과는 달리 서버는 요청으로 받은 리소스에 대해 순수한 데이터를 전송
-   기존의 GET/POST 외에 PUT, DELETE 방식을 사용하여 리소스에 대한 CRUD 처리 할 수 있음
-   가장 큰 단점은 정확히 정해진 표준이 없다는 것 ⇒ 최소 조건만 지키면 RESTFUL 하다고 할 수 있음

|작업(SQL)|기존||REST||비고|
|------|---|---|--|--|--|
|Read(Select)|POST|/write?id=ssafy|POST|/blog/ssafy|글쓰기|
|Read(Select)|GET|/view?id=ssafy&no=10|GET|/blog/ssafy/10|글읽기|
|Update(update)|POST|/modify?id=ssafy|PUT|/blog/ssafy|글수정|
|Delete(delete)|GET|/delete?id=ssafy&no=10|DELETE|/blog/ssafy/10|글삭제|


cf) 간단한 수정은 PATCH 메소드 사용

<br/>

## Annotation
1.  `@RestController` : Controller가 REST 방식을 처리하기 위한 것임을 명시
    > controller + responsebody
2.  `@ResponseBody` : JSP 같은 뷰로 전달 되는 것이 아니라 데이터 자체를 전달
3.  `@PathVariable` : URL 경로에 있는 값을 파라미터로 추출
4.  `@CrossOrigin` : Ajax의 크로스 도메인 문제 해결
5.  `@RequestBody` : JSON 데이터를 원하는 타입으로 바인딩

