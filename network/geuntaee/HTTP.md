이번시간에는 Http( HyperText Transfer Protocol )에 대해서 알아보려고 한다.
먼저 Http를 제대로 공부하려면 Http의 역사에 대해서 알아야한다고 한다.
그래서 먼저 Http Version에 대해서 알아보자.

## HTTP의 진화
Http는 월드 와이드 웹에 내재된 프로토콜로 Tim Berners-Lee에 의해 1989년부터 1991년까지 발명되었다.
본래의 단순함을 대부분 지키면서 확장성 위에서 만들어지도록 설계되었다.
Http는 의사-신뢰도가 있는 실험실 환경에서 파일을 교환하는 프로토콜에서 높은 수준의 분석과 3D 내에서 이미지와 비디오를 실어나르는 현대 인터넷으로 진화했다.

### 월드 와이드 웹의 발명
Tim Berners-Lee는 인터넷 상의 하이터텍스트 시스템을 만들기 위해 초기에 Mesh라고 불리던 것을 제작하였고, 제작 중 Mesh는 1990년에 월드 와이드 웹으로 이름을 바꾸었다.
기존의 TCP 그리고 IP 프로토콜 상에서 만들어지면서 4개의 빌딩 블록으로 구성되어 있다.
- 하이퍼텍스트 문서를 표현하기 위한 텍스트 형식의 하이퍼텍스트 마크업 언어( HTML )

- 문서 같은 것을 교환하기 위한 간단한 프로토콜인 하이퍼텍스트 전송 프로토콜( HTTP )

- 문서를 우발적으로 수정하기 위한 클라이언트인 월드 와이드 웹이라고 불리는 첫 번째 브라우저

- 문서에 접근하도록 해주는 Httpd의 초기 버전

### Http / 0.9 - 원 - 라인 프로토콜
Http 초기 버전에는 버전 번호가 없었다. Http/0.9 이후에 차후 버전과 구별하기 위해 0.9로 불리게 됐다.
Http의 0.9버전은 굉장히 단순하다. 요청은 단일 라인으로 구성되며, 리소스에 대한 프로토콜, 서버 그리고 포트는 서버가 연결되고 나면 불필요하여 URL은 아닌 경로로 가능한 메스드는 GET이 유일했다.
```html
GET /mypage.html
```
응답 또한 단순하다.
```
<HTML>
A very simple HTML page
</HTML>
```

### Http / 1.0 - 확장성 만들기
Http/0.9는 매우 제한적이었으며 브라우저와 서버 모두 융통성을 가지도록 확장되었다.

