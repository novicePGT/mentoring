## Monolithic Architecture
모놀리식 구조라고 불리는 Monolithic Architecture는 **하나의 구조로 단단히 이루어진** 이란 뜻을 갖고 있다.
![](https://velog.velcdn.com/images/dymnam/post/aae036eb-0e48-4115-ba33-b0b154138189/image.png)
위의 그림처럼 Business Logic과 Data Access Layer와 UI가 묶여 제공되는 구조이다.

#### 장점
- 작은 규모의 프로젝트 개발에 효율적이다.
- 배포가 간편, 테스트에 수월하다.( 하나의 패키지로 묶여있음 )

#### 단점
- 대형 시스템 개발 시 확장성 문제
- 하나의 서비스가 다른 서비스에 문제를 일으킬 수 있다.

## Microservice Architecture( MSA )
MSA는 모놀리식과 반대되는 개념으로 작은 단위의 서비스로 나누어진 구조를 말한다.
![](https://velog.velcdn.com/images/dymnam/post/5128f683-d5d9-400a-a05f-83237fc566e8/image.png)
위 그림처럼 하나의 서비스를 기능별로 묶어서 여러 개의 작은 어플리케이션으로 나누어 서비스를 담당하는 설계 구조이다.

#### 장점
- 뛰어난 재사용성( 분산 환경, 기능 확장성, 생산성, 효율성 )
- 개별 서비스 단위의 배포 가능
- 요구 사항의 잦은 변경에 대처가 용이( 동적 대응 )
- 새로운 기술 적용에 용이

#### 단점
- 로그 추적 등 문제 해결이 쉽지 않다.
- 유지보수 대상 항목이 늘어난다.
- API 호출을 통한 통신 구조 상 Network에 의한 성능 이슈 발생
- 트랜잭션 처리 성능 향상에 대한 비용
- 테스트 과정이 복잡하다.

#### MSA의 핵심 방향성
- Application을 기능별로 나눈다.
- 클라우드 환경에서 운영한다.
- 확장( Scale Out )이 용이한 구조로 설계한다.
