# 장고를 시작할 때 알아두면 좋은 개념 - Models



## 장고의 주요 기능들

1. Class Based Views: 클래스로 함수 기반 View 생성
2. Forms: 입력폼 생성, 유효성 검사 및 DB insert
3. Testing : 테스트 지원
4. Internationalization & Localization: 텍스트, 날짜, 시간, 숫자형식등을 방문자의 언어와 포맷에 맞게 제공
5. Cashing: 메모리 캐시, 디스크 캐시 지원
6. Geographic: DB의 Geo(GIS 웹 어플리케이션 구축) 기능 활용 (PostgreSQL 중심)
7. Sending Emails: mail module을 통해 이메일 전송 기능 지원
8. Syndications Feeds (RSS/Atom): 블로그나 뉴스등 갱신되는 내용을 feed 받을 수 있도록 제공하는 기능
9. Sitemaps: 사이트맵 기능 지원(웹사이트의 설계 및 페이지 목록)



## ORM

객체의 관계를 DB와 연결해주는 것 SQL과 파이썬 코드를 매핑함으로 코드로 더 쉽게 DB를 다룰 수 있게 되는 것을 말한다. (DB 테이블 == models.py) 1:1 매핑



## 장고 커스텀 모델 정의

- 모델 클래스명은 **단수**로 사용
- 데이터베이스 구조 및 타입을 **설계 후 모델 정의**
- **길이 제한이 없는 문자열**을 많이 쓰면 **성능 좋지 않음**
- CharField은 반드시 max_length 옵션 부여(varchar 길이 초기화)

```python
from django.db import models

class Post(models.Model): 
	title = models.CharField(max_length=100) #길이 제한(100) 있는 문자열
  description = models.TextField() #길이 제한 없는 문자열
  created_at = models.DateTimeField(auto_now_add=True) #해당 테이블 레코드 생성시 현재 시간 자동 저장
  updated_at = models.DateTimeField(auto_now=True) #레코드 갱신시 현재 시간 자동저장
```



## 모델 등록절차

1. 모델 작성 후 terminal에서 `python manage.py makemigrations`
2. `python manage.py migrate`
   1. 그외 migrate 명령어
      1. `python manage.py showmigrations 앱이름` -> migrations 리스트 보기 (전체 보고 싶을땐 앱 이름 제거)
3. Admin.py에 모델 등록

```python
from django.contrib import admin
from .models import Post

admin.site.register(Post) #생성한 Post 모델 admin에 등록
```



## 지원하는 모델 필드 타입

**Field Types** 

- AutoField
- BooleanField
- CharField
- DateTimeField
- FileField
- ImageField
- TextField
- 그외 EmailField, URLField, UUIDField 등

**Relation ship Types**

- ForeignKey
- ManyToManyField
- OneToOneField

### 파이썬데이터타입과 DB 데이터 타입 매핑

- AutoField -> int
- BinaryField -> bytes
- BooleanField -> bool
- CharField/URLField/EmailField -> str

## 주요 필드 옵션

- null : null 허용 여부 (default : False)

- unique : unique 여부 (default : False)

- blank : 입력값의 유효성 검사시 empty 허용 여부 (default : False)

- default : 기본값 지정, 값이 지정되지 않았을 때 사용

- verbose_name : 필드 레이블 지정하지 않을시 필드명 사용(소문자 지정)

  - oreignKey, ManyToManyField, OneToOneField 는 첫번째 인자로 모델 클래스를 받기 때문에, 해당 타입의 필드에 verbose name을 지정하려면 아래와 같이 키워드 인자로 전달하면 됩니다.

  - ```python
    poll = models.ForeignKey(Poll, verbose_name="the related poll")
    sites = models.ManyToManyField(Site, verbose_name="list of sites")
    place = models.OneToOneField(Place, verbose_name="related place")
    ```

- validators: 입력값 유효성 검사 수행할 함수를 여러개 지정

  - Ex: 최대 길이 제한, 최소값 제한, 이메일만 받기등

- choices : select box 소스로 사용 (form widget 용)

- help_text : 필드 입력시 도움말 (form widget 용)

- Auto_now_add : Bool, True인 경우 레코드 생성시 현재 시간으로 자동 입력



