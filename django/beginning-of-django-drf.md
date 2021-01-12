# 장고를 시작할 때 알아두면 좋은 개념 - 인증

## API 서버

웹/앱 서비스를 개발하기 위해 사용하며 데이터 위주의 기능을 제공하는 특징을 가지고 있습니다. 대표적으로 Google api(구글로그인), Naver api(지도), Kakao api(공유하기, 카카오로그인)등이 존재합니다. 유저별로 사용하는 서비스의 버전이 다양하기에 API에는 버전 개념을 두어 관리하며 웹의 경우는 늘 최신버전으로 유지합니다.



## REST API

### RESTful 이란?

REST 아키텍처 제약조건을 충족하는 애플리케이션 프로그래밍 인터페이스를 말합니다.

Representational State Transfer의 줄임말로 모든 자원을 리소스로 표현하는 특징을 가지고 있고 HTTP URI + HTTP method + message로 구성되어 있습니다.

### RESTful 디자인 원칙

- 리소스 중심 디자인
- 클라이언트에 접근할 수 있는 모든 종류의 서비스 및 개체가 리소스에 포함
- 요청/응답 포맷으로 JSON 사용
- 균일한 인터페이스로 HTTP Method (GET, POST, PUT, DELETE) 사용
- 서버에 있는 resource는 각각 접근 가능한 고유 URI 존재(해당 리소스를 고유하게 식별하는 식별자)
  - `https://example.com/exam/2/`
- 모든 요청들은 요청할때마다 정보를 보내주기 때문에 Session을 보관할 필요가 없습니다. 이러한 이유 때문에 서비스에 자유도가 높아지고 유연함 또한 증가합니다.

### 리소스 중심 API 구성(동사 명시 x -> 리소스 중심 명시)

- `/customers/ ` -> 고객 리스트
- `/customers/4/` -> pk가 4인 고객 
- `/customers/4/orders/` -> pk가 4인 고객의 주문 리스트
- `/order/392/customer/` -> order pk가 392인 고객

### HTTP Method

- GET: 리소스의 표현, 본문의 리소스 세부정보 조회
  - 상태코드: 200, 리소스 못찾은 경우 404
- POST: 새 리소스 생성, create
  - 상태코드: 201, 새 리소스 URI는 응답 laction 헤더에 지정, 리소스를 생성하지 않았을때 204, 변조된 잘못된 데이터 요청시 400
- PUT: 기존 리소스를 대체하여 갱신할 리소스 정보 제공, update (매번 같은 요청을 반복해도 서버상 같은 결과 보장해야함)
  - 상태코드: 200, 리소스를 상황에 따라 업데이트 불가한 경우 409 return
- DELETE: 기존 리소스를 제거. delete
  - 상태코드: 200, 응답 본문에 추가정보 포함되지 않은 의미로 204, 리소스 없을시 404 return
- PATCH: 기존 리소스를 부분 대체 

### 요청, 응답 형식

##### request: Content-Type 헤더

요청시 처리를 원하는 형식을 지정하면 API서버는 해당 요청 형식으로 응답합니다.

서버에서 지원하지 않는 경우는 http status-code 415를 return 



## Django-Rest-Framework

장고가 아닌 별도의 써드파티 앱으로 DRF는 RestApi 컨셉을 쉽게 구현할 수 있도록 도와줍니다. 또한 Serializer/ModelSerializer를 통한 데이터 유효성 검증, 데이터 직렬화 parser를 통한 처리등을 지원합니다.

APIView/Generic/ViewSet/ModelViewSets을 통한 요청 처리를 하며 Renderer를 통한 다양한 응답포맷도 지원하고 인증, 권한등의 써드파티를 이용해 JWT인증 처리도 구현할 수 있습니다.

## URL 설계

`/post/` => 하나의 view에서 2가지 분기처리

- GET : 목록 리스트 응답
- POST : 새글 생성 후 확인 응답

`/post/3/` -> 하나의 view에서 3가지 분기처리

- GET : 3번 글 정보 응답
- POST : 3번 글 수정, 저장 후 확인 응답
- DELETE : 3번 글 삭제 후 확인 응답

DRF는 정형화된 중복 코드를 줄일 수 있도록 도와주는 ClassBased View를 비롯해 다양한 기능을 지원합니다.



### Http 클라이언트 프로그램 (httpie, postman)

http 설치: `pip install httpie`

### Httpie 명령 예시

조회 데이터는 `==`, 입력/수정/삭제 데이터는 `=` 표시

- `$ http GET(생략시 자동 GET) 요청주소 GET인자==값 GET인자==값`
- `$ http --json POST 요청주소 GET인자==값 GET인자==값 POST인자=값`
- `$ http --form POST 요청주소 GET인자==값 GET인자==값 POST인자=값`
- `$ http PUT 요청주소 GET인자==값 GET인자==값 PUT인자=값`
- `$ http DELETE 요청주소 GET인자==값 GET인자==값`

### post 요청 후 

![스크린샷 2021-01-12 오후 11 22 22](https://user-images.githubusercontent.com/48043799/104326454-1a6a5e80-552d-11eb-8abb-8c63b0492a49.png)



## Reference

- https://educast.com/course/web/ZU53/
- https://wikidocs.net/book/837