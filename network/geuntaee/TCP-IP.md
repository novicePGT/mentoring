이번에는 Network관리의 TCP/IP에 대해서 알아볼 것이다.
시작하기에 앞서 TCP는 Connection - Oriented( 연결 지향 ) 프로토콜임을 인지해두자.

## TCP / IP
컴퓨터가 서로 통신하는 경우, 특정 규칙이나 프로토콜을 사용하여 순서대로 데이터를 전송 및 수신할 수 있다.
전세계를 통해 가장 일상적으로 사용하는 프로토콜 세트 중 하나가 **TCP/IP(Transmission Control Protocol/Internet Protocol)** 이다.
- 전송계층에서 사용하는 프로토콜
- 장치들 사이에서 논리적 접촉을 성립하기 위해 연결을 설정해 **신뢰성**을 보정하는 연결형 서비스
- 네트워크에 연결된 컴퓨터에서 실행되는 프로그램간에 일련의 옥텟( 데이터, 메세지, 세그먼트, 블록단위 )을 순서와 함께 에러 없이 안정적으로 교환할 수 있게 해준다.

## TCP / IP 용어
TCP/IP에 관련하여 사용되는 유용한 인터넷 용어를 알고 있는 것은 많은 도움을 준다.

- Client : 네트워크 프로세스나 다른 컴퓨터의 데이터, 서비스 또는 자원들을 액세스하는 컴퓨터 또는 프로세스이다.

- Host : 인터넷 네트워크에 접속되고 다른 인터넷 호스트와 통신할 수 있는 컴퓨터로 특정 사용자에 대한 로컬 호스트는 해당 사용자가 작업 중인 컴퓨터이다.

- Network : 둘 이상의 호스트 및 이들 사이의 연결 링크 조합으로 물리적 네트워크는 네트워크를 구성하는 하드웨어이다.
논리적 네트워크는 전체 또는 일부의 하나 이상의 물리적 네트워크에 있는 추상적 구조이다.

- Packet : 호스트와 네트워크 사이의 한 트랜잭션에 대한 제어 정보 및 데이터 블록이다.

- Port : 프로세스에 대한 논리적 연결 지점으로 데이터는 포트(또는 소켓)을 통해 프로세스 사이에서 전송된다.

- Process : 실행 중인 프로그램을 뜻하고, 프로세스는 컴퓨터에서 활동 중인 요소이다.

- Protocol : 물리적 또는 노리적 레벨로 통신을 처리하는 규칙 세트이다.

- Server : 네트워크 상의 다른 컴퓨터 또는 프로세스가 액세스할 수 있는 데이터, 서비스 또는 자원을 제공하는 컴퓨터 또는 프로세스이다.

