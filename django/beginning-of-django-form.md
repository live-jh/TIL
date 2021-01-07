# 장고를 시작할 때 알아두면 좋은 개념 - Form



## HTML Form(클라이언트)

클라이언트 사용자에게 입력창을 제공하고 이를 서버로 전송하고자 할 때 사용합니다.



## Django Form (서버측)

클라이언트의 전달받은 값들의 유효성 검사 수행하고 이를 DB에 저장하는 처리를 합니다. 또한 HTML Form을 생성하는 기능을 제공하기도 하며 인터페이스만 맞추고 직접

커스터마이징하여 HTML Form을 코딩할 수도 있습니다.



## Form의 enctype

**default setting: "application/x-www-form-urlencoded"**

get요청에서는 위 유형이 강제되며 url 인코딩처리되어 QueryString 형태로 전달됩니다.

**multipart/form-data**

파일 업로드시만 사용 가능합니다.

**"text/plain"**

사용되지 않는 유형



## Form 요청

### GET or Post 

- 요청 URL 뒤에 ? 표시 후 인자를 넣어 보내기
  - x-www-form-urlencoded 만 가능 (QueryString)
  - GET요청에서 주로 사용 (header만 존재하고 body는 존재하지 않는다.)
- 요청 Body에 모든 인코딩 인자를 넣어 보내기 
  - x-www-form-urlencoded, multipart/form-data 둘 다 가능

## 

## HttpRequest 객체

클라이언트의 모든 요청 내용을 담고 있으며 클래스와 함수 기반뷰에 따라 접근 방법이 다릅니다.

- 함수 기반: 매 요청시 첫번째 인자 request로 접근 
- 클래스 기반: 매 요청시 self.request로 접근

### Form 처리 속성

- .method:  "GET" or "POST" 로 대문자 사용

- .GET: GET 인자(QueryDict Type)

- .POST: POST 인자(QueryDict Type)

- .FILES: POST 인자중 파일 목록 (MultiValueDict Type)

  - MultiValueDict은 동일 key에 다수의 value를 지원하고 Immutable(불변)적 특징을 가지고 있습니다.

  - ```
    exam_dict = MultiValueDict({'name':['mount', 'james'], 'position':['MF, DF'] })
    
    exam_dict.getlist('name') #동일 key로 다수값 리스트 반환
    ```



## HttpResponse

다양한 응답을 Wrapping하여 반환하며 이때 View는 HttpResponse 객체를 리턴하기를 기대합니다. (Middleware에서 기대)

또한 `.write(), .flush(), .tell()` 등의 함수를 사용할 수 있으며 커스텀 헤더의 추가 삭제, 파일 첨부등으로 응답할 수 있습니다.



## Form

장고 Form의 역할은 입력폼 HTML 생성, 유효성 검증, 검증 후 값의 Dict 형태로 제공해주는 기능을 합니다. 장고 Form은 TextField가 없으므로 widget으로 커스텀하여 사용합니다.

```python
from django import forms
from django import models

def min_length_3_validator(val): #별도 유효성 검사 (model에서도 활용 가능)
  if len(val) < 3:
    raise form.ValidationError('3글자 이상 입력 plz')

    
class Post(models.Model):
	title = forms.CharField(validators=[min_length_3_validator]) #모델에서도 유효성 검사를 설정하면 ModelForm을 이용할때 따로 아래(PostForm)와 같이 설정하지 않아도 사용 가능
	content = forms.TextField()
  
#class PostForm(forms.ModelForm):
 # class Meta:
  #  model = Post
   # fields = '__all__'
    
class PostForm(forms.Form):
	title = forms.CharField(validators=[min_length_3_validator]) #별도 유효성 검사 
	content = forms.CharField(widget=form.Textarea) #유효성 검증에선 사용 x
```

또한 하나의 URL(요청 View)에서 빈 폼을 보여주는 기능과 입력된 값을 검증 후 저장하는 역할 크게 2가지를 동시에 수행합니다.

GET 방식 요청에서는 new or update 입력폼을 리턴하며 POST 방식의 요청에서는 유효성 검사 후 성공시 SUCCESS_URL로 이동하며 실패시 오류메세지와 함께 입력폼을 다시 보여줍니다.

```python

form = PostForm(request.POST, request.FILES) #순서 지키기 (데이터, 파일)
	if form.is_valid(): #유효성 검사 (통과시 진입)
		post = Post(form.cleaned_data)
		post.save()
		return redirect(post)
		
return render(request, 
			 'blog/post_form.html', {
			 'form': form
			 })
```



## ModelForm(admin에선 내부적으로 사용)

장고 Form을 상속받아 지정된 Model로부터 필드 정보를 읽어 Form Fields를 세팅합니다. 내부적으로 해당 Model의 Instance를 유지하며 유효성 검증에 통과한 값으로 저장 및 수정 기능을 지원합니다.

```python
from django import forms

class PostForm(forms.ModelForm):
	class Meta:
	model = Post
	fields = '__all__' # 전체 필드__all__, 개별은 ['field_1', 'field_2', ...], exclude는 제외시킬 필드 -> 추천하지 않음
```

```python
form = PostForm(request.POST, request.FILES)
post = form.save(commit=True) #default commit=True 설정, False시 save 호출 작동 안함(저장 안됌)

```

참고사항으로는 form.save() != instance.save()  같지 않고 form.save에서 내부적으로 instance.save 기능을 호출할 수 있고 인자로 commit 옵션을 보내주어 사용합니다.

commit=False는 instance.save() 함수 호출을 지연시키고자 할 때 사용합니다.



## django request meta (유저 ip정보)

