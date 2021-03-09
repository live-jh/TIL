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
- `$ http POST 요청주소 POST인자=값`
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
json.dump("한글", ensure_ascii=False).encode('utf8') #utf-8 인코딩 -> 직접 변환

json.loads(json_string) #역직렬화


#TypeError: Object of type "obj_name" is not JSON serializable (Model or QuerySet에 대해 직렬화 Rule X)
from django.contrib.auth import get_user_model
User = get_user_model()
json.dumps(User.objects.first()) # error

```

### Object of type User is not JSON serializable (Type Error)

장고 타입(Model, QuerySet)에 대한 직렬화 rule을 기본적으로 지원하지 않아서 발생하는 error입니다. 장고의 DjangoJSONEncoder, json.JSONEncoder, JSONRenderer등을 사용하여 직렬화 작업을 수행할 수 있습니다. 마지막으로 QuerySet은 JsonResponse(MyJSONEncoder)를 통해, Model타입은 따로 ModelSerializer를 통해 변환합니다.



## Djanngo 기본에서 view단의 직렬화

### 직접 변환 Rule 지정

```python
from django.core.serializers.json import DjangoJSONEncoder
from django.db.models.query import QuerySet

class MyJSONENcoder(DjangoJSONEncoder):
		def default(self, obj):
				if isinstance(obj, QuerySet): #obj가 QuerySet 타입일때
						return turple(obj)
				elif isinstance(obj, Post): #obj가 Post 일때
						return {'id': obj.id, "title": obj.title}
				elif hasattr(obj, 'as_dict'): #obj에 as_dict 포함할때
						return obj.as_dict()
				return super().default(obj)
				
data = Post.objects.all()
json.dump(data, cls=MyJSONEncoder, ensure_ascii=False) -> 직접 변환 Rule 지정
```



## JSONRender

### rest_framework/utils/encoders.py의 JSONEncoder를 이용한 직렬화

- json.JSONEncoder 상속을 통해 구현
- datetime.datetime/date/time/timedelta, decimal.Decimal, uuid.UUID, six_binary_type
- '____getItem____' 속성을 지원할 경우 dict(obj) 반환
- '____iter____' 속성을 지원할 경우 tuple 반환
- QuerySet 타입일 경우 tuple 반환
- Model 타입은 미지원 -> ModelSerializer를 사용해 변환



## JsonResponse에서 QuerySet JSON 직렬화

```python
qs = Post.objects.all()
encoder = MyJSONEncoder
safe = False
json_dumps_params = {'ensure_ascii': False} # True: data가 dict인 경우, False: dict이 아닌 경우
kwargs = {}

from Django.http import JsonResponse

response = JsonResponse(qs, encoder, safe, json_dumps_params, **kwargs)

```



## Django_RestFramework에서 직렬화

### DRF HttpResponse JSON 응답

Response상에선 JSON직렬화가 Lazy하게 동작하며 실 응답을 생성할 때 rendered_content 속성에 접근하며, 접근한 순간 변환 처리가 됩니다.

DRF Response는 요청 콘텐츠 타입에 맞춰 응답을 주는 역할을 합니다. **(drf 사용시 늘 Response 사용)**



### Response APIView

DRF의 모든 View는 APIView를 상속받습니다. APIView를 통해 Response에 다양한 속성을 지정할 수 있습니다.

### rest_framework Code Example

```python
from rest_framwork import generics

class PostListAPIView(generics.ListAPIView):
		queryset = Post.objects.all()
		serializer_class = PostModelSerializer
		
