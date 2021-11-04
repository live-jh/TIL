## Django & React Project Setting

> 장고 프로젝트 구성을 연습하며 정리한 내용 :)

### Backend

1. 장고 프로젝트 생성
  
   1. pycharm으로 new project시 아래 `2번 과정 생략`
   
2. 가상환경 [anaconda](https://www.anaconda.com/products/individual#Downloads) or [virtualenv](https://live-jh.github.io/posts/dev/django_project_setting/)중 택 1 (anaconda version)
   1. anaconda 설치시 명령어 확인 `$ conda list`
   2. `$ conda create --name 프로젝트명 python=파이썬 버전` -> 아나콘다 가상환경 생성
   3. `$ conda activate 프로젝트명` 가상환경 활성화하기
      4. `$ conda deactivate` 가상환경 비활성화
   
3. 실행하려는 명령 커맨드의 위치에서 가상환경 및 명령어 확인
   1. `$ which python` (디렉토리 위치 확인)
   2. `$ which pip` (디렉토리 위치 확인)
   
4. 프로젝트 설정
   2. `$ which django-admin` (디렉토리 위치 확인)
   3. `$ django-admin startproject 프로젝트명 .`
      1. `$python -m django startproject 프로젝트명 ` 과 동일
      2. pycharm으로 new project시 명령어 생략
   3. `$ pip install django~=버전(ex: 3.1.1)` 장고 설치 
   4. `$ python manage.py migrate`
   5. `$ python manage.py createsuperuser` superuser 생성
   6. 프로젝트 라이브러리 지정 (개발&배포 환경 분리)
      1. requirements/common, requirements/dev, requirements/prod
         1. -r 읽어들일 파일명.파일형식 (-r common.txt)
   7. `$ pip list --format=freeze > requirements.txt`
      1. requirements 생성(conda는 명령어로 list --format을 추가하기)
      2. virtualenv 가상환경의 경우 `$ pip freeze > requirements.txt`

5. settings.py 설정(dev, prod 분리)
   1. 앱 - config
      1. settings, common, static 등등
      2. common에서 key 관리 (scret_key, aws_key, DB info) 
   2. settings 개발 환경 분리(settings 폴더 생성)
      1. settings/base.py, settings/dev.py, settings/prod.py
         1. `import *` (안티패턴이지만 settings에서만 예외 적용)
      2. `asgi.py, wsgi.py -> os.environ.setdefault: prod` product 설정
      3. `manage.py -> os.environ.setdefault: dev`
   3. settings.py static 설정 추가
      1. `STATIC_URL = '/static/'`
         1. 웹 페이지에서 사용할 정적 파일의 최상위 URL 경로로 경로 자체는 실제 파일이나 디렉토리가 아니며 순수 URL로만 존재하는 단위입니다. 사용자가 임의로 변경이 가능하며 반드시 문자열로 구성되어 /로 끝나야 합니다.
      2. `STATIC_ROOT = os.path.join(BASE_DIR, 'static')`
         1. Django 프로젝트에서 사용하는 모든 정적 파일을 한 곳에 모아넣는 경로
         2. 지정한 디렉토리로 모으는 명령은 `manage.py` 파일의 `collectstatic` 명령어로 수행
            1. [collectstatic 설명](https://github.com/live-jh/TIL/blob/master/django/django-frontend-base.md) 참고
      3. `STATICFILES_DIRS = [os.path.join(BASE_DIR, '프로젝트이름', 'static')]`
         1.  개발 단계에서 사용하는 정적 파일이 위치한 경로들을 지정하는 것으로 static 폴더가 프로젝트 바로 아래 경로에 있다면 프로젝트 이름은 생략 가능
      4. `MEDIA_URL = '/media/'`
         1. 파일의 url 접근시 사용되는 설정
         2. STATIC_URL과 비슷한 기능
      5. `MEDIA_ROOT = os.path.join(BASE_DIR, 'media')`
         1. 파일 업로드시 사용되는 설정 (업로드 후 파일 배치할 최상위 경로 지정)
         2. STATIC_ROOT와 다른 경로로 지정해야 합니다.
   4. TEMPLATES 설정
      1. templates 폴더 이동시 변경
      2. `'DIRS': [os.path.join(BASE_DIR, 'config', 'templates')]`
         1. templates는 변경 불가
   
6. 프로젝트 app 생성 

   1. `python manage.py startapp 앱이름` (유저)
      1. directory settings
         1. `$ mkdir 앱이름`
         2. `$ python manage.py startapp 앱이름 ./backend/앱이름`
   2. urls.py 생성 및 urlpatterns 추가
   3. settings에 installed_apps 앱 추가
   4. DB 정의 후 Django model 선언
   5. Serializers 설정
   6. `$ python manage.py makemigrations`
   7. `$ python manage.py migrate`
      1. 쿼리 보기 -> `$ python manage.py sqlmigrate 앱명 마이그레이트 파일명`

7. urls 개발 debug 및 static 분기
   1. setting.DEBUG True일때 path url에 debug_toolbar.urls 및 static url 패턴 추가
      1. `$ pip install django-debug-toolbar` 설치
         1. [debug-toolbar-setting-guide](https://django-debug-toolbar.readthedocs.io/en/latest/installation.html#setting-up-urlconf)
      2. static -> `static(media_url='' , document_root='')` 설정

     



![스크린샷 2021-01-17 오후 5 24 41](https://user-images.githubusercontent.com/48043799/104835323-236e7d80-58e9-11eb-8dcc-f5082b6f8720.png)

> Project Setting

### frontend 추가시

1. frontend 디렉토리 생성 후 이동 `$ mkdir frontend`, `$ cd frontend/`
2. `$  npm install -g create-react-app # 글로벌로 cra 설치`
   1. `$ yarn create-react-app 폴더명`
3. `$ cd 앱이름`
4. `$ npm start or yarn start` 



## django project 명령어

```
# 장고 프로젝트 생성
$ django-admin startproject 프로젝트명

# db 마이그레이션 파일 생성
$ python manage.py makemigrations (appName)

# db 마이그레이션 적용
$ python manage.py migrate (appName)

# 사용자 유저 객체 관리를 위한 슈퍼유저 생성 (admin에서 사용 가능)
$ python manage.py createsuperuser

#서버 시작 명령
$ python manage.py runserver

#장고 앱 생성
$ python manage.py startapp 앱이름

#개발환경 변화에 따라 대처하기 위해 현 프로젝트의 패키지를 기록하는 명령어
$ pip freeze > requirements.txt

#requirements 파일에 있는 패키지 설치 (프로젝트는 왠만하면 깨끗한 상태에서 실행)
$ pip install -r requirements.txt
```





## Ver.2

### prod에선 conda보단 pip로 사용(가상환경)

1. 프로젝트 설정 (이름은 _ 불가, 카멜 or - 규칙 사용하기)

2. anaconda python 3.8 (virtual environments 가상환경)

3. 앱 이름 -> config로 변경

4. 1. settings 또는 프로젝트 앱이름 사용된 곳 다 config로 변경

5. common/env_file 붙여넣기

6. settings 수정

7. 1. language code

   2. timezone

   3. env_json = join(BASE_DIR, 'config/common/env.json')

   4. add with open(env_file)

   5. SECRET_KEY 설정

   6. 1. get_secret(key)

   7. DATABASES

8. add .gitignore 

9. default migrate

10. create superuser

11. admin, localhost 8000 잘 뜨는지 확인

12. requirements.txt 생성

13. 1. Django~=version

14. add static_url, root in settings 

15. 1. STATIC_URL, STATIC_ROOT
    2. MEDIA_URL, MEDIA_ROOT

16. modified urls.py 

17. 1. static root 서빙 설정

18. requirements 디렉토리 구분

19. 1. common, dev, prod
    2. django-debug-toolbar 설치

20. urlpatterns debug 추가

21. settings 구분

22. 1. common, dev, prod

    2. 1. dev -> installed_apps += django_debug_toolbar, MIDDLEWARE += debugtoolbar
       2. INTERNAL_IPS = ’127.0.0.1’ 추가

    3. wsgi -> 서비스의 진입점 config.settings.prod

    4. asgi -> 

    5. manage -> 개발 환경의 진입점 config.settings.dev

    6. 환경설정 -> config.settings.dev

    7. BASE_DIR depth 하나 더 추가

23. frontend 사용 안하는거 제거

24. 1. App.js

    2. App.test.js

    3. app.scss (변경)

    4. 1. CRA 로 생성된 리액트는 4버전 이후와 충돌남(아래와 같이 설정)
       2. $ yarn add node-sass@4.14.0

    5. index.scss (변경)

    6. index.js

    7. setupTests 

    8. 외 다 제거

    9. 지운 목록들 다 확인해서 코드도 제거

## Reference

- https://blog.hannal.com/2015/04/start_with_django_webframework_06/
- https://educast.com/course/web/ZU53/

