django 정리 및 코드템플릿 (메인 참고자료)



# 환경 (poetry)

1. `poetry init` : 가상환경 설정. `pyproject.toml`, `poetry.lock `파일을 생성하여 라이브러리 관린
2. `poetry shell ` 
3. `django-admin startproject <project name> <path>`  : ex)`django-admin startproject config .` - 현 폴더에 `config` 관리폴더와 `manage.py `생성
4. `command pallette` 를 통해 `gitignore` 생성

## 주의사항

- 인터프리터 설정 해줄것
- AWS EC2의 poetry는 최신 버전에서 좀 멀고 업데이트가 까다롭다. AWS를 통해 시작하려면 처음부터 AWS 환경에서 시작할 것
- poetry를 인식하지 못하는 경우

  - 'poetry' 용어가 cmdlet, 함수, 스크립트 파일 또는 실행할 수 있는 프로그램 이름으로 인식되지 않습니다. (아래 순서로 해결)

    1. 시스템 속성에 들어간다.

    2. '고급'의 환경 변수에 들어간다

    3. Path에 들어가서 poetry가 들어가있는 경로를 추가한다.
       - ('C:\Users\사용자명\AppData\Roaming\Python\Scripts')

    4. 재부팅
- 인식해도 찾지를 못한다 `~~지정된 모듈을 찾을 수 없습니다` : IDE를 관리자 권한으로 실행

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

### 주의사항

- `table already exists` : 이미 테이블이 생성한 후 `migrations `수정을 통해 다시 생성할 때 발생
  - `python manage.py migrate --fake <appname>` : 마이크레이션이 완료된 것처럼 실행

## settings.py

#### Korea 설정

- `LANGUAGE_CODE = "ko-kr"`
- `TIME_ZONE = "Asia/Seoul"`

#### 유저가 업로드한 media 파일 저장경로 설정 (개발 단계 한정 저장경로)

`FileField()`를 통해 받게된 media파일에 대해 설정

1. `settings.py` 내에 `MEDIA_ROOT = uploads` 추가  : `uploads` 폴더에 저장 (자동생성)

2. `settings.py` 내에 `MEDIA_URL = "user-uploads/"` 추가 : 저장된 파일의 url경로

   - **반드시 마지막에 slash 추가**

3. `config/urls.py` 에 다음과 같이 static 추가

   ```python
   from django.conf.url.static import static
   from django.conf import settings
   
   urlpatterns = [
       ...
   ] + static(settings.MEDIA_URL,document_root = settings.MEDIA_ROOT) # 추가 항목
   ```

**개발 단계 한정 이유** : 업로드 데이터의 문제 + 업로드 파일을 서버 디스크 공간에 할애

- django는 url 경로만 알기 때문에 업로드 데이터 확인 불가
- 서비스 단계 방식으로는 cloudflare을 이용한 이미지URL만을 받는 방식

#### env 폴더 설정

1. `settings.py`의 상위 폴더 위치에 `.env` 파일 생성
2. `.gitignore` 에 `.env` 추가
3. `poetry add django-environ`
4. `settings.py`에서 
   1. `import os`/ `import environ` 추가
   2. 가장 위쪽에 `env = environ.Env()` 추가
   3. `BASE_DIR` 아래줄에 `environ.Env.read_env(os.path.join(BASE_DIR,".env"))` 추가
   4. 사용예시ex. `SECRET_KEY = env("SECRET_KEY")` ( env(`.env`내의 변수명) )


##### 주의사항

- `.env` 폴더 내부에선 **띄어쓰기**를 하면 인식이 안된다.
  - `SECRET_KEY="asdasad"` (O)
  - `SECRET_KEY = "asdasd"` (X)


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

### 상용 Field(essential_param=) 

django docs : https://docs.djangoproject.com/ko/4.1/ref/models/fields/#field-types

