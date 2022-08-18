## CORS( Cross Origin Resource Sharing )
CORS( Cross Origin Resource Sharing )는 Origin( 출처 )를 교차하여 자원을 공유한다는 뜻이다.
즉, 한 출처에서 실행 중인 웹 애플리케이션이 **다른 출처의 선택한 자원에 접근할 수 있는 권한을 부여**하도록 브라우저에 알려주는 체제이다.

출처가 A일 때 B의 API 서버로의 리소스 요청은 SOP( Same Origin Policy )에 의해 제한 API를 사용하는 웹 어플리케이션은 자신의 출처와 동일한 리소스만 불러올 수 있으며 다른 출처의 리소스를 불러오면 그 출처에 올바른 CORS 헤더를 포함한 응답을 반환 해야한다.
-> 자원을 요청한 출처( A )와 해당 요청을 응답하는 서버의 출처( B )가 다를 경우에도 해당 자원에 접근할 수 있는 권한을 부여하도록 브라우저에게 알려주는 정책.

### Origin
![](https://velog.velcdn.com/images/dymnam/post/effc5d4a-d9bb-4676-85d3-cb705fec4032/image.png)
URL 구조에서 프로토콜을 포함한 도메인( Protocol + Host + Port )를 말한다.
Port가 다르면 다른 출처로 본다.

## CORS 동작 과정
1. CORS요청시 미리 OPTIONS 메소드로 preflight를 보내 서버가 CORS를 허용하는지 물어본다.

2. 이때 Access-Control-Request-Method로 실제 보내고자하는 메서드를 알리고, Access-Control-Request-Headers로 실제 보내고자 하는 헤더들을 알린다.

3. Allow항목들은 Request에 대응되는 것으로, 서버가 허용하는 메서드와 헤더를 응답하는데 사용된다.

4. Request와 Allow가 일치하면 CORS요청이 이루어진다.

요청 헤더의 Origin 헤더를 보면 어떤 도메인으로부터 요청이 왔는지 알 수 있다.
![](https://velog.velcdn.com/images/dymnam/post/66b7777e-b327-467f-a7d9-b9fe7044c180/image.png)

이에 대한 응답으로 서버는 Access-Control-Allow-Origin 헤더를 다시 전송한다.
![](https://velog.velcdn.com/images/dymnam/post/aa017a89-add4-44da-97d3-6314c4935419/image.png)

![](https://velog.velcdn.com/images/dymnam/post/399325d9-b883-4ef3-a17a-e59914b848f9/image.png)

## CORS 관련 응답 헤더
이 헤더들이 cors preflight요청( option 메소드 )에 대한 서버측 응답에 포함되어 있어야 브라우저가 이를 보고 cors를 허용하고 실제 요청이 전송된다.

- Access-Contorl-Allow-Origin
CORS 요청을 허용할 사이트 ( e.g. https://oddpoet.net )

- Access-Contorl-Allow-Method
CORS 요청을 허용할 Http Method들 ( e.g. GET,PUT,POST )

- Access-Contorl-Allow-Headers
특정 헤더를 가진 경우에만 CORS 요청을 허용할 경우

- Access-Contorl-Allow-Credencial
자격증명과 함께 요청을 할 수 있는지 여부.
해당 서버에서 Authorization로 사용자 인증도 서비스할 것이라면 true로 응답해야 할 것이다.

- Access-Contorl-Max-Age
preflight 요청의 캐시 시간.

## CORS 문제를 해결하기 위한 방법
1. 동일한 도메인 서버에서 자원을 요청한다.
2. 서버에서 Origin을 설정해준다. 서버에서 요청 받을 때, 해당 Client의 Origin에 대해 허가해주는 정책을 추가.
