## Web Server
-> 클라이언트( 사용자 )가 웹 브라우저에 요청을 보내면 웹 서버에서 요청을 받아 정적 컨텐츠( HTML, CSS, JS 등.. )를 제공하는 서버이다.
- 대표적인 웹서버 : Apache

![](https://velog.velcdn.com/images/dymnam/post/7d153324-22c8-4e9a-ba0d-97e5c365b441/image.png)

## Web Application Server( WAS )
-> 웹 서버와 웹 컨테이너가 합쳐진 형태로, 웹 서버 단독으로 처리할 수 없는 데이터베이스의 조회 등 다양한 로직 처리가 필요한 동적 컨텐츠( JSP, Servlet ) 구동환경을 제공해준다.
- 대표적인 WAS : Tomcat

![](https://velog.velcdn.com/images/dymnam/post/b5f0aeed-0162-4c9f-bd11-dc7adb7a6dd7/image.png)

## Web Service Architecture( 구조 )
WAS는 요청 처리 방식에 따라 다양한 구조를 가질 수 있다.
그 중에서 아래와 같은 구조의 동작 과정을 알아보자.
- 클라이언트( 사용자 ) ->  Web Server -> WAS -> DB
![](https://velog.velcdn.com/images/dymnam/post/8513bb9d-440d-4f26-92a9-3f704dbe223f/image.png)

1. 클라이언트( 사용자 ) -> 웹 서버로 HTTP 요청을 받음.

2. 웹 서버의 클라이언트의 요청 -> WAS로 보냄.

3. WAS가 관련된 Servlet을 메모리에 올림.

4. WAS가 web.xml을 참조 후 Thread를 생성.

5. HttpServletRequest와 HttpServletResponse 객체를 생성하여 Servlet에 전달

5-1. Thread는 Servlet의 service() 메서드 호출.

6. doGet() 또는 doPost() 메서드 인자에 맞게 생성된 적절한 동적 페이지를 응답 객체에 담아 WAS에 전달.

7. WAS는 응답 객체를 HttpResponse 형태로 바꿔 웹 서버로 전달.

8. 생성된 Thread를 종료, HttpServletRequest와 HttpServletResponse 객체를 제거.

## WebServer VS WebApplicationServer
웹서버는 정적 컨텐츠를 제공, WAS는 동적 컨텐츠를 제공하는데 목적을 둔다.
-> 웹서버 또는 WAS만 쓰는 것은 효율성이 떨어진다. 단순한 정적 컨텐츠는 웹서버에게 맡기고, 기능을 분리시켜 서버 부하를 방지한다.

결과적으로 나온 것이 Tomcat 5.5 버전부터 정적 컨텐츠를 처리하는 기능이 추가되었는데, 정적 컨텐츠의 처리 기능이 Apache으 기능을 포함하고 있어 Apache Tomcat이라고 부르고 있다 한다.
