# Title (주제)

## part.1(분류 1)
`description`

`장고로 개발중이다. 기본 User는 `username`을 Id로 사용한다.`

## part-2 (분류 2)
### Subject-1 (소주제)
```python
# example code

from django.db import models
from django.contrib.auth.models import (
    BaseUserManager, AbstractBaseUser,
    PermissionsMixin)


class UserManager(BaseUserManager):
    def create_user(self, email, password=None):
     

```
<br>

## References
- 