- 브라우저 친화적인 프로토콜
- 요청 및 응답에 대한 메타 데이터를 포함하는 헤더 필드 제공( Status code, Content-Type 등 )
- Response : Content-Type에 Http 파일 외에도 스크립트, 스타일 시트, 미디어 등을 전송 가능
- Method : GET, HEAD, POST
- Connection 특성 : 응답 직후 종료
![](https://velog.velcdn.com/images/dymnam/post/ba896c97-b62d-46c5-9537-89e103309656/image.png)
- 단점 : 각 모든 요청에 따라 **새로운 연결**을 열고, 응답이 전송된 후 즉시 닫기 때문에 새로운 연결이 설정 될 때마다 TCP의 3-Way Handshake가 발생한다.

### Http / 1.1 - 표준 프로토콜
- 오늘 날 가장 많이 사용되는 Http 버전
- 영구 및 파이프 라인 연결, 압축/압축 해제, 가상 호스팅, 캐시 등이 추가되어 응답속도가 빨라지고 대역폭이 절약되는 등 성능 최적화 및 기능 향상
- Upgrade로 Web Socket 전환 가능
- Method : GET, HEAD, POST, PUT, DELETE, TRACE, OPTIONS
- Connection 특성 : Long-lived

```
예제

GET /en-US/docs/Glossary/Simple_header HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://developer.mozilla.org/en-US/docs/Glossary/Simple_header

200 OK
Connection: Keep-Alive
Content-Encoding: gzip
Content-Type: text/html; charset=utf-8
Date: Wed, 20 Jul 2016 10:55:30 GMT
Etag: "547fa7e369ef56031dd3bff2ace9fc0832eb251a"
Keep-Alive: timeout=5, max=1000
Last-Modified: Tue, 19 Jul 2016 00:59:33 GMT
Server: Apache
Transfer-Encoding: chunked
Vary: Cookie, Accept-Encoding

(content)


GET /static/img/header-background.png HTTP/1.1
Host: developer.mozilla.org
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.9; rv:50.0) Gecko/20100101 Firefox/50.0
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Referer: https://developer.mozilla.org/en-US/docs/Glossary/Simple_header

200 OK
Age: 9578461
Cache-Control: public, max-age=315360000
Connection: keep-alive
Content-Length: 3077
Content-Type: image/png
Date: Thu, 31 Mar 2016 13:34:46 GMT
Last-Modified: Wed, 21 Oct 2015 18:27:50 GMT
Server: Apache

(image content of 3077 bytes)
```
- 연결을 설정하기 전에 TCP의 3-Way Handshake 발생, 마지막으로 모든 데이터를 Client로 보낸 후 Server는 더 이상 보낼 데이터가 없다는 메세지를 보내고, 그 이후 연결을 닫는다.

- 즉, Http 1.1은 여러 Request-Response에 대해 동일한 연결을 재사용할 수 있다.

### Http 1.1 Keep Alive & Pipeline
![](https://velog.velcdn.com/images/dymnam/post/27e99281-2584-4e50-8a32-1c06c62e6c32/image.png)
- Http 1.0 : TCP Connection은 Http 요청마다 3-Way Handshake와 TearDown을 반복.

- Http 1.1 : 하나의 TCP Connection이 열려있으면( Established 상태 ), 그 연결을 통해 여러 Request에 대한 Response를 받을 수 있다.

![](https://velog.velcdn.com/images/dymnam/post/48e95a5c-e934-424a-9694-8601b0a7676a/image.png)
- Http 1.1 Keep-Alive Pipeline : Pipelining을 사용할 때, Client는 여러 request를 response의 응답을 기다리지 않고 보낼 수 있다.

- Http 1.1 Keep-Alive Multiple Connections : 클라이언트는 많은 양의 Objects를 검색하는 성능을 높이기 위해 TCP 다중 연결을 할 수 있다.

### Http 2.0 - 더 나은 성능을 위한 프로토콜 
![](https://velog.velcdn.com/images/dymnam/post/39401910-1370-41e6-baa7-510e251798b2/image.png)
Http 2.0은 Http 1.1 프로토콜을 계승해 동일한 API 이면서 성능 향상에 초점을 맞췄다.
기존에 Plain Text( 평문 )을 사용하고 개행으로 구별되던 Http/1.x 프로토콜과 달리 2.0은 바이너리 포맷으로 인코딩 된 Message, Frame으로 구성된다.

- Stream : 구성된 연결 내에서 전달되는 바이트의 양방향 흐름으로 하나 이상의 메세지가 전달 가능하다.

- Message : 논리적 요청 또는 응답 메세지에 매핑되는 프레임의 전체 시퀀스이다.

- Frame : Http/2.0에서 통신의 최소 단위, 각 최소 단위에는 하나의 프라임 헤더가 포함된다. 이 프라임 헤더는 최소한으로 프레임이 속하는 스트림을 식별한다. Headers Type Frame, Data Type Frame 등

#### Multiplexed Stream
- 한 Connection으로 동시에 여려 개 메세지를 주고 받을 수 있으며, Response는 순서에 상관 없이 Stream으로 주고 받는다.

#### Stream Prioritzation
- 리소스간 우선순위를 설정해 클라이언트가 먼저 필요한 리소스부터 보내준다.

#### Server Push
- 서버는 클라이언트의 요청에 대해 요청하지 않은 리소스를 마음대로 보내줄 수 있다.
- 즉, 클라이언트가 요청하기 전에 필요하다고 예상되는 리소스를 Server에서 먼저 요청한다.

#### Header Compression
- Header Table과 Huffman Encoding 기법( HPAC 압축방식 )을 이용해 압축했다.
- 이전 Header의 내용과 중복되는 필드를 재전송하지 않아 데이터를 절약했다.
![](https://velog.velcdn.com/images/dymnam/post/2cdb9c0d-a697-43a8-be60-4e2e5c592368/image.png)

## Http 1.0 VS 1.1 VS 2.0
![](https://velog.velcdn.com/images/dymnam/post/8eec9f28-032f-4b7c-a71e-3441c0792524/image.png)
