django flow



# 환경

1. `poetry init` : 가상환경 설정. `pyproject.toml`, `poetry.lock `파일을 생성하여 라이브러리 관린
2. `poetry shell ` 
3. `django-admin startproject <project name> <path>`  : ex)`django-admin startproject config .` - 현 폴더에 `config` 관리폴더와 `manage.py `생성
4. `command pallette` 를 통해 `gitignore` 생성

## 주의사항

- 인터프리터 설정 해줄것
- AWS EC2의 poetry는 최신 버전에서 좀 멀고 업데이트가 까다롭다. AWS를 통해 시작하려면 처음부터 AWS 환경에서 시작할 것

## `django-admin` 주 명령어

`django-admin`은 `django`자체를 관리하기 위한 커맨드 라인

- `help` : 전체 도움말. 뒤에 다른 커맨드를 붙여 쓰면 해당 커맨드에 대한 옵션 설명이 나온다. 
- `version` : 버전 확인
- `sendtestemail <email address>`  :  django를 통한 이메일 전송이 잘 이루어지는지 확인하는 테스트 메일전송



## `python manage.py` 주 명령어

- `makemigrations`: DB컬럼에서 필요한 변경사항을 체크하여 migration 파일을 만든다
- `migrate` : migration 파일을 읽어 변경사항을 DB에 적용한다
- `createsuperuser` : 관리자 계정 생성
- `startapp <app name>` : `app name`의 이름을 가지는 app폴더 생성
  - 이후 `settings.py`의 `INSTALLED_APPS`에 app폴더/apps.py의 첫 클래스 이름을 추가
- `shell` : 터미널에 django를 구성. (settings.py를 가져온다)





# APP

## models.py

```python
from django.db import models

class House(models.Model):
    """ models.Model을 상속받는 기본 모델. """
    
    # field 설정 라인
    name = models.CharField(max_length=180, default="")
    country = models.CharField(
        max_length=50,
        default="대한민국",
    )
    city = models.CharField(
        max_length=80,
        default="경주",
    )
    price = models.PositiveIntegerField()
    rooms = models.PositiveIntegerField()
    host = models.ForeginKey("users.User",on_delete=models.CASCADE)
    
    def __str__(self):
        # 해당 객체의 이름 표현
        return self.name
```

### 상용 Field(essential_param=) https://docs.djangoproject.com/ko/4.1/ref/models/fields/#field-types

- `CharField(max_length=)` : 짧은 크기의 문자열 입력란
- `PositiveIntegerField()` 
- `TextField()` : 많은 양의 텍스트를 쓸 수 있는 입력란
- `BooleanFiled(default=)`
- `DateField()`,`DateTimeField()` : 날짜/날짜와시간 입력
  - argument로 `auto_now=True` / `auto_now_add=True`를 주로 사용 : object가 **최초 저장시에만/저장될 때마다** 필드값 갱신.
- `EmailField()` : 이메일
- `ImageField,URLfield,...`
- `ManyToManyField("object")` : 두 테이블간의 다대다 관계를 관리해주는 중간테이블 생성.
  - 특징
    - `_set`을 이용한 역참조가 가능하다. `room3.user_set.all()` : room3를 예약한 모든 사람.
      - 역참조 이름을 설정해줄수 있다. `ManyToManyField(..., related_name = 'users')`. (user_set이 아닌 users로 역참조 가능)
    - 데이터 추가/쿼리가 양쪽에서 가능. (위에선 House에만 필드를 추가했지만 users 모델에서 House를 부를 수 있게 된다)



#### Field Parameter(default value) https://docs.djangoproject.com/ko/4.1/ref/models/fields/

- `null=False`  : True시 빈 값은 모두 null 값이 된다. (null 값을 허용한다.)

- `blank=False` :  빈 칸을 허용한다.(필수적이지 않도록 한다.)

- `default=` : default값 설정

- `editable=True` : False 일 때, 수정이 불가능하면 관리 패널에서 사라짐

- `help_text=` : 해당 필드의 부가설명을 추가한다.

- `verbose_name=` : 설정한 이름으로 필드 이름을 설정. default로는 django가 자동으로 필드 이름에 맞게 생성

- `choices=<ChoicesClass>`  : 보기 중 고를 수 있도록 설정

  - ```python
    class User(AbstractUser):
    	class GenderChoices(models.TextChoices):
            # 튜플의 첫 원소는 db의 value, 두 번쨰는 관리자페이지의 label
            MALE = ("male","Male")
            FEMALE = ("female","Female")
    
            gender = models.CharField(max_length=10,choices = GenderChoices)
    ```

#### ForeginKey(object, on_delete=)

- 해당 모델의 id를 부여.
- object에는 model class 위치를 넣는다.
  - 단, 커스텀 유저 모델을 넣을 시, `"users.User"` 보다는 `settings`를 불러와서 `settings.AUTH_USER_MODEL` 이 권장된다.
    1. 유저 모델이 바뀔 수 있으니까
    2. `settings `불러오기 : `from django.contrib import settings`
- on_delete : id 객체 삭제시 데이터 처리
  - `on_delete = models.CASCADE` : id 객체 삭제 시 해당 모델 객체도 삭제. (ex. 유저 삭제 시 프로필 사진)
  - `on_delete = models.SET_NULL ` : id 객체 삭제 시 객체를 null로 대체 (ex. 유저 삭제 시 결제 내역)



#### 주의사항

