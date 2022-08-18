## OSI 7Layer
OSI 7Layer는 통신이 일어나는 7단계 과정을 나타낸 것으로 통신이 일어나는 과정을 단계별로 파악해 특정 부분에서 이상이 생기면 통신 장애를 일으킨 단계에서 해결할 수 있다.
![](https://velog.velcdn.com/images/dymnam/post/c2abfa28-a681-4fbd-9e5a-88b7fa9a116e/image.png)

#### 1계층 물리 계층( Physical Layer )
- 인터넷 케이블, 라우터 스위치 등의 전기적 신호가 통신하는 계층
-> 에러 파악 X, 데이터를 전기적인 신호로 변환

#### 2계층 데이터 링크 계층( Data Link Layer )
- 물리계층을 통해 송수신되는 정보의 오류와 흐름을 관리하여 안전한 정보의 전달을 수행할 수 있도록 도와주는 역할을 하는 계층
-> 에러 파악 O, 재전송 O, 맥 주소를 가지고 통신

#### 3계층 네트워크 계층( Network Layer )
- 라우팅, 흐름 제어, 세그먼테이션, 오류 제어, 인터네트워킹 등을 수행
-> 데이터를 안전하고 빠르게 전달하는 기능( 라우팅 ), 논리적인 주소( IP )를 가지고 계층적 구조

#### 4계층( Transport Layer )
- 통신을 활성화하기 위한 계층으로 TCP 프로토콜 사용
-> 오류제어 및 흐름제어, 이 계층까지 물리적인 계층( TCP/UDP 사용 )

#### 5계층( Session Layer )
- 통신 연결이 손실되면 연결 복구 시도가 가능하며, 장시간 연결되지 않으면 세션 계층의 프로토콜이 연결을 닫고 다시 연결
-> 전이중 통신, 반이중 통신이 있다.

#### 6계층( Presentation Layer )
- 코드 간 번역을 담당하는 계층으로 인코딩이나 암호화 등의 동작이 일어남.
-> 응용프로그램이나 네트워크를 위해 데이터를 표현하는 단계

#### 7계층( Application Layer )
- 응용 프로세스와 직접 관계하여 응용 서비스를 수행하는 계층
-> 크롬, 파이어폭스 등 실제로 서비스하는 계층

## TCP/IP 4Layer
OSI 7Layer는 장비 개발과 통신을 표준으로 잡아둔 것일 뿐, 실질적 통신은 TCP/IP 프로토콜을 사용한다.

![](https://velog.velcdn.com/images/dymnam/post/a9b55524-6a0b-4cef-9456-e92b5b8cf840/image.png)

#### 1계층( Network Access Layer )
- 물리계층 + 데이터링크 계층, OS의 네트워크 카드와 드라이버 등 하드웨어적인 요소와 관련되는 모든 것을 지원하는 계층

#### 2계층( Internet Layer )
- 네트워크 계층, 상위 프랜스포트 계층으로부터 받은 데이터에 IP패킷 헤더를 붙여 IP패킷을 만들어 전송

#### 3계층( Transport Layer )
- 전송 계층, 네트워크 사이의 신뢰성 있는 전송 기능을 제공하고, TCP/UDP 프로토콜을 사용한다.

#### 4계층( Application Layer )
- 세션 + 프레젠테이션 + 애플리케이션 계층, 응용프로그램들이 서비스할 수 있도록 표준적인 인터페이스를 제공한다.

## TCP/IP 5Layer
공식적으로는 TCP/IP 4Layer이지만 OSI 계층이 생겨남에 따라 Network Interface 계층을 나눠서 5계층으로 업그레이드 된 것이다. ( OSI 7 + TCP/IP 4 )

![](https://velog.velcdn.com/images/dymnam/post/9395ea06-85d4-42e3-b2be-f4ab90ea18ed/image.png)
- TCP/IP 5Layer는 Network Interface 계층을 Data Link와 Physical로 나눈 것이다.
나눈 것인지 나누지 않은 것인지에 따라 5Layer과 4Layer로 구분된다.
