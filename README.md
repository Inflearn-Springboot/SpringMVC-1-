# 2-1 스프링MVC - 백엔드 웹 개발 핵심 기술
 웹 애플리케이션을 개발할 때 필요한 모든 웹 기술 기초 그리고 스프링 MVC의 핵심 원리와 구조 이해
 https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1#

### 전체 목차
1. 웹 애플리케이션 이해
2. 서블릿
3. 서블릿, JSP, MVC 패턴
4. MVC 프레임워크 만들기
5. 스프링 MVC - 구조 이해
6. 스프링 MVC - 기본 기능
7. 스프링 MVC - 웹 페이지 만들기

--------------
#### 1. 웹 애플리케이션 이해

 - 웹 서버, 웹 애플리케이션 서버

<img width="535" alt="image" src="https://github.com/Inflearn-Springboot/SpringMVC-1-/assets/96871403/bbd01bcc-1ada-4a82-8577-2fb7f559d742">

 - 서블릿
   
   <img width="591" alt="image" src="https://github.com/Inflearn-Springboot/SpringMVC-1-/assets/96871403/2c37f423-b509-4e5c-b3c1-9a52506cb324">
   
 - 동시 요청 - 멀티 쓰레드
   - WAS의 멀티 쓰레드 지원 -> **개발자가 멀티 쓰레드 관련 코드를 신경쓰지 않아도됨**

   <img width="544" alt="image" src="https://github.com/Inflearn-Springboot/SpringMVC-1-/assets/96871403/f03f77ff-9e37-49cb-b056-38a50a1de24a">
  
 - HTML, HTTP API, SSR, CSR
   - 정적 리소스 : 고정된 HTML 파일, CSS, JS, 이미지, 영상 등을 제공
   - HTTP API : HTML이 아니라 데이터를 전달(주로 JSON)
   - SSR(서버 사이드 렌더링) : 서버에서 최종 HTML을 생성에서 클라이언트에 전달(JSP, 타임리프) <- 정적인 화면
   - CSR(클라이언트 사이드 렌더링) : HTML 결과를 자바스크립트를 사용해 웹 브라우저에서 동적으로 생성해서 적용(React, Vue.js) <- 동적인 화면
 - 자바 백엔드 웹 기술 역사