post_list = PostListAPIView.as_view()
```





### ModelSerializer

역할면에서 post 요청만 처리하는 Form으로부터 데이터가 포함된 JSON 문자열을 생성하며 입력된 데이터에 대한 유효성 검사등을 처리할 수 있습니다.

Model 객체는 many=False(default setting) 지정하지만 **QuerySet 객체의 경우 필수로 many=True** 옵션을 주어야합니다.

실제 비지니스(서비스)쪽을 담당하는 역할이기도 합니다. (데이터 생성, 수정, 조회등)

![image](https://user-images.githubusercontent.com/48043799/104604680-f3449600-56c0-11eb-99ec-e59761b6e4a5.png)

```python
serializer = PostSerializer(Post.objects.all(), many=True) #다수일경우 Many = True
get_serializer = PostSerializer(Post.objects.first()) #Model 객체 many = default => False 

```



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

**Form의 생성자 첫번째 인자**는 **data 자체**이지만, **Serializer 생성자의 첫번째 인자**는 객체의 **instance**입니다.

`PostSerializer(instance=Post.objects.all(), many=True).data -> instance`

또한 SerializerView는 **APIView 클래스** or **@api_view decorators**를 활용하여 View의 속성을 부여하여 사용합니다.



### serializers.py

BaseSerializer -> init 함수 확인 (self, instance=None, data=empty, **kwargs) 등

### generics.py

GenericAPIView(views.APIView) 상속 (query_set, serializer_classe) 등을 주로 사용

### decorators.py

method api_view -> decorator함수내에 APIView클래스를 활용해서 사용

### render: 직렬화 클래스

- JSONRenderer: JSON 직렬화
- TemplateHTMLRenderer: HTML 페이지 직렬화

### parser: 비직렬화 클래스

- JSONparser 포맷 처리 (request.body -> parsing)
- Formparser
- Multipartparser

### authentication: 인증 클래스 (유저 식별)

- SessionAuthentication: 세션기반 인증
- BasicAuthentication: HTTP basic 인증

### throttle : 요청 제한 클래스

- 빈 듀플(default setting x)

### permission: 권한 클래스 (유저를 식별하고 난 이후에 접근 레벨 정의)

- PermissionsAllowAny: 모두 접근 가능

### content_negotiation: 요청에 따른 직렬화 비직렬화 선택

- DefaultContentNegotiation: 동일 URL 요청이지만 JSON response인지 HTML response 인지 체크

### metadata: 메타정보 처리 클래스

- SimpleMetadata

### versioning: 요청에 api 버전 정보 탐색 클래스

- None: API 버전 정보를 탐지하지 않는 선언
- 요청 URL에 GET 인자에 HEADER 버전 정보를 확인하여 해당 버전에 맞는 APIView를 호출되도록 실행



## 

## DRF의 기본 뷰 - APIView(Class Base View)

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



## 다양한 generics APIView

개별 (list, post & detail ,put, delete) 로 5개로 구현되는걸 패턴화 시켜놓은 것을 **django rest framework의 generics**

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

## 

## DRF의 기본 뷰 - @api_view

### FBV APIView Example Code

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

클래스기반 View의 확장성을 극대화시켜주는 기능 (다중 상속)

APIView =>  mixins(로직들의 재사용성을 증가) => Generics(패턴화하여 더 구조화) >= ViewSet(더욱 정리한 구조, 추상화)

필요에 따라 다양한 믹스인을 생성 가능

- CreateModelMixin
- ListModelMixin
- RetrieveModelMixin
- UpdateModelMixin
- DestroyModelMixin



## View 구현 Tip

### 중복 줄이기 및 상황에 따른 View 구현

- ViewSet
- APIView(CBV), @api_view(FBV)
- View(Django) 



## ViewSet 

단일 리소스에서 **관련있는 View들을 단일 클래스에서 제공**하는 것을 말합니다. (http 요청을 다수로 엮어 처리하는 의미 -> generics를 조금 더 구조화 시켜 놓은 개념)

list/create/detail/update/delete/partial_update 등을 멤버 함수로 구현할 수 있습니다.

```python
from rest_framework import generics

class PostListAPIVIew(generics.ListCreateAPIView):
		queryset = Post.objects.all()
		serializer_class = PostSerializer
		
