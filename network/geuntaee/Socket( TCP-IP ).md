TCP/IP의 Socket 통신의 클라이언트 프로그램과 서버 프로그램은 각각 자신이 포트를 통해 통신해야 한다.
연결을 할때에도 포트를 사용하고 데이터를 교환할때도 포트를 사용한다.
자바 프로그램 안에서 포트를 사용하기 위해서는 소켓을 사용해야하는데, 자바 안에서 소켓의 종류에는 서버 소켓과 클라이언트 소켓이 있다.

#### Server Socket( 서버 소켓 )
- 서버 소켓은 말 그대로 서버 프로그램안에서만 사용하는 소켓으로 서버 소켓은 클라이언트로부터 연결 요청이 오기를 기다렸다가 연결 요청이 들어오면 클라이언트와 연결을 맺고 다른 소켓을 만드는 일을 한다.

#### Client Socket( 클라이언트 소켓 )
- 클라이언트 소켓은 기다릴 필요가 없기 때문에 바로 생성 가능하다.
- 클라이언트 프로그램에서 클라이언트 소켓은 서버프로그램으로 연결 요청을 하는 것과 데이터 전송을 하는 일을 한다.

## Socket 이전의 통신
Socket 이전에 양방향 통신을 위해서 HTTP 통신을 사용하였다.
Socket을 사용하는 통신을 소켓 프로그래밍, HTTP를 사용하는 것을 HTTP 프로그래밍이라 한다.
but, HTTP 통신에서도 소켓을 사용한다고 했으니 꼭 소켓을 사용한다고 해서 소켓 프로그래밍이라고 하지 않는다.
즉, 해당 소켓이 어떤 용도로 사용되었냐에 따라 HTTP 프로그래밍이 될 수 있고, 소켓 프로그래밍이 될 수 있는 것이다.
-> HTTP 프로그래밍은 단방향 통신이 가능하게 하며, HTTP 통신은 html 파일을 전송하거나 JSON, IMAGE 파일 등을 전송하는데 사용된다.

## Socket의 통신 흐름
![](https://velog.velcdn.com/images/dymnam/post/bdbcabb0-1c1c-43d3-8104-c9a5774f77cf/image.png)
#### Server Socket
1. 소켓( Socket )을 생성( Create )함.
2. 서버가 사용할 IP 주소와 포트 번호를 생성한 소켓에 결합( Bind )시킴.
3. 클라이언트로부터 연결 요청이 수신되는지 주시( Listen )함.
4. 요청이 수신되면 요청을 받아들여( Accept ) 데이터 통신을 위한 '새로운' 소켓을 생성함.
5. 연결 후 데이터를 송수신( Send/Recv )
6. 데이터 송수신이 완료도면 소켓( Socket )을 닫음( Close ).

#### Client Socket
1. 소켓( Socket )을 생성( Create ) 함.
2. 서버 측에 연결( Connect )을 요청.
3. 서버 소켓에서 연결이 받아들여지면 데이터를 송수신( Send/Recv )
4. 모든 처리가 완료되면 소켓( Socket )을 닫음( Close )

#### 주의 사항
- 최종적으로 클라이언트 소켓( Client Socket )과 연결( Connection )이 만들어지는 소켓( Socket )은 앞서 사용한 서버 소켓( Server Socket )이 아니라, Accept API 내부에서 새로 만들어지는 소켓이다.

- 실질적인 데이터 송수신은 Accept API에서 생성된 연결( Connection )이 수립( Established )된 소켓을 통해 처리됨.

#### Server Socket 사용방법
```java
ServerSocket server = new ServerSocket( 포트번호 );
```
- 클라이언트로부터 연결 요청이 들어오면 연결을 맺고 클라이언트 소켓을 생성해 리턴
```java
Socket socket = server.accept();
```

#### Clinet Socket 사용방법
```java
Socket socket = new Socket( 서버 아이피번호, 서버 포트번호 );
```

#### Server & Client 간 데이터 전송방법
- 데이터 수신에 사용할 입력 스트림 객체를 리턴
```java
InputStream input = socket.getInputStream();
```
- 데이터 송신에 사용할 출력 스트림 객체를 리턴
```java
OutputStream output = socket.getOutputStream();
```
- 파라미터로 넘겨준 데이터 송신
```java
String data = "hello"
output.write(data);
```
- 수신된 데이터를 읽어서 리턴
```java
String data = input.read();
```
- 소켓을 닫는 메서드
```java
socket.close();
```
- 서버 소켓을 닫는 메서드
```java
serverSocket.close();
```
