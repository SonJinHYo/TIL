# 앱 생성하기


참고로 장고에게 있는 models.Model 을 그대로 상속받아도 되지만

이미 장고 자체에서 커스텀 모델링을 강력추천하기 때문에 커스텀 모델링을 하자.



**참고로 장고 홈페이지에서 따라하라는 부분은 추가설명 없이 그대로 쓴다.**



1. `python manage.py startapp MyApp`

   - MyApp이라는 앱을 추가한다.

   

2. MyApp폴더의 models파일의 맨 위에 `from django.contrib.auth.models import ` 추가

   

3.  클래스를 추가한다.

   ```python
   from django.db import models
   from django.contrib.auth.models import
   
   class MyApp(AbstractUser):
       pass
   ```

   - 보이는 것처럼 AbstractUser을 상속받는다.

   - 물론 상속 받은걸 냅다 다 쓰면 커스텀이 아니지만 생성하는 부분만 적는거니까 pass로 마무리

     

4.  커스텀하기 전에 해당 앱폴더의 admin 파일로 이동해서 다음 코드 추가

   ```py
   from django.contrib import admin
   from django.contrib.auth.admin import UserAdmin
   from .models import  MyApp
   
   @admin.register(MyApp)
   class AdminMyApp(UserAdmin):
       pass
   ```

   - 2번줄은 장고가 시킨것
   - 그리고 다음 줄에 앱을 import 해준다.
   - 그리고 데코레이터로 등록

   

5.  migration을 해주자.

   - `python manage.py makemigrations` 터미널 창에 입력

   

6. migrate

   - `python manage.py migrate` 터미널 창에 입력

   