## DB 설계시 TIP

- 설계한 DB 구조에 따라 다르지만 최대한 필드타입을 타이트하게 지정해주기(입력값의 오류 방지 및 성능 최적화)
- blank/null 지정은 최소화
- validators를 추가로 지정하기
  - **프론트엔드**의 유효성검사는 **사용자 편의**, **백엔드** 유효성 검사는 **필수**
  - 직접 유효성 로직은 최대한 만들지 않기 -> 장고 Form, Serializer를 사용하기

## 클래스 메타 옵션

### db_table

- 모델에 사용할 DB 테이블 이름 명시
- `db_table = 'USER_ACCOUNT'`

### managed

- 기본값 true, 장고에 해당하는 db 테이블생성(장고가 DB 테이블 생명주기 관리 -> 마치 addy에서 애드캠퍼스 유저의 managed = False 해놓은것과 같은 의미 (영향을 주지 않겠다))
- False 일때 모델에 대한 db 작성, 삭제 수행되지 않음(기존테이블로 운용시)
- False 옵션일때 모델이 ManyTomanyField를 가리키는 경우 다 대 다 조인에 대한 중간 테이블이 작성되지 않음

### DB 테이블명

default: 앱이름_모델명

ex) Blog App -> Post Model 시 표기 : blog_post



## 모델 클래스 설정

### Admin 등록하기

해당 앱/admin.py에 다음과 같이 작성한다.

- `admin.site.register(앱이름)`

### 모델 클래스 __str__

admin 모델 리스트에서 "모델명 obj"를 원하는 대로 변경 (java의 toString()과 유사 역할)

```python
def __str__(self):
	return f"<{self.pk}> {self.message}"
```

### list_display 속성

모델 리스트에 출력할 컬럼들을 지정한다.

```python
@admin.register(Post)  # 래핑
class PostAdmin(admin.ModelAdmin):  # 3번 방법
    list_display = ['pk', 'message', 'created_at', 'updated_at']
    pass
```

