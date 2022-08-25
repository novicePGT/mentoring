![](https://velog.velcdn.com/images/dymnam/post/1d24ab5c-70d0-4ed9-a76e-b83148deb8b5/image.png)

## RabbitMq
RabbitMq는 AMQP를 따르는 오픈소스 메세지 브로커로 메세지를 많은 사용자에게 전달하거나, 요청에 대한 처리 시간이 길 때, 해당 요청을 다른 API에게 위임하고 빠른 응답을 할 때 많이 사용한다.

#### **RabbitMq의 흐름도**
![](https://velog.velcdn.com/images/dymnam/post/f519b599-8f96-4d59-91cb-4a75daa9c9e4/image.png)
- 메세지는 Message Queue를 통해 원하는 사용자에게 전달할 수 있다.

![](https://velog.velcdn.com/images/dymnam/post/2f33273b-6a8a-4127-9e25-15f9974edd2a/image.png)
- Message Broker는 Producer와 Consumer 사이 중간 역할을 담당한다.
- 가장 기초 구조이며 Message Queue에 저장되어 소비자가 조회할때까지 저장하게 된다.


## RabbitMq를 사용해야할 때

- Message Queue는 빠른 응답을 원할 때 주로 사용한다.
->  많은 Resource가 필요한 작업은 Event를 발생시켜 다른 API에게 위임한다. 때문에 Request에 대해 빠르게 응답할 수 있다.
- Message를 많은 사람들에게 전달하고 싶을 때 사용한다.
- 두 Application 간의 결합도는 Message Queue를 통해 낮출 수 있는 장점이 있다.

## Exchanges
#### Exchange 4Type
![](https://velog.velcdn.com/images/dymnam/post/aeae7781-ebea-43b4-90eb-a0f49c45f0ed/image.png)

#### Exchange 기타 설정 값
Durability
- 브로커가 재시작 될 때 남아 있는지 여부
- durable : 재시작해도 유지 가능
- transient : 재시작하면 사라짐

Auto-delete
- 마지막 Queue 연결이 해제되면 삭제

## Message Queue 흐름도
![](https://velog.velcdn.com/images/dymnam/post/e9a5b1cd-d179-4978-a7f7-aa9d5bc31b70/image.png)

1. Producer는 Message를 Exchange에 보내고, Exchange를 생성하면서 Exchange의 Type을 정한다.
2. Exchange는 Routing Key를 사용하여 적절한 Queue로 Routing한다.
3. Exchange - Queue 와 Binding이 완료되며, Message 속성에 따라 적절한 Queue로 Routing 된다.
4. Message는 소비자가 소비할때까지 Queue에 대기한다.
5. 소비자는 Message를 소비한다.

## RabbitMq 용어
#### Vhost( Virtual Host )
- Virtual Host를 통해서 하나의 RabbitMq 인스턴스 안에 사용하고 있는 Application을 분리할 수 있다.

#### Connection
- 물리적인 TCP Connection, HTTPs -> SSL Connection을 사용

#### Channel
- 하나의 물리적인 Connection 내에 생성되는 가상의 Connection
- 소비자의 Process나 Thread는 각자 Channel을 통해 Queue에 연결 될 수 있다.