- `CharField(max_length=)` : 짧은 크기의 문자열 입력란
- `PositiveIntegerField()` 
- `TextField()` : 많은 양의 텍스트를 쓸 수 있는 입력란
- `BooleanFiled(default=)`
- `DateField()`,`DateTimeField()` : 날짜/날짜와시간 입력
  - argument로 `auto_now=True` / `auto_now_add=True`를 주로 사용 : object가 **최초 저장시에만/저장될 때마다** 필드값 갱신.
- `EmailField()` : 이메일
- `ManyToManyField("object")` : 두 테이블간의 다대다 관계를 관리해주는 중간테이블 생성.
  - 특징
    - `_set`을 이용한 역참조가 가능하다. `room3.user_set.all()` : room3를 예약한 모든 사람.
      - 역참조 이름을 설정해줄수 있다. `ManyToManyField(..., related_name = 'users')`. (user_set이 아닌 users로 역참조 가능)

    - 데이터 추가/쿼리가 양쪽에서 가능. (위에선 House에만 필드를 추가했지만 users 모델에서 House를 부를 수 있게 된다)
- `ImageField,Filefield(upload_to=None)`:  파일 저장 필드. `upload_to=None`일 때 `settings.py` 내용으로 전환 (settings.py 참고)

##### `FileField, ImageField` 업로드 경로 커스텀

`upload_to` 파라미터에 함수를 넣으면 해당 결과를 따로 반환. **저장파일명, 입력받은 파일을 작업한 결과** 등을 저장하고 싶을 때 유용

- `instance` : model에서 생성된 instance 내용물. 즉, Field나 ForeignKey에 접근 가능

```python
def custom_upload_to(instance, filename):
    return 'user_{}/{}'.format(instance.name, filename)

class MyModel(models.Model):
    name = models.CharField(max_length=20, blank=True, null=True)
    upload = models.FileField(upload_to=custom_upload_to)
```





#### Field Parameter(default value) 

django docs : https://docs.djangoproject.com/ko/4.1/ref/models/fields/

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
- `related_name` : 역참조시 이름 명명
  - 관행적으로 모델명의 복수형으로 사용 
    - ex. 모델명(참조모델X)이 `Tweet` 이면 `related_name="tweets"`

  - 없을경우 자동으로 `모델명_set`의 역참조 쿼리 생성




#### 주의사항

- 모든 model은 `pk`(Primary Key)를 **선언하지 않아도 가지고 있다.** column type은 `Foregin Key`이므로 **정수와 착각X**
- 기존 db가 존재하는 상태에서 새로운 Field를 추가할 시, 터미널에 다음 에러가 나타날 수 있다.
  - It is impossible to add a  non-nullable field 'field_name' to user without specifying a default. This is because the database needs something to populate existing rows. Please select a fix: (기존 값에 이 필드를 추가할 시 어떻게 추가할 것인가. 이 문구는 booleanField는 null값을 기본으로 할 수 없다는 뜻)
    1. Provide a one-off default now (will be set on all existing rows with a null value for this columns) (일회성 값을 추가한다. null값으로 돌아가게 된다.)
    2. Quit and manually define a default value in models.py (멈추고 models.py에서 deefault값을 설정한다)



### Custom User Model

django에서 기본 모델을 제공. 단, 커스터마이징을 적극 권장. **반드시 프로젝트 초반에 할 것**

#### User Model Customizing

django docs : https://docs.djangoproject.com/ko/4.1/topics/auth/customizing/#extending-the-existing-user-model

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

### 해당 앱 관리자 등록

django docs : https://docs.djangoproject.com/ko/4.1/ref/contrib/admin/