class PostDetailAPIVIew(generics.RetrieveUpdateDestroyAPIView): # Retrieve -> get(detail)
		queryset = Post.objects.all()
		serializer_class = PostSerializer
    
# 위 2개의 View를 ViewSet을 사용해 하나로 합쳐 사용 가능
class PostViewSet(ModelViewSet):
    queryset = Post.objects.all()
    serializer_class = PostSerializer

    def list(self, request, *args, **kwargs):
        pass

    def create(self, request, *args, **kwargs):
        pass

    def update(self, request, *args, **kwargs):
        pass

    def retrieve(self, request, *args, **kwargs):
        pass

    def partial_update(self, request, *args, **kwargs):
        pass

    def dispatch(self, request, *args, **kwargs):
        print(request.body)  # logger
        print(request.POST)  # logger
        return super().dispatch(request, *args, **kwargs)

```

### ModelViewSet

2가지의 ViewSet이 존재 **ReadOnlyModelViewSet & ModelViewSet**

- **viewsets.ReadOnlyModelViewSet**
  - **GET** 요청에만 응답하는 ViewSet
  - list -> 1개 url
  - detail -> 1개 url
- **viewsets.ModelViewSet**
  - **GET, POST** 둘 다 응답하는 ViewSet
  - list/create -> 1개 url
  - detail/update/partial_update/delete -> 1개 url

### URL_PATTERN Router 매핑

- 개별 View.as_view 연결
- Router를 통해 일괄 등록
  - `router = DefaultRouter()`
  - `router.register('post', views.PostViewSet)`
  - `path('', include(router.urls))`



## Renderer

같은 EndPoint에 요청받은 타입에 알맞는 응답 포맷을 지원, Content-Type, URL 을 통해 Renderer 지정이 가능합니다.

### default 지원 Renderer

**JSONRenderer** : json.dumps를 사용해 JSON 직렬화

- applications/json, format -> **json**

**BrowsableAPIRenderer** : self-document HTML 렌더링

- text/html, format -> **api**

**TemplateHTMLRenderer** : 지정 템플릿을 이용한 렌더링

### TemplateHTMLRenderer example code

```python
class PostDetail(RetrieveAPIView):
		queryset = Post.objects.all() 
		renderer_classes = [TemplateHTMLRenderer] # Renderer를 사용해 아래 template으로 출력
		template_name = 'blog/post_detail.html'
		
		def get(self, request, *args, **kwargs):
	      post = self.get_object() #get_object는 RetrieveAPIView 내에서 구현이 되어 있음
				return Response({
						'post': PostSerializer(post).data
				})
```

### 다양한 Renderer

- StaticHTMLRenderer : 미리 렌더링된 HTML를 반환, Response로 응답 보낼시 HTML 문자열 전송
- AdminRenderer, HTMLFormRenderer, MultiPartRenderer 등등



## 응답 헤더 포맷

### Accept 헤더

- Accept: text/html
- Accept: applications/json

### a=Get인자 => format

- ?format=json
- ?formant=api



## Renderer 클래스 리스트 지정

### 전역

settings -> REST_FRAMEWORK -> DEFAULT_RENDERER_CLASSES 문자열로 지정(리스트)

### APIView마다 지정

queryset, serializer_class, renderer_class 

### @api_vew마다 지정

URL Captured Value는 keyword args를 통해 전달됩니다. format 인자를 받는 설정했다면 format 인자 추가 작업 `(formant=)`



## Form과 Serializer의 비교

- Serializer&ModelSerializer 는 Form&ModelForm과 유사
- 데이터 변환 및 직렬화 지원 (QuerySet&Model 객체)
- Serializer는 View 응답을 처리하는데 범용적이고 강력한 장점
- ModelSerializer는 Serializer를 생성하기 위한 Shortcut 개념



## Form과 Serializer의 특징

### Form & ModelForm

- **HTML 입력 폼**을 이용한 유효성 검사
  - HTML Form Submit을 통해 비동기 호출 (Android/iOS에 의한 http 요청도 포함)
- Create or Update 에 대한 처리 활용 -> django admin 
- CreateView/UpdateView CBV를 활용한 View 처리 (단일 수행 View)

### Serializer&ModelSerializer

- **데이터 변환 및 직렬화 (JSON 포맷)**
  - 다양한 Client에 대한 Data 위주 http 요청 (Web, iOS, RNApp, Android)
- Web API 포맷에 대한 유효성 검사
- List/Create 또는 특정 데이터에 대한 Retrieve/Edit/Delete 등에 활용
- APIView를 통한 단일 View 처리
- ViewSet을 통한 2개 View, 2개 URL 처리



## Serializer 의 생성자

```python
class BaseSerializer(Field):
		def __init__(self, instance=None, data=empty, **kwargs): # 첫번째 인자 instance (Model 객체 혹은 QuerySet), 유효성검사는 data
