# 장고를 시작할 때 알아두면 좋은 개념



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

**Relation ship Types**

- ForeignKey
- ManyToManyField
- OneToOneField



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



## 클래스 메타 옵션

### db_table

- 모델에 사용할 DB 테이블 이름 명시
- `db_table = 'USER_ACCOUNT'`

### managed

- 기본값 true, 장고에 해당하는 db 테이블생성(장고가 DB 테이블 생명주기 관리 -> 마치 addy에서 애드캠퍼스 유저의 managed = False 해놓은것과 같은 의미 (영향을 주지 않겠다))
- False 일때 모델에 대한 db 작성, 삭제 수행되지 않음(기존테이블로 운용시)
- False 옵션일때 모델이 ManyTomanyField를 가리키는 경우 다 대 다 조인에 대한 중간 테이블이 작성되지 않음



## 관계 표현 모델필드

### OneToOneField

1:1 관계 표현 PK와 FK 관계와 동일하지만 관계가 상하가 아닌 동등한 상태로 가리키는 물체를 반환한다. (하나의 object)

(unique=true, foreignKey 개념) -> Review 좋아요나 조회수 테이블에서 리뷰id 하나를 가리킬때

쉽게 말해 fk의 테이블의 PK가 상위 테이블의 fk 하나를 바라볼때 (조인 후 where 조건을 1:1 매칭으로 걸 수 있을 때)

### ForeignKey

1:N 관계이며 게시글과 댓글이 대표적인 예

N쪽인 관계에서 선언하며 두개의 파라미터가 필요하다 (대상이되는 클래스, 삭제 이슈)

### ManyToManyField

다대다 관계를 의미,  여러개의 글과 여러 태그등이 대표적인 예

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