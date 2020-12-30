# 장고를 시작할 때 알아두면 좋은 개념 - View



## View란?

한번의 http 요청에 하나의 view가 호출됩니다. urls.py내에 urlpatterns 리스트안에 매핑하여 사용하고 크게 클래스, 함수 2가지 형태로 나뉩니다.

- 함수 기반 뷰 : 호출 가능한 객체 자체
- 클래스 기반 뷰 : 클래스.as_view()를 통해 호출 가능한 객체를 생성 및 리턴



## View의 인자

첫번째 인자: HttpRequest 객체

- 현재 요청에 대한 모든 내역을 가지고 있습니다.

두번째 인자: 현재 요청의 url에 Capture된 문자열

- path를 통해 전달된 인자는 Converter의 to_python에 맞게 변환된 값이 인자로 전달됩니다. `path('<int:pk>', view.post_detail) -> int로 변환된 인자 전달` 



## View 호출의 리턴

render함수를 통해 HttpResponse를 리턴해야합니다. 만약 다른 타입을 리턴하게 된다면 Midellware에서 처리 오류를 발생시킵니다.

```python
return render(request, 'instagram/post_list.html') #리턴

#render 함수 설명
def render(request, template_name, context=None, content_type=None, status=None, using=None):
    """
    Return a HttpResponse whose content is filled with the result of calling
    django.template.loader.render_to_string() with the passed arguments.
    """
    content = loader.render_to_string(template_name, context, request, using=using)
    return HttpResponse(content, content_type, status)
```

### 파일 객체 혹은 str/bytes 타입 response 

```python
response = HttpResponse()
response.write("hello world") #장고내에 encode 자동처리
return response
```

<img width="328" alt="스크린샷 2020-12-29 오후 11 16 31" src="https://user-images.githubusercontent.com/48043799/103290677-6e554d80-4a2d-11eb-8c30-c777f9f4f004.png">



## HttpRequest & HttpResponse

```python
from django.http import HttpResponse, HttpRequest
def index(request: HttpRequest, pk: int) -> HttpResponse:
   #request.method
   #request.META, request.GET, Post, body, FILES등
	 return pass
```



## URL Dispatcher

특정 URL 패턴 -> View List

프로젝트/settings.py의 최상위 URLConf 모듈을 지정할 수 있으며 최초의 urlpatterns로 부터 include를 통해 트리 구조로 확장도 가능합니다. http 요청에 따라 등록된 root_urlconf  매핑 리스트를 순차적으로 확인하면서 url 매칭하는 구조로 이루어져있습니다. (매칭되는 url이 여러개라면 처음 Rule만 사용)

```python
#프로젝트/settings.py
ROOT_URLCONF = '프로젝트명.urls'

#프로젝트/urls.py
urlpatterns = [
    path('admin/', admin.site.urls),
    path('instagram/', include('instagram.urls')), 
]

#프로젝트/instagram/urls.py
urlpatterns = [
    path('', views.post_list), #instagram/
    path('<int:pk>/', views.post_detail) # instagram/2
]

```



## URL Patterns Ex

```python
urlpatterns = [
  path('', views.post_list, name='post_list'), 
  path('new/', views.post_new, name='post_new'),
  path('<int:id>/', views.post_detail, name='post_detail'),
  re_path(r'^(?P<id>\d+)/$', views.post_detail, name='post_detail'), #정규표현식 
]

```



## path와 re_path

path는 Path converters를 통해 정규표현식 기입을 간소화시켜주는 기능을 합니다. re_path는 장고 2.x 에서 기존 1.x url 정규표현식 스타일을 동일하게 쓰기 위한 기능을 합니다.

기본 제공 path converters에는 IntConverter, StringConverter, UUIDConverter, SlugConverter, PathConverter등이 있습니다.

한글을 매칭하고 싶을때는 SlugUnicodeConverter를 사용합니다.

### re_path 기호

