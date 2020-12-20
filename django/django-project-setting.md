## Django & React Project Setting

Backend & Frontend 분리

### Backend

1. 가상환경 [anaconda](https://www.anaconda.com/products/individual#Downloads) or [virtualenv](https://live-jh.github.io/posts/dev/django_project_setting/)중 택 1
   1. anaconda 설치시 명령어`conda list`
   2. `conda create --name 프로젝트명 python=파이썬 버전` -> 아나콘다 가상환경 생성
   3. `conda activate 프로젝트명` 가상환경 활성화
   4. `conda deactivate` 가상환경 비활성화
2. DB 정의 후 Django model 선언
3. Serializers 설정
4. `python manage.py makemigrations`
5. `python manage.py migrate`
6. serializers 설정 (class)
7. settings 설정 (installed_apps, debug_toolbar, 개발환경 구성 분리, db설정,  CORS 설정, timezone, root_url, static_url등 수정)



### frontend

1. frontend 디렉토리 생성 후 이동 `mkdir frontend`, `cd frontend/`
2. ` npm install -g create-react-app # 글로벌로 cra 설치`
3. `create-react-app '앱이름'`
4. `cd 앱이름`
5. `npm start`







# 장고 프로젝트 세팅



## 1. 프로젝트 생성

1. 프로젝트 생성 
   1. 생성 후 프로젝트명 config로 변경하기
      1. urls -> root_urls 변경 후 url 각 앱별로 분기하기
   2. settings 패키지 생성(settings -> local, development 구분)
      1. local.py setting 값 변경
         1. root_urlconf, static_url, installed_apps, templates 설정 등등
   3. config app에 common 폴더 생성(secret key 담기)
   4. tempplates에 각 페이지별 html 담기
      1. static -> css, js, api, plugin, assets등등
   5. templatetags 패키지 생성
      1. common_tags(html dom 운용)
2. 각 기능별 앱 생성
   1. `python manage.py startapp account`
3. 프로젝트 구조 나누기
   1. 각 앱별로 views, urls, models, apps 나누기
   2. views에 list, get, post, put, delete, 등 선언
4. secret key 설정
5. settings 파일(local) 설정 변경하기
   1. installed_apps 에 생성한 앱 등록
   2. language_code = ko-kr
   3. time_zone asia/seoul
   4. templates dirs 설정
   5. root_urlconf 설정
6. manage.py와 wsgi.py에  setdefault 설정 local.py dir
   1. `os.environ.setdefault()`
7. requirements.txt 작성
   1. `pip freeze > requirements.txt`
8. model 작성
9. `python manage.py makemigrations`
10. `python manage.py migrate`



base

- development

- production 

## References

- 