# 장고를 시작할 때 알아두면 좋은 개념 - DRF

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

### Httpie 명령 예시 (example site: httpbin.org)

조회 데이터는 `==`, 입력/수정/삭제 데이터는 `=` 표시

- `$ http GET(생략시 자동 GET) 요청주소 GET인자==값 GET인자==값`
- `$ http --json POST 요청주소 GET인자==값 GET인자==값 POST인자=값`
  - `application/json -> 요청 데이터 JSON 직렬화`
- `$ http --form POST 요청주소 GET인자==값 GET인자==값 POST인자=값`
  - `multipart/form-data`
- `$ http PUT 요청주소 GET인자==값 GET인자==값 PUT인자=값`
- `$ http DELETE 요청주소 GET인자==값 GET인자==값`

### post 요청 

![스크린샷 2021-01-12 오후 11 22 22](https://user-images.githubusercontent.com/48043799/104326454-1a6a5e80-552d-11eb-8abb-8c63b0492a49.png)



## 직렬화(Serialization)

프로그래밍 언어 또는 소프트웨어 개발에서 통신하거나 저장시 사용하기 편한 형식(문자열)으로 변환하는 것을 말합니다. 역직렬화는 반대로 받은 데이터를 사용(객체)할 수 있도록 변환하는 것이며 포맷으로는 JSON, XML등이 있습니다.

보통 API서버에서는 `JSON`을 사용하며 파이썬에서는 전용 포맷으로 `PICKLE`  을 사용하기도 합니다. `PICKLE`은 파이썬 시스템끼리 통신시만 사용 가능한 점과 버전별 이슈가 있는점이 단점으로 작용합니다.

```python
post_set = [
	{'title': 'hello'},
]

json_string = json.dumps(post_set) #'{"title": "hello"}' #직렬화

json.loads(json_string) #역직렬화

```



### Object of type User is not JSON serializable

장고 타입(Model, QuerySet)에 대한 직렬화 rule을 기본적으로 지원하지 않아서 발생하는 error입니다. 장고의 DjangoJSONEncoder, json.JSONEncoder, JSONRenderer등을 사용하여 직렬화 작업을 수행할 수 있습니다. 마지막으로 QuerySet은 JsonResponse(MyJSONEncoder)를 통해, Model타입은 따로 ModelSerializer를 통해 변환합니다.



## ModelSerializer

역할면에서 post 요청만 처리하는 Form으로부터 데이터가 포함된 JSON 문자열을 생성하며 입력된 데이터에 대한 유효성 검사등을 처리할 수 있습니다.

Model 객체는 many=False(default setting) 지정하지만 QuerySet 객체의 경우 필수로 many=True 옵션을 주어야합니다.

![image](https://user-images.githubusercontent.com/48043799/104604680-f3449600-56c0-11eb-99ec-e59761b6e4a5.png)

```python
serializer = PostSerializer(Post.objects.all(), many=True) #다수일경우 Many = True
get_serializer = PostSerializer(Post.objects.first()) #Model 객체 = default => False 

serializer.data

```



## DRF HttpResponse JSON 응답

Response상에선 JSON직렬화가 Lazy하게 동작하며 실 응답을 생성할 때 rendered_content 속성에 접근하며, 접근한 순간 변환 처리가 됩니다.

DRF Response는 요청 콘텐츠 타입에 맞춰 응답을 주는 역할을 합니다. (drf 사용시 늘 Response 사용)



## Response APIView

DRF의 모든 View는 APIView를 상속받습니다. APIView를 통해 Response에 다양한 속성을 지정할 수 있습니다.



## Serializer의 중첩 사용 

포스트 조회 응답시에 사용자의 이름이 필요할 때 다음과 같이 적용할 수 있습니다. PostSerializer를 응답할떄` AuthorSerializer`를 인스턴스로 사용하여 `author`를 변수로 선언하고 이를 PostSerializer 응답시 Author 자체의 username을 포함해 필요한 모든 필드를 선언 후 사용할 수 있습니다.

### Example

```python
class AuthorSerializer(ModelSerializer):
    class Meta:
        model = get_user_model()
        fields = [
            'username',
            'email',
            'is_superuser',
            'is_staff',
            'is_active',
            'date_joined',
        ]


class PostSerializer(ModelSerializer):
    # author_name = serializers.ReadOnlyField(source='author.username')
    author = AuthorSerializer()

    class Meta:
        model = Post
        fields = [
            'author', # author = AuthorSerializer() 의미
            'message',
            'created_at',
            'updated_at',
        ]

```

![스크린샷 2021-01-15 오전 12 32 23](https://user-images.githubusercontent.com/48043799/104612084-27bc5000-56c9-11eb-9b38-c161738fcecb.png)



## Serializer View 처리

Form의 생성자 첫번째 인자는 data 자체이지만, Serializer 생성자의 첫번째 인자는 객체의 instance입니다.

`PostSerializer(instance=Post.objects.all(), many=True).data -> instance`

또한 SerializerView는 **APIView 클래스** or **@api_view decorators**를 활용하여 View의 속성을 부여하여 사용합니다.



### serializers.py

BaseSerializer -> init 함수 확인 (self, instance=None, data=empty, **kwargs) 등

### generics.py

GenericAPIView(views.APIView) 상속 (query_set, serializer_classe) 등

### decorators.py

method api_view -> decorator함수내에 APIView클래스를 활용해서 사용

### render: 직렬화 클래스

- JSONRenderer: JSON 직렬화
- TemplateHTMLRenderer: HTML 페이지 직렬화

### parser: 비직렬화 클래스

- JSONparser 포맷 처리
- Formparser
- Multipartparser

### authentication: 인증 클래스

- sessionAuthentication: 세션기반 인증
- BasicAuthentication: HTTP basic 인증

### throttle : 요청 제한 클래스

- 빈 듀플(default setting x)

### permission: 권한 클래스

- PermissionsAllowAny: 모두 접근 가능

### content_negotiation: 요청에 따른 직렬화 비직렬화 선택

- DefaultContentNegotiation: 동일 URL 요청이지만 JSON response인지 HTML response 인지 체크

### metadata: 메타정보 처리 클래스

- SimpleMetadata

### versioning: 요청에 api 버전 정보 탐색 클래스

- None: API 버전 정보를 탐지하지 않는 선언
- 요청 URL에 GET 인자에 HEADER 버전 정보를 확인하여 해당 버전에 맞는 APIView를 호출되도록 실행



## APIView(Class Base View)

하나의 CBV로써 하나의 URL만 처리가 가능하고 method(get, post, put, delete)에 맞게 멤버함수를 구현 후 해당 요청이 들어올때마다 함수 호출

- 직렬화/비직렬화 (JSON)
- 인증 체크
- 사용량 제한 체크(호출 범위 지정)
- 권한 클래스 지정 (유저 로그인, 비로그인, 인증, 비인증등 허용 여부 결정)
- 요청된 API 버전 문자열 탐지 후 request.version에 저장
- [참고 URL](https://github.com/encode/django-rest-framework/blob/master/rest_framework/views.py) -> dispatch -> initial

<img width="588" alt="스크린샷 2021-02-22 오후 12 56 09" src="https://user-images.githubusercontent.com/48043799/108656904-71405c00-750d-11eb-9890-fcde797d985a.png">

> API version 버전관리가 사용중인 경우 확인,  들어오는 요청 허용 여부등 체크



## CBV APIView Example Code

```python
# APIView(View): 클래스 내에 return시 crsf_exempt(view)
# Note: session based authentication is explicitly CSRF validated,
# all other authentication is CSRF exempt.

class PostListAPIView(APIView):
    def get(self, request):
        qs = Post.objects.all()
        serializer = PostSerializer(qs, many=True)
        return Response(serializer.data)

    def post(self, request):
        serializer = PostSerializer(data=request.data)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=200)
        return Response(serializer.errors, status=400)
      
```

## FBV APIView Example Code

```python
# 하나의 작업만을 구현할때 @api_view 장식자 이용한 로직 유용
@api_view(['GET', 'POST'])
def post_list(request):
    if request.method == 'GET':
        serializer = PostSerializer(Post.objects.all(), many=True)
        return Response(serializer.data)
    else:
        serializer = PostSerializer(data=request.data, many=True)
        if serializer.is_valid():
            serializer.save()
            return Response(serializer.data, status=200)
        return Response(serializer.errors, status=400)
```



## Django DRF Support mixins

필요에 따라 다양한 믹스인을 생성 가능

- CreateModelMixin
- ListModelMixin
- RetrieveModelMixin
- UpdateModelMixin
- DestroyModelMixin

## 다양한 generics APIView

- generics.CreateAPIView : post -> create
- generics.ListAPIView : get -> list
- generics.RetrieveAPIView : get -> retrieve
- generics.DestroyAPIView : delete -> destroy
- generics.UpdateAPIView : put -> update, patch -> partial_update
- generics.ListCreateAPIView : get -> list, post -> create
- generics.RetrieveUpdateAPIView : get -> retrieve, put -> update, patch -> partial_update
- generics.RetrieveDestroyAPIView : get -> retrieve, delete -> destroy
- generics.RetrieveUpdateDestroyAPIView : get -> retrieve, put -> update, delete -> destroy, patch -> partial_update

> [django generics github code](https://github.com/encode/django-rest-framework/blob/master/rest_framework/generics.py)

## View 구현 Tip

### 중복 줄이기 및 상황에 따른 View 구현

- ViewSet
- APIView(CBV), @api_view(FBV)
- View(Django)



## ViewSet

단일 리소스에서 관련있는 View들을 단일 클래스에서 제공하는 것을 말합니다.

list/create/detail/update/delete/partial_update 등을 멤버 함수로 구현할 수 있습니다.

```python
Class PostListAPIVIew(generics.ListCreateAPIView):
		queryset = Post.objects.all()
		serializer_class = PostSerializer
		
Class PostDetailAPIVIew(generics.RetrieveUpdateDestroyAPIView): # Retrieve -> get(detail)
		queryset = Post.objects.all()
		serializer_class = PostSerializer
    
# ViewSet을 사용해 하나로 합쳐 사용 가능
```

### ModelViewSet

- viewsets.ReadOnlyModelViewSet
  - list -> 1개 url
  - detail -> 1개 url
- viewsets.ModelViewSet
  - list/create -> 1개 url
  - detail/update/partial_update/delete -> 1개 url

### URL_PATTERN 매핑

- 개별 View.as_view 연결
- Router를 통해 일괄 등록
  - `router = DefaultRouter()`
  - `router.register('post', views.PostViewSet)`
  - `path('', include(router.urls))`





## Reference

- https://educast.com/course/web/ZU53/
- https://wikidocs.net/book/837