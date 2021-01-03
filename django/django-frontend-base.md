# 장고 프론트엔드 기초 요약

### CSS/JS 파일을 별도로 분리하는 이유

- HTML 응답 body 크기를 줄일 수 있습니다.
- 해당 페이지를 새로고침하더라도, 브라우저 캐싱으로 인해 같은 파일을 서버로부터 다시 읽어들이지 않습니다.
- 웹페이지의 응답성을 높여줄 수 있습니다.

## 웹 요청 및 응답

- 웹은 http프로토콜로 동작

- 클라이언트가 웹서버로 요청하며 웹서버는 이에 따른 응답

  - 응답은 html 코드 문자열, css, js, zip, mp4등 다양한 포맷으로 가능

- 웹서버에서 응답을 만들때 요청의 종류 구분 기준

  - url, request header, session, cookie등

  

## 하나의 HTTP 요청 -> 하나의 응답

1. 브라우저에서 서버로 HTTP 요청

2. 요청에 대한 서버는 장고 View 함수 호출

3. 요청을 받은 View 함수가 리턴해야만 HTTP 응답이 시작되며, HTTP 응답을 받기 전까지 브라우저는 **하얀 화면**만 보여지게 됩니다. 따라서 View의 처리 시간이 길면 길수록 하얀 화면 보는 시간이 길어지므로 View 함수 리턴이 빨리 처리되야하는 것이 매우 중요합니다.

4. 위처럼 HTTP UI 응답이 낮아지는 경우로는 과도한 CSS, JS 로딩 및 연산, 잦은 시각적 개체 업데이트등이 있습니다.

   1. UI 응답성을 높이기 위해선 아래와 같은 방법을 적용합니다.

      1.  CSS, JS 파일은 minify시켜 다운 용량을 줄입니다.
      2. CSS를 HTML 콘텐츠보다 앞에 위치, JS를 HTML 콘텐츠보다 뒤에 위치시키기

      ## 

## Layout

공통으로 사용되어질 기능 및 항목에 대한 html을 따로 파일로 분리시켜 부모(상위)의 역할을 부여하고 그외 각 기능 및 페이지별 내용들에 대한 html을 자식(하위) 역할로 지정하여 사용합니다. 부모가 정의한 layout은 자식 페이지내에서 변경할 수 없으며 자식의 html에 `block영역 외에 다른 코드를 작성한 부분은 전부 무시`하게 됩니다.

해당 코드는 다음과 같습니다.

```html
<!-- 부모 역할의 layout.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>{% block title %}{% endblock title %}</title> <!-- 하위 html에서 title을 재정의한 것을 사용-->
    <!-- CSS only -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootswatch/3.4.1/darkly/bootstrap.min.css"
          integrity="sha384-pKJMCXwCXq3HwRBt27cwwSmc0/DAo2BjRxGd7nEESEStk++p6LffHmhX9oqzVDUk" crossorigin="anonymous">
    <script src="http://code.jquery.com/jquery-2.2.4.js" integrity="sha256-iT6Q9iMJYuQiMWNd9lDyBUStIq/8PuOW33aOqmvFpqI="
            crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"
            integrity="sha384-B4gt1jrGC7Jh4AgTPSdUtOBvfO8shuf57BaghqFfPlYxofvL8/KUEfYiJOMMV+rV"
            crossorigin="anonymous"></script>

</head>
<body>
<!--layout을 부모로 만드는 의미. 하위 html에서 extends를 사용해 block 내에 정의한 코드를 해당 영역에 표시-->
{% block content %}
{% endblock content %}
</body>
</html>
```

```html
<!--상속받기 post_list.html-->
{% extends "instagram/layout.html" %}
{% load bootstrap4 %}

<!--title 재정의-->
{% block title %}
    Instagram / Post List
{% endblock title %}

{% block content %}
    <table class="table table-bordered table-hover">
        <form action="" method="get">
            <input type="text" name="q" value="{{ q }}">
            <input type="submit">
        </form>
        {% for post in post_list %}
            <tr>
                <td>
                    {{ post.id }}
                </td>
                <td>
                    {% if post.post_img %}
                        <img src="{{ post.post_img.url }}" style="width: 100px"/>
                    {% else %}
                        <p>empty image</p>
                    {% endif %}
                </td>
                <td>
                    {#                <a href="/instagram/{{ post.id }}">#}
                    {#                <a href="{% url 'instagram:post_detail' post.pk %}">#}
                    <a href="{{ post.get_absolute_url }}">
                        {{ post.message }}
                    </a>
                </td>
            </tr>
        {% endfor %}
    </table>
    {{ page_obj }}
    {% if is_paginated %}
        {% bootstrap_pagination page_obj size="small" justify_content="center" %}
    {% endif %}
{% endblock content %}
```

## 개발서버 옵션

`python manage.py runserver` 를 이용해 서버를 실행시켰을때 bind 되는 ip는 default로 127.0.0.1로 생성됩니다. 이는 서버를 실행시킨 컴퓨터에서만 접속이 가능하지만 다른 컴퓨터에서도 접속이 가능하도록 서버를 실행시킬때는 `python manage.py runserver 0.0.0.0:8000` 으로 실행하면 가능합니다. 이때 0.0.0.0은 모든 것을 허용한다는 의미이고 외부망에서 접속하고자 할 경우는 외부 네트워크 설정이 추가적으로 필요합니다.

