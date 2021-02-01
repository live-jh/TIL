## Coding Style

### PEP 8

파이썬 공식 스타일 가이드

- 들여쓰기 스페이스 4칸
- 최상위 함수와 클래스 선언 사이 구분은 두 줄 띄우기
- 클래스 내에 메서드들을 나누기 위해 한줄 띄우기
- 한줄당 텍스트 79글자 넘지 않기 (오픈 소스 프로젝트시 필수)

### Import

import 선언시 그룹의 순서 지정

1. 표준 라이브러리 import
2. 연관 외부 라이브러리 import
3. 로컬 애플리케이션(프로젝트 앱) or 라이브러리에 한정된 import

```python
# 표준 라이브러리
from math import sqrt
from os.path import abspath
# 코어 장고 
from django.db import models
from django.utils.translation import ugettext_lazy as _
# 서드 파티 앱
from django_extensions.db.models import TimeStampedModel
# 프로젝트 앱, 명시적 상대 import
from .models import Post
```

### import * 피하기

각각 다른 파이썬 모듈의 공간들이 작업하는 모듈에 추가로 로딩되거나 등록되어 기존 것에 덮여 로딩되는 경우를 막기 위함

### url 패턴시 "-" 대신 "_"

파이썬 다운 "_" 를 사용하자! 웹의 URL주소 패턴이 아니라 `urls.py`에서 사용되는 name을 의미

<br>

## References

- [Two Scoops of Django](https://g.co/kgs/3Bpx8R)

