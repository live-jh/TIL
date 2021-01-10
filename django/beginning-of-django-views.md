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

urls.py 변경으로 각 뷰에 대한 url 변경되는 유연한 시스템입니다.

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



## URL Reverse

개발자가 하나하나 url을 명시하지 않아도 되는 기능이며 url이 변경되어도 reverse 기능으로 변경된 url을 추적하여 연결해주는 것을 역할을 합니다.

### URL Reverse를 수행하는 4가지 함수

**url template tag**: 내부적으로 reverse 함수 사용

**reverse method**: 매칭 url이 없을때 NoReverseMatch 예외 발생

**resolve_url method**: 매칭 url이 없을때 인자 문자열을 그대로 리턴 (내부적으로 reverse 사용)

**redirect method**: 매칭 url이 없을때 인자 문자열을 그대로 리턴 (내부적으로 resolve_url 사용)

```python
{% url "blog:post_detail" 100 %}
{% url "blog:post_detail" pk=100 %}

reverse('blog:post_detail', args=[100]) # appName:pathName
reverse('blog:post_detail', kwargs={'pk':100}) # 파라미터 {}로 전달

resolve_url('blog:post_detail', 100)
resolve_url('blog:post_detail', 100, pk=100)

redirect('blog:post_detail', 100)
redirect('blog:post_detail', pk=100)
```

### 모델 객체에 대한 detail 주소 계산

`redirect('blog:post_detail', pk=post.pk) -> redirect(post)` 으로 간단히 변경 은 모델 클래스에 `get_absolute_url()` 을 구현합니다. 다른 활용법으로는 CreateView, UpdateView에서 success_url을 지정하지 않을 경우 model instance에 지정한 get_absolute_url 주소를 확인하여 이동이 가능할경우 이동합니다.

```
resolve_url(to, *args, **kwargs): #to는 모델 객체일수도 있고 url을 받을수도 있다.

class Post(models.Model):
    author = models.ForeignKey(settings.AUTH_USER_MODEL, on_delete=models.CASCADE)
    message = models.TextField()
    post_img = models.ImageField(blank=True, upload_to="instagram/post/%Y/%m%d")
    is_public = models.BooleanField(default=False, verbose_name="공개여부")
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)
    tag_set = models.ManyToManyField('Tag', blank=True)

```







## 새로운 장고 앱 생성시 작업 순서

1. 앱 생성
2. 앱이름/url.py 파일 생성
3. 프로젝트/url.py include 등록
4. 프로젝트/settings.py INSTALLED_APPS에 앱이름 등록



## 함수 기반 View

View 구현의 가장 기초 및 기본, 공통 기능들은 장식자 문법 사용합니다. 404, 403, 500 에러 핸들링은 함수기반View를 통해 사용합니다.

```python
@api_view(["GET"])
@throttle_classes([OncePerDayUserThrottle])
def PostView(request, id):
		return Response({"message": "STATUS_RESPONSE_SUCCESS"})
```

### get_object_or_404() 에러처리

```python
try:
   post = Post.objects.get(id=pk)
except Post.DoesNotExist:
   raise Http404
post = get_object_or_404(Post, id=pk) #위 예외처리 코드와 같은 기능


def handler404(request):
  return render(request, '403.html', status=403)

def handler500(request):
  return render(request, '500.html', status=500)
  
```

## 

## 클래스 기반 View

공통 기능들은 상속 문법 사용, View 함수를 만들어주는 클래스로 as_view() 클래스 함수를 통해 View 함수 생성, 상속을 통해 여러 기능을 사용할 수 있습니다. 또한 객체지향을 활용해 코드의 재사용과 개발 생산성을 높여주는 기능을 합니다.

### Generic View

- **Base View**: 뷰 클래스 생성하고, 제네릭 뷰의 모체(부모) 클래스가 되는 뷰
  - View: 최상위 부모 뷰
  - TemplateView: 주어진 템플릿으로 렌더
  - RedirectView: 주어진 URL로 리다이렉트
- **Display View**: 객체 목록, 하나의 객체 상세정보를 보는 뷰
  - DetailView: 조건에 맞는 하나의 객체 출력
  - ListView: 조건에 맞는 객체 목록 출력
- **Edit View**: 폼을 통해 create, update, delete를 수행하는 뷰
  - FormView: 폼을 주어지면 해당 폼 출력
  - CreateView: 객체를 생성하는 폼 출력
  - UpdateView: 기존 객체 수정하는 폼 출력
  - DeleteView: 기존 객체 삭제하는 폼 출력