```



### data= 인수가 전달된 경우

- .is_valid() - 사용할 수 있습니다.
- .initial_data - 사용 가능
- .validated_data - 'is_valid()' 호출 후(유효성 검증 후)에만 사용할 수 있고 .save()할 때 사용합니다.
- .error - 'is_valid()'를 호출한 후(유효성 검증 후)에만 사용할 수 있습니다.
- .data - is_valid()를 호출한 후(유효성 검증 후)에만 사용할 수 있습니다.

### serializer.save(**kwargs) 호출 할 때

- DB에 저장한 관련 **instance**를 리턴
- .validated_data와 kwargs dict을 합친 데이터 다음 요청과 같이 수행한다.
  - .update & .create 함수를 통해 필드에 값을 할당 후 DB insert
    - .update() : self.instance 인자를 지정시
    - .create() : self.instance 인자를 지정하지 않았을 시



## DRF에서 유일성 체크를 위한 Validators 제공

- UniqueValidator: 지정 1개 필드가 QuerySet 범위에서 유일성 여부 체크
  - 모델 필드에 **unique=True 지정시 자동 지정**
    - QuerySet 필수
    - Message: 유효성 검사 실패시 에러 메세지
    - lookup: default exact (매칭)
    - `validators=[UniqueValidator(queryset=Post.objects.all())]`
      - 가급적 모델 필드에 Unique=True 설정으로 사용 
- UniqueTogetherValidator: UniqueValidator의 다수 필드
- UniqueForDateValidator: 유일성 여부 체크
  - `validators=[UniqueForDateValidator(queryset=Post.objects.all(), field='slug', date_field='published')]`
  - queryset 필수 (1차 범위)
  - field 필수 (조건 부여)
  - date_field 필수 (2차 범위)
- UniqueForMonthValidator: 지정 월 범위 유일성 체크
- UniqueForYearValidator: 지정 년 범위 유일성 체크



## 유효성 검사 실패시 ValidationError 예외 발생

### **rest_framework.exceptions.ValidationError 사용** 

- serializer의 유효성 검사 방법 -> 필드 정의시 **validators 지정** 또는 **클래스 Meta.validators 지정**
- APIException예외 클래스의 상속을 받아 처리
- django.forms.exceptions.ValidationError X
- form에서는 `clean` 멤버함수로 serializer에서는 `validate_` 멤버함수로 구현



### Mixins perform_ 계열 함수

Create시 추가로 DB 반영(저장, 수정, 삭제)

- perform_create(serializer)
- perform_update(serializer)
- perform_destroy(instance)



## 인증

유입되는 요청에 대해 **허용/거부** 하는 것이 아니라 **단순 인증 정보로 유저를 식별**하는 것

- Authentication: 유저 식별
- Permissions: 각 요청에 대한 허용/거부
- Throttling: 일정 기간 허용 최대 횟수

### 인증 처리 순서 (클라이언트에서 서버에 요청시 매번 인증)

1. 매 요청시 APIView의 dispatch(request) 호출
2. APIView의 Initial(request) 호출
3. APIView의 perform_authentication(request) 호출
4. request의 user 호출
5. request의 _authentiacate() 호출 

### 지원 인증 종류

- SessionAuthentication
  - 세션을 이용한 인증 (APIView에서 default set)
    - 웹프론트엔드와 장고가 같은 host를 사용시 가능 (nginx 활용), 외부 서비스 및 앱에선 사용 불가
  - 장고 기본 앱에서 `django.contrib.auth`를 이용해 지원 (로그인, 로그아웃)
- BasicAuthentication
  - Basic 인증 헤더를 이용한 인증 (Authorization -> Basic "암호")
  - 외부 서비스/앱에서 매번 헤더에 유저 정보를 넘기는 것은 보안상 위험하고 하지 말아야할 것
  - HTTPie를 통한 요청 -> `http --auth 유저명:암호 --form POST :8000 필드명1:값 필드명2:값`
- TokenAuthentication
  - Token 헤더를 이용한 인증 (Authorization -> Token "토큰암호")
  - username/password 로 Token을 발급받고 토큰에 API 요청을 담아 보내 인증처리
  - 단점: 만료 시점이 없고 기본 토큰은 1개만 부여되기에 유출시 위험
- RemoteuserAuthentication

### Token Model

User 모델과 1:1 관계이며 각 User별 토큰을 수동 생성해야 합니다. Token은 유저별로 유일하며 오직 Token으로만 인증을 수행합니다.

### Token 생성 방법

- ObtainAuthToken: View를 이용한 get_or_create (url 매핑 필요)
- Signal: signal을 이용한 자동 생성
- Management: 명령을 이용한 생성



## 인증과 허가

클라이언트측에서 요청한 개체에 접근을 허용하기 위해 인증/식별만으로 충분하지 않고 유저마다 적절한 추가적 허가가 필요합니다.

### DRF Permission 

현재 요청에 따른 허용/거부 여부를 결정하고 APIView 단위별로 지정 가능

- AllowAny (default 전역 설정): 인증 여부에 상관없이 View 호출 허용
- IsAuthenticated: 인증된 요청에 한해 View 호출 허용
- IsAdminUser: staff 인증 요청에 한해 View 호출 허용
- isAuthenticatedOrReadOnly: 비 인증 요청엔 읽기만 허용
- DjangoModelPermissions: 인증된 요청에 한해 View 호출 허용, 추가 장고 모델 단위 Permission 확인
- DjangoModelPermissionsOrAnonReadOnly: DjangoModelPermissions에 비인증 요청엔 읽기만 허용
- DjangoObjectPermissions: 비인증 요청은 거부, 인증 요청엔 Object 권한 체크

### Default global setting

```python
REST_FRAMEWORK = {
		'DEFAULT_PERMISSION_CLASSES': [
				'rest_framework.permissions.AllowAny',
		]
}
```

### Custom permission

Permission 클래스는 2가지 함수를 구현할 수 있습니다.

- has_permission(request, view)
  - api 접근시 체크
  - Permission 클래스에서 구현하며 로직에 따라 True/False 반환
- has_object_permission
  - APIView의 self.get_object 함수를 통해 object 반환시 체크
  - DjangoObjectPermissions에서 구현하며 로직에 따라 True/False 반환
  - 브라우저를 이용해 API 접근할때 CREATE/UPDATE Form 노출시 체크

### has_object_permission 

SAFE_METHODS = ['GET', 'HEAD', 'OPTIONS']  -> 조회 요청 (인증 여부에 상관없이 허용)

PUT, DELETE는 저자와 유저가 같아야 허용

- get : get 요청
- head : 실제 응답 head만 받아보는 기능
- options : 엔드포인트에서 어떤 메소드를 지원하는지 알려주는 인터페이스









## Pagination

- PageNumber
  - page/page_size 인자를 이용한 처리 
- LimitOffset
  - offset/limit 인자를 이용한 처리

### PageNumberPagination

page_size default 지정이 필요, `setting.py` 내 `REST_FRAMEWORK = {"PAGE_SIZE" : 10}` 선언하여 전역 설정 가능합니다.

특정 API내에 custom page size를 지정시 PageNumberPagination을 상속받아 사용할 수 있습니다.

```python
class MyLimitOffsetPagination(LimiOffsetPagination):
		page_size = 10
