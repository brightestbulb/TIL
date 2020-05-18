## 서블릿 컨테이너 ( Tomcat )

- 서블릿 컨테이너는 서블릿 저장소라고 봐도 된다.
- 자바로 웹 개발을 하기 위해 여러 서블릿 들이 필요하게 된다.
- 서블릿 컨테이너는 개발자가 웹 서버와 통신하기 위하여 소켓을 생성하고 특정 포트에 리스닝하고 스트림을 생성하는 등의 복잡한 일들을 대신 해준다.   
- 컨테이너는 servlet의 생성부터 소멸까지의 Life Cycle을 관리하고, 서블릿 컨테이너는 요청이 들어올 때마다 새로운 자바 스레드를 만든다.    
- 톰캣같은 WAS가 .java를 컴파일해서 Class로 만들고 메모리에 올려 Servlet 객체를 만든다.    
- 스프링 MVC도 서블릿 컨테이너가 관리하고 있는 서블릿 중 하나이다.
- 서블릿 없이 스프링 MVC만 있으면 된다고 하는것은 비즈니스 로직을 스프링을 통해 처리하겠다는 것이지 서블릿이 필요없다는 것이 아니다.


**Servlet 동작 과정**
1. 사용자가 URL을 입력하면 HTTP Request를 Servlet Container에 보낸다.
2. Servlet Container는 HttpServletRequest, HttpServletResponse 두 객체를 생성한다.
3. 사용자가 요청한 URL을 분석하여 어느 서블릿에 대한 요청인지 찾는다.
4. 컨테이너는 서블릿의 service() 메소드를 호출하며, POST, GET 여부에 따라 doGet() 또는 doPost()가 호출된다.
5. doGet(), doPost() 메소드는 동적인 페이지를 생성한 후 HttpServletResponse 객체에 응답을 담아 보낸다.
6. 응답이 완료되면 HttpServletRequest, HttpServletResponse 객체를 소멸시킨다.


**서블릿 생명 주기**
- init() : 서버가 켜질때 한번만 실행
- service : 모든 유저의 요청들을 받는다.(doGet(), doPost())
- destroy() : 서버가 꺼질때 한번만 실행


출처 :    
https://jojoldu.tistory.com/28    
https://minwan1.github.io/2017/10/08/2017-10-08-Spring-Container,Servlet-Container/    
https://minwan1.github.io/2018/11/21/2018-11-21-jsp-springboot-%EB%8F%99%EC%9E%91%EA%B3%BC%EC%A0%95/   