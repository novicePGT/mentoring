## MQTT
MQTT( Message Queueing Telemetry Transport )는 메세지 발행-구독하는 방법으로 통신하는 메세지 기반의 송수신 프로토콜이다.
MQTT는 TCP/IP 프로토콜 위에서 동작하며, Wifi나 인터넷에 연결하여 통신한다.
MQTT 서버는 Facebook Messenger에서 채택하면서 유명해졌으며, 그 외 많은 기업들이 중계 시스템 개선을 위해 사용하고 있다.

#### 핵심
최소한의 전력과 패킷량으로 통신하는 프로토콜로 제한된 통신 환경을 고려하여 디자인 되어서 IoT와 모바일 어플리케이션 등에 적합하다.

## 기본 구조
![](https://velog.velcdn.com/images/dymnam/post/9900bd7f-ba0f-40ac-8727-bc7f26e6cfa6/image.png)
#### Publisher
Topic 발행( 1 Topic - N Subscriber 가능 )

#### Broker
중계 역할, Topic과 구독, 발행을 총괄한다.
Mosquitto 등 다양한 프로그램이 있다.

#### Subscriber
구독자

#### Topic
Publisher에서 발행하는 데이터 Subscriber가 받고자 하는 데이터

![](https://velog.velcdn.com/images/dymnam/post/6394485d-aebf-49ff-a036-e06655cc75f7/image.png)
- '/'를 이용해 계층적으로 구성할 수 있는 특징을 가지고 있어 대량의 센서들을 효율적으로 관리할 수 있다.
> ex) DepartmentStore/ AStore / Shirts

- '+'를 이용해 단 한 개의 토픽을 임의의 토픽으로 대체할 수 있다. 아래는 백화점 내 모든 셔츠를 체크한다.
> ex) DepartmentStore/ + / Shirts

- '#'을 이용하여 2단계 이상의 하위 토픽을 와일드카드 기능으로 대체할 수 있다. 이 기능은 맨 마지막에만 사용 가능하며 백화점 내의 모든 가게의 모든 물건을 체크할 수 있다는 의미이다
> ex) DepartmentStore/ #

## QoS
MQTT는 3단계의 QoS(Quality of service)를 제공한다.
- 0 : 메시지는 한번만 전달하며, 전달여부를 확인하지 않는다. Fire and Forget 타입이다.
- 1 : 메시지는 반드시 한번 이상 전달된다. 하지만 메시지의 핸드셰이킹 과정을 엄밀하게 추적하지 않기 때문에, 중복전송될 수도 있다.
- 2 : 메시지는 한번만 전달된다. 메시지의 핸드셰이킹 과정을 추적한다. 높은 품질을 보장하지만 성능의 희생이 따른다.
서비스의 종류에 따라서 적당한 QoS 레벨을 선택해야 한다.
