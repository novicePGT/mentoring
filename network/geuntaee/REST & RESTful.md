## REST( Representational State Transfer )
REST( Representational State Transfer ) : 자원을 이름으로 구분하여 해당 자원의 상태를 주고받는 모든 것.

- HTTP URI( Uniform Resource Identifier )를 통해 자원을 명시.
- HTTP Method( POST, GET, PUT, DELETE)를 사용.
- 자원에 대한 CRUD가 가능함.

## REST 구성요소
#### 자원( Resource ) : HTTP URI
#### 자원에 대한 행위 : HTTP Method
#### 자원에 대한 행위의 내용( Representation ) : HTTP Message Pay Load

## REST 특징
- Server - Client( 서버 - 클라이언트 구조 )
- Stateless( 무상태 )
- Cacheable( 캐시 처리 가능 )
- Layered System( 계층화 )
- Uniform Interface( 인터페이스 일관성 )

## REST 장점 !
- HTTP 표준 프로토콜을 따르는 모든 플랫폼에서 사용 가능.
- 여러가지 서비스 디자인에서 생길 수 있는 문제를 최소화.
- 서버와 클라이언트의 역활을 명확하게 분리.

## REST 단점 !
- 표준이 존재하지 않아 정의가 필요.
- 사용할 수 있는 메소드가 4가지 뿐.
- HTTP Method 형태가 제한적.

## RESTful
RESTFUL이란 REST의 원리를 따르는 시스템을 의미한다.
But, REST를 사용했다 하여 모두가 RESTful한 것은 아님.
REST API의 설계 규칙을 올바르게 지킨 시스템을 RESTful하다 말할 수 있으며, 모든 CRUD 기능을 POST 처리 하는 API, 또는 URI 규칙을 올바르게 지키지 않은 API는 REST API를 사용하였지만 RESTful하지 못한 시스템이라고 말한다.
**RESTful의 목적은 이해하기 쉽고 사용하기 쉬운 REST API를 만드는 것**

## REST API 특징
- REST 기반으로 시스템을 분산해 확장성과 재사용성을 높여 유지보수 및 운영에 용이.
- HTTP 표준 프로토콜을 따르는 모든 플랫폼에서 사용 가능.

## REST API 설계 기본 규칙
#### URI는 정보의 자원을 표현해야한다.
- resource는 동사보다는 명사를 사용한다.
- resource는 영어 소문자 복수형을 사용하여 표현한다.
-> GET /MEMBER/1 ---> GET /members/1

#### 자원에 대한 행위는 HTTP Method( CRUD 등... )로 표현한다.
- URI에 HTTP Method가 들어가면 안된다.
-> GET /members/delete/1 ---> DELETE /members/1

- URI에 행위에 대한 동사 표현이 들어가면 안된다.
-> GET /members/show/1 ---> GET /members/1
-> GET /members/insert/2 ---> POST /members/2

#### REST API 설계규칙
1. 슬래시 구분자( / )는 계층 관계를 나타내는데 사용한다.
> http://restapi.example.com/houses/apartments

2. URI 마지막 문자로 슬래시( / )를 포함하지 않는다.
> REST API는 분명한 URI를 만들어 통신을 해야 하기 때문에 혼동을 주지 않도록 URI 경로의 마지막에는 슬래시(/)를 사용하지 않는다.
http://restapi.example.con/houses/apartments

3. 하이픈( - )은 URI 가독성을 높이는데 사용한다.

4. 밑줄( _ )은 URI에 사용하지 않는다.

5. URI 경로에는 소문자로 사용한다.
> RFC 3986( URI 문법 형식 )은 URI 스키마와 호스트를 제외하고는 대소문자를 구별하도록 규정.

6. 파일확장자는 URI에 포함하지 않는다.