```python
from django.contrib import admin
from .models import House

@admin.register(House) # House class를 관리하는 class
class HouseAdmin(admin.ModelAdmin):
    list_display = ("name","price","rooms","some_method")
    list_filter = ("price","city",)
    
    def some_method(self,house):
        # 두번째 원소로 house를 넣으면 House 모델의 각 객체를 house으로 받게 된다.
        pass
    
    ## ManyToMany필드 읽기 함수 (violations가 MTM필드 일 때)
	def name_list(self, obj):
        return ",".join([violation.name for violation in obj.violations.all()])
    
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



### Custom FIlter

```python
# 항상 looksup, queryset 2개의 함수를 가져야한다.
class WordFilter(admin.SimpleListFilter):

    title = "Filter by words!"

    parameter_name = "word"

    def lookups(self, request, model_admin):
        # 커스텀 필터는 튜플리스트를 반환값으로 가진다
        return [
            # 튜플의 원소 : (url에 나타나는 값, admin 패널에 보여지는 값)
            ("good", "Good"),
            ("great", "Great"),
            ("awesome", "Awesome"),
        ]

	# 필터링을 거친 값을 반환	
    def queryset(self, request, reviews):
        word = self.value()
        if word:
            return reviews.filter(payload__contains=word)
        else:
            reviews

@admin.register(Review)
class ReviewAdmin(admin.ModelAdmin):

    list_display = (
        "__str__",
        "payload",
    )
    
    list_filter = (
        WordFilter,
        "rating",
        "user__is_host",
    )
```





### Admin Actions

 ```python
  # 1. admin.py 파일 내에서 액션함수 선언
  
  @admin.actions(description = "패널에서 액션 설명 문구")
  def reset_prices(model_admin, request, queryset):
      """
      model_admin : 액션을 가진 admin모델
      request : 요청 정보 (user 정보 포함)
      queryset : 선택 요소. (room을 선택할 상황일 때, rooms로 이름을 지정하면 편함)
      """
  	# 가격 리셋
      for room in rooms.all():
          room.price = 0
          room.save()
            
            
@admin.register(Room)
class RoomAdmin(admin.ModelAdmin):
    
    # 2. action 추가
    actions = (reset_price,)
 ```

## apps.py

### 패널상의 앱 이름 변경

apps.py에서

```python
class <ClassNameConfig>(AppConfig):
    ...
    # verbose_name 추가
    verbose_name = "앱 이름"
```



## views.py / urls.py / serializers.py

참고 (DRF) : https://github.com/SonJinHYo/TIL/blob/main/django/drf.md

### 기본 함수

- `path("url",func)` : "url"을 요청받으면 func를 실행. `from django.urls`
- `include("앱url")` : 경로를 해당 앱의 urls에서 찾도록 이어주는 django 함수  . `from django.urls`
- `render(request,template_name,{key: value})` : 받은 request가 첫 인자, template가 두번째 인자, 딕셔너리 형태로 넘길 파라미터를 세번째 인자로 사용.
  -   `from django.shortcut`
  -   넘겨받은 파라미터는 key를 사용하고 value로 출력

```python
# config/ views.py 파일
from django.urls import path,include

urlpatterns = [
    path("admin/", admin.site.urls),
    path("api/v1/rooms/", include("rooms.urls")),
]
```

### apps/urls.py 추가

ex . `api/v1/rooms/1/photo` 경로를 추가할 때,

1. `config/urls.py`에 `path("api/v1/rooms/",include("rooms.urls"))` 추가 (rooms 앱일 경우)
2. `rooms`폴더에 `urls.py` 추가
3. `urlpatterns = [..., path("<int>/photo",function),]`
   - function은 대체로 views에서 가져온다. `from . import views`

###### `api/v1/` 명시 이유

- `admin`페이지와 같이 서버 내부의 url로 착각하기 쉽다.
- 버전 관리가 쉽다. 업데이트된 새로운 버전이 나온다면 `api/v2/rooms`를 추가해주면 된다.

```python
# rooms/ views.py 파일

from django.urls import path
from . import views  # DRF 참조