```

### settings에 pagination 설정 전 후 차이 (좌: True, 우: False)

![image](https://user-images.githubusercontent.com/48043799/110532552-52260900-8160-11eb-8cac-d6a4b1103031.png)



## Throttling

최대 호출 횟수 제한하기

- Rate: 지정 기간내 허용할 최대 호출 횟수
- Scope: Rate에 대한 별칭
- Throttle: 특정 조건에 최대 호출 횟수를 결정하는 로직 구현 클래스

### Default Throttle

- AnonRateThrottle
  - 인증 요청 제한 x
  - 비인증 요청은 IP 단위로 횟수 제한
- UserRateThrottle
  - 유저 단위로 횟수 제한
  - 비인증 요청은 IP 단위로 횟수 제한
- ScopedRateThrottle
  - 유저 단위로 횟수 제한
  - 비인증 요청은 IP 단위로 횟수 제한
  - APIView내 throttle_scope 설정을 체크하여 개별로 서로 다른 Scope를 적용

### Setting example

```python
# default
REST_FRAMEWORK = {
		'DEFAULT_THROTTLE_CLASSES': [],
		'DEFAULT_THROTTLE_RATES': {
				'anon': None,
				'user': None,
      	'upload': '20/day',
      	'contact': '100/day'
		}
}

