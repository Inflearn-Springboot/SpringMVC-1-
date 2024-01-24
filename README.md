# 2-1 스프링MVC - 백엔드 웹 개발 핵심 기술
 웹 애플리케이션을 개발할 때 필요한 모든 웹 기술 기초 그리고 스프링 MVC의 핵심 원리와 구조 이해

### 전체 목차
1. 웹 애플리케이션 이해
2. 서블릿
3. 서블릿, JSP, MVC 패턴
4. MVC 프레임워크 만들기
5. 스프링 MVC - 구조 이해
6. 스프링 MVC - 기본 기능
7. 스프링 MVC - 웹 페이지 만들기

-------------
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
-------------
#### 4. MVC 프레임워크 만들기
-------------
#### 5. 스프링 MVC - 구조 이해
-------------
#### 6. 스프링 MVC - 기본 기능
-------------
#### 7. 스프링 MVC - 웹 페이지 만들기
