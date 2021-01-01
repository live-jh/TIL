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



## Reference

- https://educast.com/course/web/ZU53/

  