- **Date View**: 날짜 기반의 객체 연/월/일로 구분해 보여주는 뷰
  - YearArchiveView: 연도에 해당하는 객체 출력
  - MonthArchiveView: 월에 해당하는 객체 출력
  - DayArchiveView: 일에 해당하는 객체 출력
  - TodayArchiveView: 오늘에 해당하는 객체 출력
  - DateDetailView: 주어진 연,월,일 PK에 해당하는 객체 출력

### Generic View Overriding

**model**

BaseView를 제외한 모든 제네릭 뷰에서 사용

**queryset**

queryset을 사용하면 model은 무시 (BaseView를 제외한 모든 제네릭 뷰 사용)

**template_name**

TemplateView를 포함한 모든 제네릭 뷰에서 사용, 문자열로 지정

**context_object_name**

view에서 템플릿 파일에 전달하는 컨텍스트 변수명 지정

**paginate_by**

ListView와 날짜 기반 View에 사용 (페이징 기능 활성화 된 경우 페이지당 row 갯수 지정)

**date_field**

날짜 기반 뷰에서 사용 (필드타입은 DateField or DateTimeField)

**form_class**

FormVIew, CreateView, UpdateView에서 폼을 생성시 클래스로 지정

**success_url**

edit view 폼에 대한 처리가 성공시 리다이렉트할 url 지정

### method overriding

`def get_queryset()`, `def_get_context_data(**kwargs)`, `def form_valid(form)`

### 모델을 지정하는 방식 3가지

- model 속성 변수 지정

  - `model = Post`

- queryset 속성 변수 지정

  - `queryset = Post.objects.filter(is_public=True)`

- `def get_queryset()` 오버라이딩

  - ```python
    def get_queryset(self):
    		qs = super().get_queryset() 
            if not self.request.user.is_authenticated: 
                qs = qs.filter(is_public=True) 
            return qs
    ```



### Generic List, DetailView Ex

```python
#예제코드
from django.views.generic import ListView, DetailView
from .models import Post

#ListView
class PostListView(ListView):
  	model = Post
    paginate_by = 10
    throttle_classes = [OncePerDayUserThrottle] #요청수 제한
	
	  def get_queryset(self):
        return Post.objects.order_by('-created_at')[:20]
  
post_list = PostListView.as_view()

#DetailView
class PostDetailView(DetailView):
    model = Post
		
    def get_queryset(self):
        # request 인자는 class기반 View에서 self.request로 접근가능
        qs = super().get_queryset()  # 재정의할때 super 호출
        if not self.request.user.is_authenticated: 
            qs = qs.filter(is_public=True)  # 공개 게시물만 허용
        return qs


post_detail = PostDetailView.as_view()




```

### CreateView, UpdateView, DeleteView

```
#CreateView
class PostCreateView(CreateView):
    model = Post
    form_class = PostForm

    def form_valid(self, form):
        self.object = form.save(commit=False)
        self.object.author = self.request.user
        messages.success(self.request, '메세지를 등록하였습니다.')
        return super().form_valid(form)


post_new = PostCreateView.as_view()

#UpdateView

#DeleteView

```



## 장고 부트스트랩 4 라이브러리

bootstrap-pagination 기능 활용

- `pip install django-bootstrap4`

- setting.py -> INSTALLED_APPS  add "bootstrap4"



## 장식자 (Decorator)

어떤 함수를 감싸는 (wrapping) 함수 (자바의 어노테이션 표기와 같다) 

```python
@login_required #함수
def protected_view(request):
	return render(request, 'myBlog/index.html')


@method_decorator(login_required, name="dispatch") #클래스형
class PostListView(ListView):
    model = Post
    paginate_by = 10

post_list = PostListView.as_view()
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



## Http 상태코드

HttpResponse 클래스마다 고유한 status_code 할당, REST_API 생성시 사용합니다.

### 200번대: 성공

- 200: 서버 요청 처리 success
- 201: 요청 접수 후 새 리소스 작성시

### 300번대: 요청을 마치기 위해 추가 조치 필요

- 301: 영구이동, 요청 페이지가 새 위치로 영구적 이동
- 302: 임시이동, 요청자는 향후 원래 위치를 계속 사용해야할 때

### 400번대: 클라이언트측 오류

- 400: 잘못된 요청
- 401: 권한 없음
- 403: 필요한 권한을 가지고 있지 않아 요청 거부
- 404: 서버에서 요청한 리소스 찾을 수 없음
- 405: 허용되지 않는 요청 (ex: get인데 post요청시)

### 500번대: 서버측 오류

- 500: 서버 내부 오류



## Reference

- https://educast.com/course/web/ZU53/
- https://wikidocs.net/book/837