## TCP/IP 4계층
![](https://velog.velcdn.com/images/dymnam/post/25404458-91fc-4a63-9611-b917b90696ce/image.png)

TCP와 UDP는 TCP/IP의 **전송계층**에서 사용되는 프로토콜이다.
<span style=”color:indianred”>전송계층은 IP에 의해 전달되는 패킷의 오류를 검사하고 재전송하는 요구 등의 제어를 담당하는 계층</span>이다.

- 1계층 : 네트워크 액세스 계층( Network Access Layer )
OSI 7계층의 물리계층과 데이터 링크 계층에 해당하고, 물리적인 주소로 MAC을 사용한다.

- 2계층 : 인터넷 계층( Internet Layer )
OSI 7계층의 네트워크 계층에 해당하고, 통신 노드 간의 IP패킷을 전송하는 기능과 라우팅 기능을 담당한다.
프로토콜 - IP, ARP, RARP

- 3계층 : 전송 계층( Transprot Layer )
OSI 7계층의 전송 계층에 해당하고, 통신 노드 간의 연결을 제어하고, 신뢰성 있는 데이터 전송을 담당한다
프로토콜 - TCP, UDP

- 4계층 : 응용 계층( Application Layer )
OSI 7계층의 세션 계층, 표현 계층, 응용 계층에 해당하고, TCP/UDP 기반의 응용 프로그램을 구현할 떄 사용한다.
프로토콜 - FTP, HTTP, SSH

## TCP( Transmission Control Protocol, 전송 제어 프로토콜 )의 특성 5가지
#### 연결형 서비스
가상 회선 방식을 제공한다.
- 3 Way-Handshake : 연결 초기화
- 4 Way-Handshake : 연결 해제, 세션 종료

#### 흐름 제어
데이터 처리 속도를 조절하여 수신자의 **버퍼 오버플로우**를 방지하며, 수신자의 윈도우 크기 값을 통해 수신량을 정할 수 있다.
- 버퍼 오버플로우 : 사용자가 입력한 데이터의 크기가 너무 과하여 제한된 버퍼 용량에서 넘침을 의미한다.

#### 혼잡 제어
네트워크 내의 패킷 수가 넘치게 증가하지 않도록 방지한다. 정보의 소통량이 과다하면 혼잡 붕괴 현상이 일어날 수 있기 때문이다.

#### 신뢰성 높은 전송
TCP에서 항상 메인 이슈인 retransmission, 재전송에 대한 내용이다.
- Dupack-based retransmission
- Timeout-based retransmission

#### 전이중, 점대점 방식
- 전이중( Full-Duplex )
전송이 양방향에서 동시에 일어날 수 있다. ----><----

- 점대점( Point to Point )
각 연결이 정확이 2개의 종단점을 갖고 있다. O------O

## TCP 3-Way Handshake( 연결 초기화 )
TCP 3-Way Handshake는 TCP/IP 프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터르 전송하기 전에 **정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립
하는 과정을 의미**한다.

Client -> Server : TCP SYN( Synchronize Sequence Numbers )
Server -> Client : TCP SYN, ACK( Acknowledgment )
Client -> Server : TCP ACK

![](https://velog.velcdn.com/images/dymnam/post/3c30293b-a60b-496f-bb98-816a69a5a51a/image.png)
(1) Client는 Server에 접속 요청 메세지( SYN )를 전송하고 SYN_SENT 상태가 된다.

(2) Server 는 SYN 요청을 받고 Client에 요청을 수락( SYN + ACK ) 하고 SYN_RECEIVED 상태가 된다.

(3) Client는 Server에게 수락 확인( ACK )를 보내고 Server는 ESTABLISHED 상태가 된다.

위 과정은 양쪽 모두 상대편에 대한 Sequence Number를 가지고 데이터를 전송할 준비가 되었으며, 신뢰성있는 연결을 보장하는 과정이다.

## TCP 4-Way Handshake( 연결 해제, 세션 종료 )
TCP 4-Way Handshake는 3-Way로 접속을 진행한 뒤, 접속 상태를 **해제**하기 위해서 필요한 과정이다.

![](https://velog.velcdn.com/images/dymnam/post/30a1d0b2-b20f-43b4-b852-1121f4834188/image.png)
![](https://velog.velcdn.com/images/dymnam/post/75ecb69b-0d35-4f03-b5ec-d776c11b159a/image.png)
위 그림 2개를 보면서 설명을 해보자면 다음과 같다.

(1) 연결이 되어 있는 상태이다.

(2) 연결을 종료하고자 하는 Client는 Server에게 TCP Header의 flags 필드의 FIN을 1로 세팅하여 전송하고 소켓을 FIN_WAIT_1 상태로 변경한다.

(3) FIN을 받은 Server는 소켓을 CLOSE_WAIT 상태로 변경되며 FIN에 대응되는 ACK를 전송해준다.

(4) Server는 연결 종료를 위해 FIN 패킷을 Client에게 전송하며 소켓을 LAST_ACK 상태로 변경한다.

(5) FIN을 받은 Client는 TIME_WAIT 상태로 변경되며 FIN에 대응되는 ACK를 Server에 전송한다.
ACK를 받은 Server는 소켓을 CLOSED 상태로 변경한다.

(6) 시간이 경과한 뒤, Client도 소켓을 CLOSED 상태로 변경한다.( MSL은 커널마다 지정된 시간 확인 필요 )
