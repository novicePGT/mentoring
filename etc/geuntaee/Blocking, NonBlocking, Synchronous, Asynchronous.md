## ETC에 대해 가장 첫 장
ETC는 라틴어 et cetera의 준말로, and other thing의 의미로 ex가 아니라 et인게 포인트이다.
-> **한국어로 '기타 등등'**의 의미가 된다.

## Blocking, NonBlocking, Synchronous, Asynchronous
![](https://velog.velcdn.com/images/dymnam/post/a15b08ef-de1e-42d0-8137-27eed99fdd5f/image.png)

#### Block
- 어떤 일을 요청하고, 작업이 끝날때까지 다른 일을 할 수 없다.

#### NonBlock
- 어떤 일을 요청하고, 작업이 끝날때까지 다른 일을 할 수 있다.

#### Synchronous( 동기 )
- 요청과 결과가 동시에 일어난다.
- 만약 요청이 3개일 경우, 순서가 지켜지면 Synchonous 이다.

#### Asynchronus( 비동기 )
- 요청과 결과가 동시에 일어나지 않는다.
- 만약 요청이 3개일 경우, 순서가 지켜지지 않는다면 Asynchonous 이다.

## Sync( 동기 ) & Blocking
![](https://velog.velcdn.com/images/dymnam/post/0605def0-25b5-4bf8-83b1-02533a50c075/image.png)
- 요청한 일이 끝나면 응답을 보내준다.

## Sync( 동기 ) & NonBlocking
![](https://velog.velcdn.com/images/dymnam/post/8ef98107-a5f0-441b-a57c-939d2f8cc5a6/image.png)
- 요청 후 응답을 바로 보내지 않고, 다시 요청을 보내야 응답을 알려준다.

## Async( 비동기 ) & Blocking
![](https://velog.velcdn.com/images/dymnam/post/d62200c5-3d6f-4c8b-bb00-dc59c4748c9a/image.png)
- 비동기로 응답이 일정한 순서로 오지 않는데, 첫번째 응답을 보내고 Block 할 수 없기에 무의미하다.

## Async( 비동기 ) & NonBlocking
![](https://velog.velcdn.com/images/dymnam/post/9f93c08c-a141-499a-85c6-becfcf5c0d5a/image.png)
- 요청이 완료되었는지 물어보기 위해 Block 되는 시간이 없어 효율적이다.