- 모든 model은 `pk`(Primary Key)를 **선언하지 않아도 가지고 있다.** column type은 `Foregin Key`이므로 **정수와 착각X**
- 기존 db가 존재하는 상태에서 새로운 Field를 추가할 시, 터미널에 다음 에러가 나타날 수 있다.
  - It is impossible to add a  non-nullable field 'field_name' to user without specifying a default. This is because the database needs something to populate existing rows. Please select a fix: (기존 값에 이 필드를 추가할 시 어떻게 추가할 것인가. 이 문구는 booleanField는 null값을 기본으로 할 수 없다는 뜻)
    1. Provide a one-off default now (will be set on all existing rows with a null value for this columns) (일회성 값을 추가한다. null값으로 돌아가게 된다.)
    2. Quit and manually define a default value in models.py (멈추고 models.py에서 deefault값을 설정한다)



### Custom User Model

django에서 기본 모델을 제공. 단, 커스터마이징을 적극 권장. **반드시 프로젝트 초반에 할 것**

#### User Model Customizing https://docs.djangoproject.com/ko/4.1/topics/auth/customizing/#extending-the-existing-user-model

1. `python manage.py startapp users` : 새로운 User App 생성

2. users/models.py 로 가서,

   ```python
   from django.contrib.auth.models import AbstractUser
   
   class User(AbstractUser):
       """
       사용법 : AbstactUser로 가서 필요한 property를 그대로 가져온다.
       """
       pass
   
   	## 클래스 복수형 표현 수정 (or 다른 이름)
       class Meta:
           verbose_name_plural = "복수형/다른 이름"
   ```

3. `settings.py ` 에 다음 내용 추가

   ```python
   .
   .
   # users app 등록
   INSTALLED_APPS = [...,
                     users.apps.UserConfig,]
   .
   .
   # 맨 끝에 커스텀 유저모델 선언
   # Auth
   AUTH_USER_MODEL = "users.User" # User 클래스 위치
   ```

4. `python manage.py makemigrations` , `python manage.py migrate` 실행 : DB에 등록

5. `users/admin.py` 에서 관리자 클래스 추가 : 다른 앱과는 다르게 상속받은 클래스를 관리하므로 차이가 있다.

   ```python
   from django.contrib.auth.admin import UserAdmin
   from .models import User
   
   @admin.register(User)
   class CustomUserAdmin(UserAdmin):
       """
       사용법 : UserAdmin으로 가서 필요한 property를 그대로 가져온다. https://docs.djangoproject.com/en/4.1/ref/contrib/admin
       """
       # fieldsets : model의 field가 보이는 순서 설정. 
       fieldsets = (
           (
               "Profile", # 이름대신 None을 선언하면 큰 Section이 없어짐
               {
                   "fields":("username","password","email",), # User properties
                   "classes": ( # 문서 참조
                       "collapse", # 숨김 버튼 제공
                       "wide", # 더 넓게 펴기
                   )
               }
           )
       )
       
       #list_display : 사용자 list를 표시할 때 보이는 column 설정
       list_display = ("username","email","gender",)
   ```



### Common Model

- 많은 모델이 공통필드를 가질 때 선언해두면 편하다.

- ```python
  # common app 선언 후 common models.py
  from django.db import models
  
  class CommonModel(models.Model):
      created_at = models.DateTimeField(auto_now_add = True)
  	updated_at = models.DateTimeField(auto_now = True)
      
      # DB에 추가하지 않는 추상클래스임을 선언
      class Meta:
          abstact = True
          
  ```

- 이후 CommonModel을 상속

## admin.py

### 해당 앱 관리자 등록 https://docs.djangoproject.com/ko/4.1/ref/contrib/admin/

```python
from django.contrib import admin
from .models import House

@admin.register(House) # House class를 관리하는 class
class HouseAdmin(admin.ModelAdmin):
    list_display = ("name","price","rooms",)
    list_filter = ("price","city",)
    
    def some_method(self,house):
        # 두번째 원소로 house를 넣으면 House 모델의 각 객체를 room으로 받게 된다.
        pass
    
    
```

- `list_display = (model의 properties)` : 관리창에 관리모델의 미리보기를 입력에 맞게 보여준다.
- `list_filter = (model의 properties)` : 관리창 측면에 필터 생성. 입력에 맞게 필터를 생성한다.
  - property를 탐색할 때 순서
    - admin class 내부 property->  관리 model property -> 관리 모델 method
- `search_fields = (model의 properties)` : 검색창 생성
  - ex) `search_fields = (address,price)` : 검색창에 입력시 address와 price를 기준으로 입력값이 **포함된**  모든 것을 검색
  - properties 뒤에 두 개의 언더바를 붙여서 옵션 설정 가능
    - `search_fields = (address_starts/ends/contains/with)` : 검색한 텍스트로 시작하는/끝나는/포함하는 address만 검색.
      - 위 조건들에 앞에 i가 붙으면 대소문자 구별X ( ex. istartswith )
    - `<field_name>__lt/lte/gt/gte = val` : field_name  `<`  / `<=` / `>` / `>=` val
    - `<field_name>__in = val_list` : val_list 내의 객체들 반환
- `exclude = (model의 properties)` : 해당 property 제외

- `fields = (model의 properties,(model의 properties),)` : 튜플로 묶어진 property는 한 칸에 같이 표현







## apps.py

### 패널상의 앱 이름 변경

apps.py에서

```python
class <ClassNameConfig>(AppConfig):
    ...
    # verbose_name 추가
    verbose_name = "앱 이름"
```



