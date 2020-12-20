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



## References

- 