### 같은 네트워크가 아닌 휴대폰으로 내부망 개발서버에 접속하는 방법

- 휴대폰을 같은 네트워크 설정하기
- 개발서버에 외부망 연결하기 **(순수히 개발, 테스트 목적이므로 실서비스에 적용 X)**
  - 공인IP 할당
  - IP 공유기에 포트 포워딩, DMZ 세팅
  - ngrok(일종의 proxy서버와 같은 역할), localtunnel 사용

### ngrok의 사용

- 해당 디렉토리내에서 `./ngrok http 연결 포트번호`

![스크린샷 2021-01-02 오전 1 02 14](https://user-images.githubusercontent.com/48043799/103442073-3bfa5900-4c96-11eb-911e-6a7d59bdc91c.png)

- forwarding 주소를 `settings.py` 에 ALLOWED_HOSTS에 추가 
- 이후 forwarding에 표시된 주소로 접속



## scale을 x1.0 고정으로, 유저의 배율 조정을 막겠다는 선언

```html
<meta name="viewport" content="width=device-width,initial-scale=1.0,minimum-scale=1.0,maximum- scale=1.0,user-scalable=no">
```



## Static 파일

개발리소스로 앱/ 프로젝트 단위로 서빙하는 정적인 파일을 말합니다. (js, css, html)

장고는 1개의 프로젝트에 여러가지 앱의 구조를 가지고 있으며 한 앱을 위한 static 파일 경로는`app/static/app` 으로  지정합니다. 

단 프로젝트 전체에 사용되는 static파일은 settings.STATICFILES_DIRS에 지정된 경로로 사용하며 다수 디렉토리에 저장된 static파일은 `collectstatic` 명령을 통해 경로를 모아서 사용합니다.

### settings 예시

- STATIC_URL: 각 static 파일의 URL prefix
  - 템플릿 태그 {% static '경로' %}에 참조되는 설정 
  - `/` 로 끝나는 문자열 설정 
- STATICFILES_DIRS
  - File System Loader에 의해 참조되는 설정
- STATIC_ROOT
  - python manage.py collectstatic 명령에 참조되는 설정
  - 여러 디렉토리로 나뉜 static 파일들을 이 경로의 디렉토리로 복사하여 서빙,배포할때만 적용되는 설정

### 추천 settings

```python
STATIC_URL = '/static/'
STATIC_ROOT = os.path.join(BASE_DIR, 'staticfiles')
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, '프로젝트명', 'static')
]
```



## Static Files Finders

Template Loader와 비슷한 기능을하며 설정된 Finders를 통해 static 템플릿에 있을 디렉토리 목록을 구성하여 목록에 있는 지정 상대경로를 가지는 static 파일을 찾는 기능을 합니다. (참고 -> INSTALLED_APP => 'django.contrib.staticfiles')



## template static URL 처리

### settings.STATIC_URL 사용하기

`<img src="/static/blog/title.png/>"` 라는 이미지태그가 있을 때 settings.py 파일내에 `STATIC_URL = ""` 항목으로 지정하여 사용할 수 있습니다.

```
{% load static %}

<img src= "{% static "blog/title.png" %}"/> #사용
```

이는 개발환경에서만 사용하며 (DEBUG=True) 개발환경이 아닌 경우는 별도로 static 서빙 설정을 해줘야 합니다. 지정 이름의 Static 파일을 장고의 StaticFilesFinder에서 찾아서 내용을 읽고 응답하는 형식입니다.



## static 파일 서빙하는 방법

- 클라우드 정적 스토리지(aws-s3), CDN 서비스 활용
- apache/nginx 웹서버
- 장고를 통한 서빙(whitenoise 라이브러리)



## 외부 웹서버를 이용한 static/media 서비스

정적 콘텐츠는 외부 웹서버를 통해 처리시 효율적이며 정적 콘텐츠만의 최적화가 필요(`memcache` or `redis cache` )

![스크린샷 2021-01-03 오후 2 33 38](https://user-images.githubusercontent.com/48043799/103472428-ca72f580-4dd0-11eb-8ef4-a64f702436b2.png)

> nginx에서 static, media 파일을 관리 location이 /static일 경우 staticfiles 접근(settings.STATIC_ROOT)
>
> location이 media일 경우 /media  media 접근 (settings.MEDIA_ROOT)



## 배포시 static 처리(관련 라이브러리: django-storages)

1. 서비스용 settings에 배포 static 설정
2. 관련 클라우드 스토리지 설정 및 nginx/apache static 설정
3. 개발 완료된 static 파일을 한 디렉토리에 복사
   1. `python manage.py collectstatic --settings="서비스 settings"`
   2. STATIC_ROOT  경로로 복사됨
4. settings.STATIC_ROOT 경로에 복사된 파일들을 배포서버 복사(s3, apache&nginx에서 참조할 경로)
5. static 웹서버를 가리키도록 static_url 수정



## Reference

- https://educast.com/course/web/ZU53/

  

