# 장고를 시작할 때 알아두면 좋은 개념 - Auth



##  [django/conf/global_settings](https://github.com/django/django/blob/master/django/conf/global_settings.py)

- `LOGIN_URL = '/accounts/login/'` 
- `LOGIN_REDIRECT_URL = '/accounts/profile/'`
  - 변경시 현재 프로젝트의 settings.py에서 `LOGIN_REDIRECT_URL` 옵션 추가하여 변경 가능
- `LOGOUT_REDIRECT_URL = None`



## django/contrib/auth/urls.py

- urlpatterns = [ ] 참고

```python
path('login/', LoginView.as_view(), name='login') #login default template_name = 'registration/login.html'
```



## django/contrib/auth/forms.py

### UserCreationForm: 모델 -> 유저, 필드 -> 유저네임, 추가필드 정의 (pw1, pw2)

- clean_password2()
  - 패스워드1에서는 체크하지 않습니다.
  - pw1, pw2의 값이 같은지 다른지 체크
- _post_clean: 패스워드의 길이, 적합한 형식등을 체크하는 함수 
- save(self, commit=True): pw1을 가져와서 save처리
  - 함수내에 set_password('패스워드') -> None일시 로그인처리 할 수 없도록 만드는 것을 의미 (구글인증, 페이스북 Oauth 인증시 유저 패스워드로 로그인 불가처리)



## Reference

- https://educast.com/course/web/ZU53/
- https://wikidocs.net/book/837