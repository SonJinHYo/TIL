django flow



# 환경

1. `poetry init` : 가상환경 설정. `pyproject.toml`, `poetry.lock `파일을 생성하여 라이브러리 관린
2. `poetry shell ` 
3. `django-admin startproject <project name> <path>`  : ex)`django-admin startproject config .` - 현 폴더에 `config` 관리폴더와 `manage.py `생성
4. `command pallette` 를 통해 `gitignore` 생성

## 주의사항

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
    
    def __str__(self):
        # 해당 객체의 이름 표현
        return self.name
```

### 상용 Field(essential_param=) https://docs.djangoproject.com/ko/4.1/ref/models/fields/#field-types

- `CharField(max_length=)` : 짧은 크기의 문자열 입력란
- `PositiveIntegerField()` 
- `TextField()` : 많은 양의 텍스트를 쓸 수 있는 입력란
- `BooleanFiled(default=)`
- `DateField()` : 날짜 입력
- `DateTimeField`: 날짜와 시간
- `EmailField()` : 이메일
- `ImageField,URLfield,...`

#### Field Parameter(default value) https://docs.djangoproject.com/ko/4.1/ref/models/fields/

- `null=False`  : True시 빈 값은 모두 null 값이 된다
- `blank=False` :  빈 칸을 허용한다
- `default=` : default값 설정
- `editable=True` : False 일 때, 수정이 불가능하면 관리 패널에서 사라짐
- `help_text=` : 해당 필드의 부가설명을 추가한다.
- `verbose_name=` : 설정한 이름으로 필드 이름을 설정. default로는 django가 자동으로 필드 이름에 맞게 생성



## admin.py

### 해당 앱 관리자 등록 https://docs.djangoproject.com/ko/4.1/ref/contrib/admin/

```python
from django.contrib import admin
from .models import House

@admin.register(House) # House class를 관리하는 class
class HouseAdmin(admin.ModelAdmin):
    list_display = ("name","price","rooms",)
    list_filter = ("price","city",)
    
```

- `list_display = (model의 properties)` : 관리창에 관리모델의 미리보기를 입력에 맞게 보여준다.
- `list_filter = (model의 properties)` : 관리창 측면에 필터 생성. 입력에 맞게 필터를 생성한다.
- `search_fields = (model의 properties)` : 검색창 생성
  - ex) `search_fields = (address,price)` : 검색창에 입력시 address와 price를 기준으로 입력값이 **포함된**  모든 것을 검색
  - properties 뒤에 두 개의 언더바를 붙여서 옵션 설정 가능
    - `search_fields = (address_starts/ends/contains/with)` : 검색한 텍스트로 시작하는/끝나는/포함하는 address만 검색.
      - 위 조건들에 앞에 i가 붙으면 대소문자 구별X ( ex. istartswith )
    - `<field_name>__lt/lte/gt/gte = val` : field_name  `<`  / `<=` / `>` / `>=` val
    - `<field_name>__in = val_list` : val_list 내의 객체들 반환
- `exclude = (model의 properties)` : 해당 property 제외

- `fields = (model의 properties,(model의 properties),)` : 튜플로 묶어진 property는 한 칸에 같이 표현





