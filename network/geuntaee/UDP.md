이전 TCP/IP에 이어서 연결 되는 내용인 UDP에 대해서 알아볼 것이다.
UDP는 User Datagram Protocol이다.
TCP와 UDP 프로토콜은 모두 해킷을 한 컴퓨터에서 다른 컴퓨터로 전달해주는 IP 프로토콜을 기반으로 구현되어 있지만, 서로 다른 특징을 가지고 있다.

## UDP
UDP는 굉장히 빠른 속도로 데이터를 전달할 수 있는 특징을 가지고 있다.
그 이유에 대해 설명하면 아래 그림을 참조해보자
![](https://velog.velcdn.com/images/dymnam/post/7f1b74e6-6fe9-45c9-ac60-5ee6e0c74889/image.png)
위 그림은 TCP의 데이터 송신 과정이다.
![](https://velog.velcdn.com/images/dymnam/post/4d315b5b-2bb9-4bd0-821b-91bfd4e8509b/image.png)
위 그림은 UDP의 데이터 송신 과정이다.
그림을 비교해서 확인해보면 TCP는 양방향으로 통신을 하는데, UDP는 일방적으로 통신하는 과정을 볼 수 있다.
즉, 신뢰성이 요구되는( 필요한 ) 애플리케이션에서는 TCP를 사용하고, 간단한 데이터를 빠르게 전송하고자 하는 애플리케이션에서는 UDP를 사용한다.
UDP는 신뢰성보다 실시간성과 연속성, 속도 등을 더 중요시하고 있다는 뜻이다.

## UDP의 단점
TCP와 비교하면 신뢰성 있는 데이터의 전송을 보장할 수 없다는 것이다.
독립적인 패킷( Datagram )을 단위로 하여 통신하는 규약으로 TCPdㅘ 달리 연결을 위해 할당되는 논리적 경로가 없고, 각각의 패킷이 다른 경로로 전송, 처리되는 **비연결형 프로토콜**이다.
UDP는 <span style=”color:lightblue”>정보의 송수신에 대한 신호절차가 없고, 최소한의 오류만을 검증하여 패킷의 손실이 상대적으로 많다.</span>

## TCP VS UDP
![](https://velog.velcdn.com/images/dymnam/post/573696e2-157e-4c45-b241-634cd4c29712/image.png)