-------------
#### 2. 서블릿
 - 클라이언트의 요청을 처리하고, 그 결과를 반환하는 Servlet 클래스의 구현 규칙을 지킨 자바 웹 프로그래밍 기술
 - HttpServletRequest
   - https://github.com/Inflearn-Springboot/SpringMVC-1-/blob/main/servlet/src/main/java/hello/servlet/basic/request/RequestHeaderServlet.java

   - **GET - 쿼리 파라미터** : (/url**?username=hello&age=20**) 메시지 바디 없이, URL의 쿼리 파라미터에 데이터를 포함해서 전달 예) 검색, 필터, 페이징등에서 많이 사용하는 방식
   - **POST - HTML Form** : (content-type: application/x-www-form-urlencoded)메시지 바디에 쿼리 파리미터 형식으로 전달 username=hello&age=20 예) 회원 가입, 상품 주문, HTML Form 사용
   - **HTTP message body**에 데이터를 직접 담아서 요청 HTTP API에서 주로 사용, JSON, XML, TEXT <- 데이터 형식은 주로 JSON 사용 POST, PUT, PATCH
 - HTTP 요청 데이터 - GET, POST
   - GET : `request.getParameterNames().asIterator()
                .forEachRemaining(paramName -> System.out.println(paramName + "=" + request.getParameter(paramName)));`
   - POST : application/x-www-form-urlencoded` 형식은 앞서 GET에서 살펴본 쿼리 파라미터 형식과 같다. 따라서 **쿼리 파라미터 조회 메서드를 그대로 사용**하면 된다.
 - HTTP 요청 데이터 - 단순 텍스트, JSON
   - 단순 텍스트 : `ServletInputStream inputStream = request.getInputStream();
        String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);`
   - JSON : HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);
     <img width="200" alt="image" src="https://github.com/Inflearn-Springboot/SpringMVC-1-/assets/96871403/d934cd8a-83a2-4af4-9191-1de69f01c03c">

 - HttpServletResponse (HTTP 응답 데이터 - HTML, API JSON)
   - https://github.com/Inflearn-Springboot/SpringMVC-1-/tree/main/servlet/src/main/java/hello/servlet/basic/response
-------------
#### 3. 서블릿, JSP, MVC 패턴
 - 서블릿
   - 자바 코드로 HTML을 만들어 내는 것 보다 차라리 HTML 문서에 동적으로 변경해야 하는 부분만 자바 코드를 넣을 수 있다면 더 편리할 것이다.
   - 템플릿 엔진을 사용하면 HTML 문서에서 필요한 곳만 코드를 적용해서 동적으로 변경할 수 있다.
템플릿 엔진에는 JSP, Thymeleaf, Freemarker, Velocity등이 있다.
 - JSP : `<% ~ %>` 를 사용해서 HTML 중간에 자바 코드를 출력하고 있다.
   - 한계 : JAVA 코드, 데이터를 조회하는 리포지토리 등등 다양한 코드가 모두 JSP에 노출되어 있다.
 - **MVC 패턴**
   - **컨트롤러**: HTTP 요청을 받아서 파라미터를 검증하고, 비즈니스 로직을 실행한다. 그리고 뷰에 전달할 결과 데이터를 조회해서 모델에 담는다.
   - **모델**: 뷰에 출력할 데이터를 담아둔다. 뷰가 필요한 데이터를 모두 모델에 담아서 전달해주는 덕분에 뷰는 비즈니스 로직이나 데이터 접근을 몰라도 되고, 화면을 렌더링 하는 일에 집중할 수 있다.
   - **뷰**: 모델에 담겨있는 데이터를 사용해서 화면을 그리는 일에 집중한다. 여기서는 HTML을 생성하는 부분을 말한다.
   - 한계 : 공통 처리가 어렵다는 문제가 있다 -> 프론트 컨트롤러 패턴 도입(수문장 역할)
  
   
  **redirect vs forward**
리다이렉트는 실제 클라이언트(웹 브라우저)에 응답이 나갔다가, 클라이언트가 redirect 경로로 다시 요청한다. 따라서 클라이언트가 인지할 수 있고, URL 경로도 실제로 변경된다. 반면에 포워드는 서버 내부에서 일어나는 호 출이기 때문에 클라이언트가 전혀 인지하지 못한다.

-------------
#### 4. MVC 프레임워크 만들기
 - 프론트 컨트롤러 패턴

   <img width="470" alt="image" src="https://github.com/Inflearn-Springboot/SpringMVC-1-/assets/96871403/969c8962-52d6-41b6-8b37-c82e53df3ef6">

   - 스프링 웹 MVC의 "DispatcherServlet"이 FrontController 패턴으로 구현되어있음
 - View 분리 : MyView 클래스 분리
![image](https://github.com/Inflearn-Springboot/SpringMVC-1-/assets/96871403/01bf31b7-3ef2-4ba0-becf-a0e27417bdbc)
 - Model 추가 : 서블릿 종속성 제거
request 객체를 Model로 사용하는 대신에 별도의 Model 객체를 만들어서 반환하면 된다.

![image](https://github.com/Inflearn-Springboot/SpringMVC-1-/assets/96871403/33162f73-52c5-4fb0-8d4b-f5074d7195de)

 - 실용적이고 유연한 컨트롤러로 업그레이드
![image](https://github.com/Inflearn-Springboot/SpringMVC-1-/assets/96871403/42662142-ca16-4724-a034-57b93e8df23c)

-------------
#### 5. 스프링 MVC - 구조 이해

- 스프링 MVC 전체구조
  ![image](https://github.com/Inflearn-Springboot/SpringMVC-1-/assets/96871403/18acdece-3b6c-4ce8-a4b6-57c639360c5c)
DispatcherServlet 서블릿 등록
DispatcherServlet 도 부모 클래스에서 HttpServlet 을 상속 받아서 사용하고, 서블릿으로 동작한다.
DispatcherServlet FrameworkServlet HttpServletBean HttpServlet
스프링 부트는 DispatcherServlet 을 서블릿으로 자동으로 등록하면서 모든 경로( urlPatterns="/" )에
대해서 매핑한다.
참고: 더 자세한 경로가 우선순위가 높다. 그래서 기존에 등록한 서블릿도 함께 동작한다.

동작 순서 : 핸들러 조회 -> 핸들러 어댑터 조회 -> 핸들러 어댑터 실행 -> 핸들러 실행 -> viewResolver 호출 -> view 반환 -> 뷰 렌더링

- 핸들러 매핑과 핸들러 어댑터

HandlerMapping(핸들러 매핑)
핸들러 매핑에서 이 컨트롤러를 찾을 수 있어야 한다.
예) 스프링 빈의 이름으로 핸들러를 찾을 수 있는 핸들러 매핑이 필요하다.
HandlerAdapter(핸들러 어댑터)
핸들러 매핑을 통해서 찾은 핸들러를 실행할 수 있는 핸들러 어댑터가 필요하다.
예) Controller 인터페이스를 실행할 수 있는 핸들러 어댑터를 찾고 실행해야 한다.

- 뷰 리졸버
  
application.properties 에 다음 코드를 추가하자
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp

스프링 부트는 InternalResourceViewResolver 라는 뷰 리졸버를 자동으로 등록하는데, 이때 application.properties 에 등록한 spring.mvc.view.prefix , spring.mvc.view.suffix 설정 정보를 사용해서 등록한다.

- 스프링 MVC
  
@Controller :
스프링이 자동으로 스프링 빈으로 등록한다. (내부에 @Component 애노테이션이 있어서 컴포넌트 스캔의대상이 됨)
스프링 MVC에서 애노테이션 기반 컨트롤러로 인식한다.
@RequestMapping : 요청 정보를 매핑한다. 해당 URL이 호출되면 이 메서드가 호출된다. 
애노테이션을 기반으로 동작하기 때문에, 메서드의 이름은 임의로 지으면 된다.
ModelAndView : 모델과 뷰 정보를 담아서 반환하면 된다.

-------------
#### 6. 스프링 MVC - 기본 기능

@RestController:
@Controller 는 반환 값이 String 이면 뷰 이름으로 인식된다. 그래서 뷰를 찾고 뷰가 랜더링 된다.
@RestController 는 반환 값으로 뷰를 찾는 것이 아니라, HTTP 메시지 바디에 바로 입력한다. 따라서 실행 결과로 ok 메세지를 받을 수 있다. @ResponseBody 와 관련이 있는데, 뒤에서 더 자세히 설명한다.

@Slf4j: 코드를 자동으로 생성해서 로그를 선언해준다. 개발자는 편리하게 log 라고 사용하면 된다.

스프링MVC는 @ModelAttribute 가 있으면 다음을 실행한다.
HelloData 객체를 생성한다.요청 파라미터의 이름으로 HelloData 객체의 프로퍼티를 찾는다. 그리고 해당 프로퍼티의 setter를 호출해서 파라미터의 값을 입력(바인딩) 한다.
예) 파라미터 이름이 username 이면 setUsername() 메서드를 찾아서 호출하면서 값을 입력한다.

@RequestBody:
@RequestBody 를 사용하면 HTTP 메시지 바디 정보를 편리하게 조회할 수 있다. 참고로 헤더 정보가 필요하다면
HttpEntity 를 사용하거나 @RequestHeader 를 사용하면 된다.

@RestController:
@Controller 대신에 @RestController 애노테이션을 사용하면, 해당 컨트롤러에 모두 @ResponseBody 가
적용되는 효과가 있다. 따라서 뷰 템플릿을 사용하는 것이 아니라, HTTP 메시지 바디에 직접 데이터를 입력한다. 이름
그대로 Rest API(HTTP API)를 만들 때 사용하는 컨트롤러이다.

![image](https://github.com/Inflearn-Springboot/SpringMVC-1-/assets/96871403/d423c30f-9f9a-4796-a620-434459d3b4b5)

-------------
#### 7. 스프링 MVC - 웹 페이지 만들기
