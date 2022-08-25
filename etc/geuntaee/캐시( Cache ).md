## Cache
캐시란 자주 사용하는 데이터나 값을 미리 복사해 놓은 임시 장소를 가리킨다.
아래 그림과 같이 캐시는 저장공간이 작고 비용이 비싼 대신 빠른 성능을 제공한다.
![](https://velog.velcdn.com/images/dymnam/post/0f44e7bc-116f-4a45-965b-4af0a6c9dfbe/image.png)

## Cache 사용 경우
- 접근 시간에 비해 원래 데이터를 접근하는 시간이 오래 걸리는 경우( 서버의 균일한 API 데이터 )
- 반복적으로 동일한 결과를 돌려주는 경우( 이미지나 썸네일 )

캐시에 데이터를 미리 복사해 놓으면 계산이나 접근 시간 없이 더 빠른 속도로 데이터에 접근할 수 있다.
반복적으로 데이터를 불러오는 경우에 데이터베이스 또는 서버에 요청하는 것이 아니라 Memory에 데이터를 저장하였다가 불러다 쓰는 것을 의미한다.

## Cache의 필요성
![](https://velog.velcdn.com/images/dymnam/post/8a43e791-109d-45cd-92ae-e043003296bf/image.png)
위 그래프는 Long Tail 그래프이다.
Long Tail 법칙은 20%의 요구가 시스템 리소스의 대부분을 사용한다는 법칙이다.
그렇기 때문에 20%의 기능에 Cache를 이용함으로 리소스 사용량은 줄이고, 성능은 향상 시킬 수 있다.

## Local Cache VS Global Cache
#### Local Cache
- Local 장비 내에서만 사용되는 캐시로, Local 장비의 Resource를 이용한다.
- Local 에서만 작동해서 속도가 빠르다.
- Local 에서만 작동해서 다른 서버와 데이터 공유가 어렵다

#### Global Cache
- 여러 서버에서 Cache Server에 접근하여 사용하는 캐시로 분산된 서버에서 데이터를 저장하고 조회할 수 있다.
- 네트워크를 통해 데이터를 가져와 Local Cache에 비해 상대적으로 느리다.
- 별도의 Cache 서버를 사용해 서버 간 데이터의 공유가 쉽다.