urlpatterns = [
	path("<int>/photo",views.RoomPhotos.as_view()),
]
```



### URL_Args

`rooms/<arg_type: arg_name>` :  URL에서 꺽쇠에 해당하는 위치의 문자는 arg_type으로 받아서 arg_name으로 사용. 

- arg는 함수의 인자로 받게된다. 
- arg가 여러 개라면 입력순으로 함수가 받는다.

- ex . `path("rooms/<int:room_id>", views.see_one_room`)  
  - 이 때 views.py의 see_one_room 함수 : `def see_one_room(request,room_id)`

#### 주의사항

str타입의 변수를 받을 때는 다른 url과 겹치지 않도록 아래로 내린다

```python
# 에러 발생 : home이 들어올 경우 'username'을 home으로 받는다
urlpatterns = [
	path("<str:username>",views.RoomPhotos.as_view()),
	path("home/photo",views.RoomPhotos.as_view()),
]

# 줄을 바꾸어 수정
urlpatterns = [
	path("home/photo",views.RoomPhotos.as_view()),
	path("<str:username>",views.RoomPhotos.as_view()),
]

# 인스타식 방법 (이름앞 특수문자)
urlpatterns = [
	path("@<str:username>",views.RoomPhotos.as_view()),
	path("home/photo",views.RoomPhotos.as_view()),
]

```



### templates

django는 render을 통해 template를 찾을 때, 자동으로 `templates`폴더를 찾아 그 내부의 파일을 탐색

django_docs : https://docs.djangoproject.com/ko/4.1/topics/templates/



## tests.py

`python manage.py test` : 모든 `test.py` 실행 및 결과 출력

DRF 테스트 클래스 사용 : `from rest_framework.test import APITestCase`

### APITestCase Functions

**거짓일 시 출력할 문구는 알아서 인자로 넣기**

- `assertEqual(a,b)/assertNotEqual(a,b)` : `a == b/ a=! b` 테스트. 
- `assertIsInstance(a,type)` : `a의 타입 == type` 테스트. 
- `assertIn(a,b)/assertNotIn(a,b)` : `a in b/ a not in b` 테스트.



### Rules

1. 테스트 클래스 내부의 테스트 함수는 `test_`로 시작한다. `def test_blabla(self):` (self는 APITestCase를 참조)

2. `python manage.py test`를 할 경우 테스트용 db가 생성후 삭제된다. **테스트에 사용될 default value를 작성할 것**
   - `def setUp(self):` 테스트 전에 실행되는 함수. default값 설정시 사용
3. 테스트 코드를 세부적으로 나누는 법 (프로젝트 크기가 커질 시)
   1. tests 폴더 생성
   2. `__init__.py` 생성 (django가 패키지로 인식)
   3. `test_filename.py` 생성 (**test가 반드시 앞에 붙어야 django가 인식**). 이후 tests.py와 동일하게 작성



### 코드 템플릿

- setUp, GET test

  ```python
  class TestAmenities(APITestCase):
      NAME = "Amenity Test"
      DESC = "Amenity Des"
      URL = "/api/v1/rooms/amenities/"
      UPDATE_NAME = "Update Amenity"
      UPDATE_DESC = "Update Dsc"
      
      def setUp(self): # Ruels 2.
          models.Amenity.objects.create(
              name=self.NAME,
              description=self.DESC,
          )
  
      def test_all_amenities(self):
          response = self.client.get(self.URL)
          data = response.json()
  
          self.assertEqual( # response 확인
              response.status_code,
              200,
              "Status code isn't 200.",
          )
          self.assertIsInstance( # type 확인
              data,
              list,
          )
          
          # 이하 오브젝트 데이터가 맞는지 확인
          self.assertEqual(
              len(data),
              1,
          )
          self.assertEqual(
              data[0]["name"],
              self.NAME,
          )
          self.assertEqual(
              data[0]["description"],
              self.DESC,
          )
  ```

  - `self.client` 사용법 : `response = self.client.get/post/put/delete("api주소")` 

- POST(생성) test (예시는 위 클래스와 이어서, **유저 생성이 아닌 모델 생성**)

  ```python
      def test_create_amenity(self):
  
          new_amenity_name = "New Amenity"
          new_amenity_description = "New Amenity desc."
  
          response = self.client.post(
              self.URL,
              data={
                  "name": new_amenity_name,
                  "description": new_amenity_description,
              },
          )
          data = response.json()
  
          self.assertEqual( # response 확인
              response.status_code,
              200,
              "Not 200 status code",
          )
          
          # 이하 데이터 확인
          self.assertEqual(
              data["name"],
              new_amenity_name,
          )
          self.assertEqual(
              data["description"],
              new_amenity_description,
          )
          
  		# 잘못된 요청 테스트
          response = self.client.post(self.URL)
          data = response.json()
  
          self.assertEqual(response.status_code, 400)
          self.assertIn("name", data)
  ```

  - response_test 에러시 `views.py`의 `Response(status=)` 설정 확인

- PUT test

  ```python
      def test_put_amenity(self):
          response = self.client.put( # put request
              "/api/v1/rooms/amenities/1",
              data={"name": self.UPDATE_NAME, "description": self.UPDATE_DESC},
          )
  		
          # 업데이트 데이터와 response 확인
          data = response.json()
          self.assertEqual(data["name"], self.UPDATE_NAME) 
          self.assertEqual(data["description"], self.UPDATE_DESC)
          self.assertEqual(response.status_code, 200)
  		
          # max_length 테스트
          name_len_200 = 'a' * 200
          name_validate_response = self.client.put(
              "/api/v1/rooms/amenities/1",
              data={"name": name_len_200},
          )
          
          data = name_validate_response.json()
          self.assertIn('name', data)
          self.assertNotIn('decs', data)
          self.assertEqual(name_validate_response.status_code, 400)
  ```

- DELETE test

  ```python
      def test_delete_amenity(self):
  
          response = self.client.delete("/api/v1/rooms/amenities/1")
  
          self.assertEqual(response.status_code, 204)
  ```

- Authentication test (ex. 유저가 방을 만드는 상황일 때)

  ```python
  class TestRooms(APITestCase):
      def setUp(self):
          user = User.objects.create(
              username="test",
          )
          user.set_password("123")
          user.save() # save는 테스트DB에 저장된다
          self.user = user
  
      def test_create_room(self):
  
          response = self.client.post("/api/v1/rooms/")
  
          self.assertEqual(response.status_code, 403) # 로그인 전 테스트
  		
          # 로그인 자체를 테스트
          self.client.login(
              username = "test",
              password = "123"
          )
          # 로그인 테스트가 아닌 다른 테스트를 위한 강제 로그인 상태로 만들기
          # 인증시스템을 통과하는지 체크할 때 사용 (로그인 여부에 따른 response 확인)
          self.client.force_login(
              self.user,
          )
  ```


### Fields 테스트데이터 생성

- DateTime Test

  - 필요 라이브러리 : `from django.utils import timezone` , `import datetime`
  - `created_ad = timezone.now()` 
  - `datetime.date/datetime.today()/now()` : 오늘의 date/now 등을 **마이크로초** 반환
  - `datetime.timedelta(week=,days=,...)` 입력받은 기간을 **마이크로초**로 반환

  

- ManyToMany Test

  ```python
      def create_violation_info_1(self):
          """
  		violations : m2m 필드
          """
          # 2개의 M2M 필드를 넣는다고 하면
          # 1. 입력할 m2m object 생성
          vio1 = self.create_violations(self.VIO1, self.LAW1) 
          vio2 = self.create_violations(self.VIO2, self.LAW2) 
          # 2. m2m object를 입력할 object(아래선 return_obj) 생성
          return_obj = obj.objects.create(**data)
          
          # 3. add를 사용하여 추가
          # 3-1
          return_obj.violations.add(vio1)
          return_obj.violations.add(vio2)
          # 3-2 전체 오브젝트 일 때 (vios)
  		return_obj.violations.add(**vios) 
           
          data = {
              "violations": v1,
              "img": self.IMG_URL,
              "detected_time": self.DETECTED_TIME,
          }
          return ViolationInfo(**data)
  ```

  

### 주의사항

같은 폴더 내의 파일을 import 할 때, `import views ` 를 하면 에러, `from . import views`로 해야 에러가 안난다.