# setting 
REST_FRAMEWORK = {
		'DEFAULT_THROTTLE_CLASSES': [
      	'rest_framework.throttling.UserRateThrottle'
    ],
		'DEFAULT_THROTTLE_RATES': {
				'user': '7/day', # 7times a day -> message if exceeded "Too Many Request"
		}
}

# ViewSet (APIView 별)
from rest_framework.throttling import UserRateThrottle
class PostViewSet(ViewSet):
  		throttle_classes = UserRateThrottle
    
    
# API별로 서로 다른 Rate 적용 exam
class ContactListView(APIView):
  		throttle_scope = 'contact' # 1000 times a day
    
class UploadView(APIView):
		  throttle_scope = 'upload' # 20 times a day
```

 

### TooManyRequests

예외 메세지를 통해 Retry-After: 86396 형태로 재요청 보낼 수 있는 시점을 안내해줍니다. (Throttle의 wait 멤버함수를 이용해 계산)



## Cache

### cache-list

기본 setting의 default는 로컬 메모리 캐시입니다. (서버가 재시작시 캐시는 초기화)

- memcached cache: django.core.cache.backends.MemcachedCache
- db cache: django.core.cache.backends.DatabaseCache (not support)
- file system cache: django.core.cache.backends.FIleBasedCache (not support)
- local memory cache: django.core.cache.backends.LocMemCache

### Redis를 활용한 캐시

[django-redis-cache](https://github.com/sebleier/django-redis-cache)

요청시 cache에선 timestamp list를 get/set하며 SimpleRateThrottle에는 다음과 같이 default 캐시 설정

```python
from django.core.cache import cache as default_cache

class SimpleRateThrottle(BaseThrottle): # 코드 참고 (rest_framework/throttling)
		cache = default_cache
```

### Setting format

- 포맷:  "숫자/간격"
- 숫자: 지정 간격내 최대 요청 제한 횟수
- 간격: 지정 문자열의 첫글자만 사용
  - "s": 초, "m": 분, "h": 시, "d": 일

### IP 단위의 요청 기준

X-Forwarded-For > REMOTE_ADDR 순으로 헤더를 참조해서 확인 (Load-Balancer or nginx등을 활용할 수 있기에)



## Reference

- https://educast.com/course/web/ZU53/
- https://wikidocs.net/book/837