- ^: 정규표현식 시작기호
- $: 정규표현식 종료 기호
- r: 이스케이프 기호 (raw의 약자로 \를 자동 이스케이프 처리되는 파이썬 기본 문법)



## 다양한 정규표현식 패턴 Ex(띄어쓰기 x)

- 1자리 숫자: `"[0-9]"` or `r"[\d]"`
- 2자리 숫자:  `"[0-9][0-9]"` or `"\d\d"`
- 3자리 숫자: `r"\d\d\d"` or `r"d{3}"`
- 자릿수 범위 숫자: `r"\d{2,4}"`  (10 또는 3049)
- 핸드폰 번호: `r"010[1-9]\d{7}"`
- 알파벳 소문자, 대문자: `"[a-z]"` or  `"[A-Z]"`

### 반복횟수 지정

- r"\d{1}" -> 1회
- r"\d{3,5}" -> 숫자 3자리~5자리 사이
- r"\d?" -> 0회 또는 1회
- r"\d*" -> 0회 이상
- r"\d+" -> 1회 이상



## 새로운 장고 앱 생성시 작업 순서

1. 앱 생성
2. 앱이름/url.py 파일 생성
3. 프로젝트/url.py include 등록
4. 프로젝트/settings.py INSTALLED_APPS에 앱이름 등록



## 함수 기반 View

View 구현의 가장 기초 및 기본, 공통 기능들은 장식자 문법 사용

```python
@api_view(["GET"])
@throttle_classes([OncePerDayUserThrottle])
def PostView(request, id):
		return Response({"message": "STATUS_RESPONSE_SUCCESS"})
```



## 클래스 기반 View

공통 기능들은 상속 문법 사용, View 함수를 만들어주는 클래스로 as_view() 클래스 함수를 통해 View 함수 생성, 상속을 통해 여러 기능을 사용할 수 있습니다.

django.views.generic는 장고의 기본 클래스뷰 패키지입니다.

### View

모든 CBV의 모체이며 http method 요청에 따라 get, post, put, delete 멤버 함수를 호출하여 구현합니다.

```python
class PostAPI(APIView):
  throttle_classes = [OncePerDayUserThrottle] #요청수 제한
	def get(self, request, id):
		return Response({"message": "STATUS_RESPONSE_SUCCESS"})
  
  def post(self, request):
    return Response({"message": "STATUS_RESPONSE_SUCCESS"})
```

### get_object_or_404()

```python
try:
   post = Post.objects.get(id=pk)
except Post.DoesNotExist:
   raise Http404
post = get_object_or_404(Post, id=pk) #위 예외처리 코드와 같은 기능
```

## 장고 부트스트랩 4 라이브러리

bootstrap-pagination 기능 활용

- `pip install django-bootstrap4`

- setting.py -> add django-bootstrap4 

## 장고 기본 CBV API 

리스트 참고



## 장식자 (Decorator)

어떤 함수를 감싸는 (wrapping) 함수 (자바의 어노테이션 표기와 같다) 

```python
@login_required
def protected_view(request):
	return render(request, 'myBlog/index.html')
```

### django.views.decorators.http

- require_http_methods, require_GET, require_POST등 (지정 method가 아니면 HttpResonseNotAllowed 응답)

### django.contrib.auth.decorators

- user_passes_test: 지정 함수가 False 반환시 login_url 으로 redirect
  - 해당 유저가 어떤 조건을 부합하는지에 따라 처리할 수 있는 기능 지원
- login_required: 로그아웃 상황에서 login_url 으로 redirect
- permission_required: 지정 퍼미션 없을시 login_url 으로 redirect
  - 장고 기본 퍼미션 시스템으로 판단하여 처리할 수 있는 기능을 지원

### django.contrib.admin.views.decorators

- staff_member_required: staff memberr가 아닌 경우 login url redirect

## Reference

- https://educast.com/course/web/ZU53/