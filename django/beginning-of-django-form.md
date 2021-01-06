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
	fields = '__all__' # 전체 필드__all__, 개별은 ['field_1', 'field_2', ...]
```

```python
form = PostForm(request.POST, request.FILES)
post = form.save(commit=True) #default commit=True 설정, False시 save 호출 작동 안함(저장 안됌)

```

참고사항으로는 form.save() != instance.save()  같지 않고 form.save에서 내부적으로 instance.save 기능을 호출할 수 있고 인자로 commit 옵션을 보내주어 사용합니다.

commit=False는 instance.save() 함수 호출을 지연시키고자 할 때 사용합니다.





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





## Reference

- https://educast.com/course/web/ZU53/
- https://wikidocs.net/book/837