- REMOTE_ADDR
- models.GenericIPAddressField(IPv4, IPv6 저장 필드)

```python
#request.POST
form = CommentForm(request.POST, request.FILES)
if form.is_valid():
	comment = form.save(commit=False)
	comment.ip = reqeust.META['REMOTE_ADDR'] #IP 가져오기
	comment.save()
	return redirect('/')
else:
	form = CommentForm()
```





## 사이트간 요청 위조 공격 & 방어를 위한 Token 체크

사용자가 의도하지 않은 게시판에 글을 쓰거나, 쇼핑을 하게 하는 공격등이 있습니다.

이미지 태그에 src="http://site-victim.com/travel/102/update/?src=Bali&dest=Korea" 처럼 사이트 url의 파라미터를 보내거나 또는 post요청을 보내는등 다양한 요청 위조가 가능합니다.

이러한 공격을 막기 위해 POST 요청에 한해서 Django는 CsrfViewMiddleware로 방어를 할 수 있습니다. POST 요청시 Token값이 없거나 유효하지 않으면 403 응답을 보냅니다.

### 처리순서

1. 입력 Form을 보여주고 CSRF Token값도 함께 할당
   1. CSRF Token은 사용자별로 다르며 지속적으로 변경
2. 입력을 통한 Form이 전달 될 경우 Token 유효성 검증

CSRF_Token은 유저인증 토큰이 아니며 JWT(Json Web Token) 또한 아닌 전혀 다른 개념의 토큰인 것을 알고 있어야 합니다.  현재의 요청이 유효한지에 대한 토큰일뿐 유저인증과는 전혀 별개의 토큰인 것을 참고해야 합니다.

사용하는데 거의 비용이 들지 않으므로 기본적으로 사용하지만 만약 특정 View에 따라 Csrf Token 체크를 하지 않고 싶을때는 해당 View에 `@csrf_exempt` 장식자를 추가하는 방법을 사용합니다. 

또한 DRF(django-rest-framework)의 APIView엔 csrf_exempt가 자동으로 적용되어져 있습니다. (앱인증은 jwt, 유저인증토큰으로 활용)

### 쿠키에서 csrf_token 가져오기

```javascript
function getCookie(name) {
    let cookieValue = null;
    if (document.cookie && document.cookie !== '') {
        const cookies = document.cookie.split(';');
        for (let i = 0; i < cookies.length; i++) {
            const cookie = cookies[i].trim();
            // Does this cookie string begin with the name we want?
            if (cookie.substring(0, name.length + 1) === (name + '=')) {
                cookieValue = decodeURIComponent(cookie.substring(name.length + 1));
                break;
            }
        }
    }
    return cookieValue;
}
const csrftoken = getCookie('csrftoken');
```



## 유효성 검사 호출 로직

1. **form.full_clean() 호출**
   1. 각 필드 객체 별로(해당 `객체.clean()`을 통해 각 필드 type에 맞춰 검사),  필드 2개 이상일때 사용
      1. `다수 필드`에 대한 검사/변경, 예외 발생시 non_field_errors로 분류
   2. Form 객채 내에서 필드 별로 `form.clean_필드명()` 함수가 존재할 시 호출 후 유효성 검사
      1. `특정 필드별` 검사/변경, 예외 발생시 해당 필드 Error로 분류
2. **에러 유무에 따른 True/False**
   1. Validator 함수를 통한 유효성 검사
      1. 원하는 조건에 부합하지 않을 시 ValidationError 예외를 발생시키며 리턴값은 사용되지 않습니다.

> **참고**
>
> https://github.com/django/django/blob/master/django/forms/forms.py
>
> def _clean_fields(self):

## Validator 지정

함수형과 클래스형 2가지로 나뉘며 **함수형**은 유효성 검사를 수행할 데이터 값 즉, 하나의 인자를 받은 객체 callable(객체리턴) object이며 **클래스형**은 클래스의 인스턴스 자체가 callable object입니다. 주요 클래스 validator로는 RegexValidator(__call__ 메소드 사용)가 있고 함수의 형태로 구현 할때는 validators 함수를 정의하고 해당 모델 클래스 필드내에 `validators=` 정의한 함수를 옵션을 주어 사용하게 됩니다. 보통 빌트인 Validators를 가장 많이 사용합니다.

### Built-in Validator

- RegexValidator
- EmailValidator
- URLValidator
- validate_ipv4_address
- MaxValueValidator
- MinValueValidator
- ...

### 모델 필드에 default 적용된 validators

EmailField, URLField, GenericIPAddressField, SlugField

### validator 사용시 알아야 하는 점

- 모든 validators는 모델에 정의하고, ModelForm을 통해 모델의 validators 정보 함께 사용



## Form clean 함수의 순기능

- 필드별 Error 기록 or Non 필드 Error 기록
  - add_error(필드명, 오류내용) 함수를 통해 직접호출 가능
- 원하는 format으로 값 변경 (리턴값을 이용)



### Example Validators

```python
class GameUser(models.Model):
server = models.CharField(max_length=10)
username = models.CharField(max_length=20, validators=[MinLengthValidator(3)]) # validators 체크 최소 길이 3 이상

#unique 속성 설정
	class Meta: 
    unique_together = [ 
      ('server', 'username'),
    ]
  
  
class GameUserSignupForm(forms.ModelForm): 
  class Meta:
		model = GameUser
		fields = ['server', 'username']
	
  #해당 필드의 값 변경은 clean 함수에서만 가능하며 Validator내에서는 지원하지 않음
  def clean_username(self):
	return self.cleaned_data.get('username', '').strip()
```





## Reference

- https://educast.com/course/web/ZU53/
- https://wikidocs.net/book/837