![image](https://user-images.githubusercontent.com/48043799/102789542-a33b2200-43e7-11eb-8107-e69b6485f349.png)

> 위에 list_display에 지정한 필드로 admin에 노출된다.



## 파이썬 쉘에서 쿼리 확인하기

터미널에서 명령어 입력

- `python manage.py shell`

- 확인하고자 하는 Model import

  - `from user.models import Profile`
  - `qs = Profile.objects.all()`
  - `print(qs.query)`

  

## 관계 표현 모델필드

### OneToOneField

1:1 관계 표현 PK와 FK 관계와 동일하지만 관계가 상하가 아닌 동등한 상태로 가리키는 물체를 반환한다. (하나의 object)

(unique=true, foreignKey 개념) -> Review 좋아요나 조회수 테이블에서 리뷰id 하나를 가리킬때

쉽게 말해 fk의 테이블의 PK가 상위 테이블의 fk 하나를 바라볼때 (조인 후 where 조건을 1:1 매칭으로 걸 수 있을 때)

### ForeignKey

1:N 관계이며 게시글과 댓글이 대표적인 예

N쪽인 관계에서 선언하며 두개의 파라미터가 필요하다 (대상이되는 클래스, 삭제 이슈)

### ManyToManyField

다대다 관계를 의미,  하나의 글엔 여러개의 태그가 있고 하나의 태그가 여러개의 글에 쓰일 수 있는 것이 대표적인 예 

관계를 선언시 어느쪽에서 하던 상관없으며 위에 있는 모델에서 지정시 하위 테이블을 문자열로 표현, 태그를 지정 안할 시를 고려해 blank=True

### select_related()

- ForeignKey, OneToOneField 관계에서 활용

- ForeignKey, OneToOneField관계에서 Lazy하게 쿼리하지 않고 DB 단에서 Inner join으로 쿼리할 수 있다.

- SQL의 JOIN의 형식으로 하나의 쿼리셋을 가져올때 미리 relate(관계 설정) objects들까지 다 불러오는 함수이다. select_related는 foreign_key, OneToOne과 같은 single value 관계에서만 사용 가능하다.

  예를 들어 애드캠퍼스의 대학 리뷰(ml_review) 테이블(하위)은 campus(대학코드)(상위), major(학과코드)(상위), account(유저index)(상위)를 참조할 때 review테이블 기준으로 select_related 설정할 때 사용할 campus와 major를 선언해준다. 자기가 부모일때는 사용 x

### prefetch_related()

- ManyToManyField, ForeignKey의 reverse relation 에서 활용
- 각 관계 별로 DB 쿼리를 수행하고, 파이썬 단에서 조인을 수행한다.
- prefetch_related는 조인하지 않고 개별 쿼리를 실행한 후에 장고가 직접 데이터를 조합한다.

## Static 파일 & Media 파일

### static 파일

개발 리소스로서 정적 파일(js, css, img)

앱 또는 프로젝트 단위로 저장

### media 파일

FileField/ImageField를 통해 저장한 모든 파일들

DB필드엔 저장 경로(url)를 저장하며 파일은 파일 스토리지(S3)에 저장

ImageField를 사용할 때 사용하는 `pip install pillow (이미지 라이브러리)`

### media 파일 처리 순서

1. HttpRequest.FILES 를 통해 파일 전달
2. View, Form 로직을 통해 유효성 검증을 수행 후 경로를 저정한다
3. settings.MEDIA_URL에 저장

```python
#django project settings.py

MEDIA_URL = '/media/' #파일 url 접근시 사용 설정
MEDIA_ROOT = os.path.join(BASE_DIR, 'media') #업로드시 사용되는 설정


#urls.py
if settings.DEBUG:
    urlpatterns += static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
```

### upload_to 옵션 (디렉토리 지정하는 옵션)

default: 파일명 그대로 settings.MEDIA_ROOT에 저장

settings.MEDIA_ROOT 하위에서 지정한한 파일, 경로명 지정(함수 또는 문자열로 지정 가능)

문자열인 경우는 저장할 디렉토리를 작성, 함수일 경우 return값은 저장할 경로의 `문자열로 반환하며 파일명까지 변경`할 수 있다.

`upload_to="instagram/post/%Y/%m%d"` -> instagram/post/2020/12/22/파일명으로 저장



## Model Manager

DB 인터페이스를 제공 default Manager로 ModelCls.objects가 제공합니다.

query_set은 lazy한 특징을 가지고 있습니다.

아래와 같이 orm을 활용해 쿼리셋을 작성하더라도 준비만 하고 있을 뿐 실제로 쿼리를 실행하지는 않습니다. 실행하는 시점은 해당 쿼리 결과를 사용할 때 즉 출력이 필요한 순간에 동작하게 됩니다.

```python
#DB 접근 전
qs = Post.objects.all().order_by('-id')[:2] 

#DB 접근 상황 (그외 -> list(query_set), for in query, print등)
for post in qs:
	print("id: {id}, message: {message}, created_at: ({created_at})".format(**post.__dict__))

#id: 4, message: 첫번째 메세지, created_at: 2020-12-21 15:19:57.925964+00:00
#id: 5, message: 두번째 메세지, created_at: 2020-12-21 15:20:02.017573+00:00
#id: 6, message: 세번째 메세지, created_at: 2020-12-21 15:20:07.441932+00:00

```

## QuerySet Chaining

```python
#__icontains == '%like%'
#order_by '-' = desc

qs = Post.objects.all()\ #\표시로 줄바꿈 가능
		.filter(message__icontains = "첫번째")\
		.order_by('')
    
```

## QuerySet get

````python
qs = Post.objects.all()
# 쿼리셋의 결과 object가 1개만 원할때 get을 사용 (id를 통한 검색)
qs.get(id=2)
qs.get(id__lte=3) #error 
#less than, less than equal(작거나 같다)
# greater than, greater than equal(크거나 같다)

````

## QuerySet filter - exclude & or & and

```python
#필드명 = 조건값
#1개 이상의 인자 지정 -> 모두 and 조건으로 묶인다.
# or 조건으로 묶으려면 django.db.models.Q를 사용
#filter의 반대는 exclude(not)
qs = Post.objects.all().filter(message__icontains="메세지", id__lt=6)
print(qs) #<QuerySet [<Post: 첫번째 메세지>, <Post: 두번째 메세지>]>

# or => Q
qs = Post.objects.all()
cond = Q(id__gte=6) | Q(message__icontains='두')
qs = qs.filter(cond)
print(qs) #<QuerySet [<Post: 두번째 메세지>, <Post: 세번째 메세지>]>

```



## 정렬 조건

DB 자체적으로 다수 필드에 대한 정렬을 지원하지만 가급적 **단일 필드**로 적용하는 것이 성능에 효율적입니다.

### 방법

1. Model class의 Meta 속성으로 order 설정(list 지정) -> **추천**
   1. 기본정렬 이후 query_set에 order_by 정렬을 사용하면 기본 정렬은 무시됩니다.
2. query_set에 order_by 조건 추가



## 슬라이싱

str, list, tuple의 슬라이싱과 비슷한 개념이지만 역순은 지원하지 않습니다. 역순을 알고 싶을때는 정렬을 반대로 지정 후 앞에 2개를 가져오는 방식으로 진행합니다.

`객체[start:stop:step]` step은 쿼리에 대응 x, 사용하지 않는다.

```python
Post.objects.all()[:2] # 앞에서 2개
Post.objects.all().order_by('-id')[:2] # 뒤에 2개
```

step 값을 지정하는 순간 리스트로 변경되기 때문에 lazy한 특징은 사라집니다. 즉 orm으로 쿼리셋을 작성하는 것만으로도 쿼리 요청을 보내는 것이니 이점은 참고해야 합니다.



## 디버그 툴바

디버그 툴바는 **body태그**가 존재해야 기능이 정상적으로 동작합니다.

### root_url.py

```python
if settings.DEBUG:
	import debug_toolbar
	urlpatterns += [
  	  path('__debug__/', include(debug_toolbar.urls))
	]
```

### settings.py

```python
MEDDLEWARE = [
	'debug_toolbar.middleware.DebugToolbarMiddleware' # 추가
]
INSTALLED_APPS = [
	'debug_toolbar', # 추가
]

INTERNAL_IPS = ['127.0.0.1']
```

![스크린샷 2020-12-24 오전 12 00 36](https://user-images.githubusercontent.com/48043799/103010105-188b2c00-457b-11eb-8035-200f2d8dba09.png)

> 적용된 모습 :)
>
> 실서버에서는 False!

SQL 실행 내역을 개발서버 콘솔 표준으로 출력(ajax 내역 포함)하는 기능을 사용하고 싶으면 `django-querycount`

## RDBMS의 관계

### 1:N 관계

models.ForeignKey로 표현

- 1명의 사용자가 쓰는 다수의 글
- 1명의 사용자가 쓰는 다수의 댓글
- 1개의 글의 다수의 댓글

### 1:1 관계

models.OneToOneField로 표현

- 1명의 사용자가 가지는 1개의 profile 테이블

### M:N 관계

models.ManyToManyField로 표현

- 1개의 글에 다수의 태그형식 묶음 (1개 태그는 다수의 포스팅에 쓰임)

### ForeignKey

- 1:N 관계에서 N측에 명시
  - Post(글) - **Comment(댓글)** 관계에서 **댓글**에다가 명시
- to: 대상 모델 (클래스를 직접 입력하거나, 문자열로 지정, 자기 자신 참조는. "self")
- on_delete: 레코드 데이터 삭제시 룰
  - casecade: fk로 참조되는 모델의 데이터도 삭제
  - project: protectedError를 발생시키며 삭제 방지
  - set_null : default null (null=True 필수)
  - set_default: default값으로 대체
  - set: 대체할 값이나 함수 지정, 호출시 리턴값 사용
  - do_nothing: 어떠한 액션도 지정하지 않음



## Django - Serializing multiple objects

다수의 데이터 queryset 형태를 serialize화 하고자 할 때 many=True 사용

```python
book_set = Book.objects.all()
serializer = BookSerializer(book_set, many=True)

print(serializer.data)
# [
	{'id': 1, 'title': '콩쥐팥쥐', 'author': '동화'},
	{'id': 2, 'title': '신데렐라', 'author': '동화'}
]
```

