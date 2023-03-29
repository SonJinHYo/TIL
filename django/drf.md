DRF (메인 참고자료)



# DEF  (Django Rest Framework)

#### API (Application Programming Interface)

- 여러 프로그램, DB, 기능들의 상호 통신 방법을 규정하고 도와주는 매개체

#### REST API (RESTful API)

- REST는 제약 조건이다. 현재의 표준화가 된 제약 조건.

 - API가 RESTful로 간주되기 위한 조건.
   - 클라이언트, 서버 및 리소스로 구성되었으며 요청이 HTTP를 통해 관리되는 클라이언트-서버 아키텍처
   - [스테이트리스(stateless)](https://www.redhat.com/ko/topics/cloud-native-apps/stateful-vs-stateless) 클라이언트-서버 커뮤니케이션: GET 요청 간에 클라이언트 정보가 저장되지 않으며, 각 요청이 분리되어 있고 서로 연결되어 있지 않음
   - 클라이언트-서버 상호 작용을 간소화하는 캐시 가능 데이터
   - 정보가 표준 형식으로 전송되도록 하기 위한 구성 요소 간 통합 인터페이스. 여기에 필요한 것은 다음과 같습니다.
     - 요청된 리소스가 식별 가능하며 클라이언트에 전송된 표현과 분리되어야 합니다.
     - 수신한 표현을 통해 클라이언트가 리소스를 조작할 수 있어야 합니다(이렇게 할 수 있는 충분한 정보가 표현에 포함되어 있기 때문).
     - 클라이언트에 반환되는 자기 기술적(self-descriptive) 메시지에 클라이언트가 정보를 어떻게 처리해야 할지 설명하는 정보가 충분히 포함되어야 합니다.
     - 하이퍼텍스트/하이퍼미디어를 사용할 수 있어야 합니다. 즉, 클라이언트가 리소스에 액세스한 후 하이퍼링크를 사용해 현재 수행 가능한 기타 모든 작업을 찾을 수 있어야 합니다.
   - 요청된 정보를 검색하는 데 관련된 서버(보안, 로드 밸런싱 등을 담당)의 각 유형을 클라이언트가 볼 수 없는 계층 구조로 체계화하는 계층화된 시스템.
   - 코드 온디맨드(선택 사항): 요청을 받으면 서버에서 클라이언트로 실행 가능한 코드를 전송하여 클라이언트 기능을 확장할 수 있는 기능. 



## 환경설정

1. `pip install djangorestframework`
2. `settings.py`로 이동 후,  `INSTALLED_APPS`에 `'rest_framework'` 추가
3. Chorme에 `JSON Viewer`를 설치하면 보기 편함.



## views.py

### Functions

- `JsonResponse(dictionary)` : Json 응답을 위한 함수.  `from django.http`

- `serializers.serialize("json",queryset)` :  queryset을 해당 포맷으로 바꿔준다. "json/xml/jsonl/yaml" `from django.core`

  - serializers.py를 사용하지 않는 상황에서 주로 사용. 권장X

- `Response` : DRF에서 제공하는 response 함수. `from rest_framework.response`

- `api_view` : DRF에서 제공하는 **decorator**.  `from rest_framework.decorator`

- `exceptions `: 여러가지 에러 반환. `from rest_framework`

  - ex. `raise exceptions.NotFound `

- `status` : 여러가지 HTTP status 반환. `from rest_framework`

  - ex. `Response(status=status.HTTP_204_NO_CONTENT)`

- `transaction` : 에러시 쿼리와 db상태를 롤백. `from django.db`

  - 배경

    - django는 기본적으로 모든 쿼리를 db에 즉시 적용한다.
    - 쿼리를 생성하거나 수정할 때, 중간에 에러가 난다면 이전 수정사항을 수동으로 되돌려야 한다.
      - 수동 자체도 번거롭지만 pk, id의 낭비가 심해진다.

  - 작동방식

    1.  포함된 내용의 코드를 즉시 적용하지 않고 변경사항을 탐색
    2.  변경사항들을 리스트로 만듦
    3.  에러가 없다면 db에 push. 에러가 발생한다면 push하지 않는다.
        - ` transaction` 내에 `try-except`구문이 없어야 하는 이유. 에러 발생시 except 단계에서 에러가 제외됨.

  - ex. (`try-except` 구문으로 감싸지 않아도 작동은 같다. **감싸주는 편이 에러 파악에 용이**)

    ```python
    	try:
    		with transaction.atomic():
            	room = serialzer.sava(
                    owner = request.user,
                    category = category,
                	)
                options = request.data.get("options")
                
                for option_pk in options: # 위에서 데이터를 저장했지만 options의 데이터 상태나 내용물에서 에러가 날 수 있다.
                    pass
                
                return Response(serializer.data,status="~~")
        except Exception:
            raise ParseError("Option Not Found")
    ```

    


### Class

- `APIView` : views.py 클래스에 사용하는 상속클래스. `from rest_framework.views`
- `ModelViewSet` : 기본 . `from rest_framework.viewsets`

### example (ViewSet X)

```python
"""  (함수 방식) """
@api_view(["HTTP Methods"]) # api_view(["GET","POST","DELETE"])
def categories(request):
    """ serializer """
    all_categories = Category.objects.all() # 데이터를 불러와서
    serializer = CategorySerializer(all_categories,many=True) # serialize / 여러 객체일시 반드시 many=True 추가
	
    """ HTTP Method """
    if request.method == "GET":
        
	elif request.method == "POST":
       
    
    return Response({category : serializer.data}) # 데이터를 Response에 반환  


"""   권장(클래스 방식)   """
class Categories(APIView): # 모델은 단수/ 뷰는 복수형으로 명명
    
    # django convention. object 하나를 얻을 때 자주 선언
	def get_object(self,pk): 
        try:
            return Category.objects.get(pk=pk)
        except Category.DoseNotExist:
            raise exceptions.NotFound
    
    
    def get(self,request,pk):
		serializer = CategorySerializer(self.get_object(pk))
        return Response(serializer.data)
    
    
    def post(self,request,pk):
        serializer = CategorySerializer(data=request.data) # 요청 정보를 받아서 serializer
        if serializer.is_valid(): # is_valid()로 적합한 데이터 확인
            new_category = serializer.save() # 맞다면 db에 저장 후 객체로 선언
            return Response(
                CategorySerializer(new_category).data, # JSON으로 반환
            )
        else:
            return Response(serializer.errors,status = "status 추가") # 적합한 데이터가 아니라면 에러 반환
    
    
	def put(self,request,pk): 
        serializer = CategorySerializer(
            self.get_object(pk),
            data=request.data,
            partial=True, # 일부 데이터만 넣을 때 사용
        )
        if serializer.is_valid():
            updated_category = serializer.save()
            return Response(CategorySerializer(updated_category).data)
        else:
            return Response(serializer.errors,status = "status 추가")
        
        
	def delete(self, request, pk):
        self.get_object(pk).delete()
        return Response(status=HTTP_204_NO_CONTENT)
       
```

##### `POST, DELETE, PUT`할 때 유저확인

```python
# 직접 쓰기 (권장X)
	def post(self,request,pk):
        if request.user.is_authenticated: # 유저 인증이 됐다면
            serializer = CategorySerializer(data=request.data) 
            if serializer.is_valid(): 
                new_category = serializer.save() 
                return Response(
                    CategorySerializer(new_category).data,
                )
            else:
                return Response(serializer.errors)
        else: # 아닐시 
			raise exceptions.NotAutenticated
	
  	def delete(self, request, pk):
        obj = self.get_object(pk)
        if not request.user.is_authenticated: # 유저 인증과
            raise exceptions.NotAutenticated
        if room.owner != request.user:	# 소유자 여부. PUT일 때도 이 2가지 체크
            raise exceptions.PermissionDenied
		obj.delete()
        return Response("data",status=HTTP_204_NO_CONTENT)    
```

```python
# permission_classes 사용 (권장O)
from rest_framework.permisstions import IsAuthenticated

class Categories(APIView):
	permission_classes = [IsAuthenticated]
    
    
from rest_framework.decorators import permission_classes

# api_view에서의 사용
@api_view(['GET'])
@permission_classes([IsAuthenticated])
def example_view(request, format=None):
    pass
```

- permission_classes 종류 (DRF docs) : https://www.django-rest-framework.org/api-guide/permissions/#api-reference

### example(ViewSet)

DRF docs : https://www.django-rest-framework.org/api-guide/viewsets/#viewsets

커스텀이 많다면 사용하지 않는것을 권장.

- 기본적인 과정을 많이 가리고 주어진 기능만을 편하게 다룰수 있게 해주기 때문

```python
""" views.py """
from .models import Category
from .serializers import CategorySerializer

class CategoryViewSet(ModelViewSet):
    
    serializer_class = CategorySerializer
    queryset = Category.objects.all()
    
    
""" urls.py """
urlpatterns = [
    path("<int:pk>",views.CategoryViewSets.as_view({
        'get':'retrive',
        'put':'partial_update',
        'delete':'destroy',
    }))
]
```





## serializers.py

### Functions

- **QuerySet is not JSON serializalbe**
  - serializer : QuerySet을 브라우저가 이해할 수 있는 JSON/ JSONL/ XML/ YAML 포맷으로 바꿔주는 번역기 역할
    - post와 같은 반대 상황에서도 적용

### Class

- `ModelSerializer`: serializers.py 클래스의 상속클래스.  `from rest_framework.serializers`
  - ex. `class CategorySerializer(serializers.Serializer):`



### example

```python
from rest_framework.serializers import ModelSerializer

class CategorySerializer(ModelSerializer):
    """
    Category의 Field중 보여줄 부분 명시. Category의 필드를 
    Category가 어떻게 JSON으로 변환될 지 커스텀
    """
    class Meta: # db에 저장할 데이터가 아니므로
        model = Category
        fields = "__all__" # 모든 필드 보이기
        fields = ("properties") # 일부 필드 보이기
        exclude = ("properties") # 일부 필드 제외
```



### 관계확장 (Serializer 내의 Serializer)

```python
from users.serializers import TinyUserSerializer

class CategorySerializer(serializers.Serializer):
    
    # 권장O
    """
    owner 필드를 가져올 때 커스텀한 TinyUserSerializer(간단유저정보)을 참조
    단, Category를 POST할 때, TinyUserSerializer를 필요로 해선 안되므로 read_only=True 추가
    """
    owner = TinyUserSerialzer(read_only=True)
    
    class Meta: 
        model = Category
        fields = "__all__"
        
        # 권장X
        depth = 1 # 필드의 모든 데이터를 출력
```

#### 특정 필드를 GET에선 Custom Serializer, POST에선 request.data 받기 (ex. owner 필드)

1. `serializer = CategorySerializer(data=request.data)` 

2. 이후 save시, `category = serializer.save(owner = request.user)` 으로 추가



### SerializerMethodField

#### 함수의 결과를 Serializer에서 사용하는 법

1.  Serializer 모델클래스 내에서 `SerializerMethodField` 선언. `rating = seializers.SerializerMethodField()`
2.  Serializer 모델클래스 내에서 함수 선언.  **이 때 반드시 get_함수명 으로 명명하고, 함수명은 변수와 같아야한다.** `def get_rating(self,object):`
    - args : self, object(해당 객체를 받는다.



### Serializer Context

- Serializer의 인자에 `context = {}`를 넣어서 추가적인 내용을 전달할 수 있다
- ex. `serializer = CategorySerializer(catergory,context = {"pet_name":"pupu"})`
  - 이후 Serializer 클래스에서 `self.context`로 접근이 가능
- **requset를 그대로 전할 때 주로 사용**. `context = {"request":request}`



### Pagination

#### 역접근자의 위험성

- 역접근하는 모델의 모든 데이터를 가져올 때, 너무 많은 양의 데이터를 한번에 불러올 수 있음 -> Pagination 필요

#### Pagination 설정

1. Pagination은 전체페이지에 적용하는 설정이므로 `settings.py`에 설정

   - `PAGE_SIZE = 5` 줄 추가 후 view 클래스에 적용. (`from django.conf import settings`)

2. ```python
   from django.conf import settings
   
   class RoomReviews(APIView):
       def get_object(self, pk):
           try:
               return Room.objects.get(pk=pk)
           except Room.DoesNotExist:
               raise NotFound
   
       def get(self, request, pk):
           try:
               # http창에 `~~?page=2`의 입력을 받는다. get의 두번째 원소는 default값
               page = request.query_params.get("page", 1) 
               page = int(page) # 정수 변환(기본은 str타입). 정수 입력이 아닐 경우를 대비해 try구문
           except ValueError:
               page = 1
           page_size = settings.PAGE_SIZE
           start = (page - 1) * page_size
           end = start + page_size
           room = self.get_object(pk)
           serializer = ReviewSerializer(
               room.reviews.all()[start:end], # 현재 인덱스에서 설정한 페이지까지만
               many=True,
           )
           return Response(serializer.data)
   ```



### Create User

```python
""" users.serializers.py """
class PrivateUserSerializer(ModelSerializer):
    class Meta:
        model = User
        exclude = (
            "password",
            "is_superuser",
            "id",
            "user_permissions", # 제외할 필드목록
        )

""" users/views.py """
class Users(APIView):
    def post(self, request):
        password = request.data.get("password") # 입력받은 패스워드
        if not password:
            raise ParseError
        serializer = serializers.PrivateUserSerializer(data=request.data)
        if serializer.is_valid():
            user = serializer.save()
            user.set_password(password)
            user.save()
            serializer = serializers.PrivateUserSerializer(user)
            return Response(serializer.data)
        else:
            return Response(serializer.errors)
```

#### Save User Password

- **틀린 방법** : `user.password = password`  (`password`는 입력받은 비밀번호)
  - 저장하는 password는 해쉬화가 된 패스워드여야 한다.
  - **옳은 방법** : `user.set_password(password)`
- 현재 비밀번호 확인 방법 : `if user.check_password(now_password):`
  - `now_password` : 입력받은 현재 패스워드. `now_password = request.data.get("now_password")`



### Log In/Out

```python
class LogIn(APIView):
    def post(self, request):
        username = request.data.get("username")
        password = request.data.get("password")
        if not username or not password: # 입력 에러
            raise ParseError
        user = authenticate(
            request,
            username=username,
            password=password,
        )
        if user:
            login(request, user)
            return Response({"ok": "Welcome!"})
        else:
            return Response({"error": "wrong password"})


class LogOut(APIView):

    permission_classes = [IsAuthenticated]

    def post(self, request):
        logout(request)
        return Response({"ok": "bye!"})
```

- `authenticate(request=None,**credentials)` : User 인증함수 `from django.contrib.auth import authenticate,login,logout`
  - 유요한 User객체가 있다면 해당 객체를 반환. 아닐 시 None 반환
- `login(request,user,backend=None)` : 로그인 함수
  - django의 세션 프레임워크를 사용하여 세션에 인증된 사용자의 ID를 저장
- `logout(request)` : 로그아웃 함수
  - 요청한 로그인 세션에 대한 모든 데이터 삭제



### 주의사항

Serializer은 대부분 한 객체의 데이터를 표현한다. 여러 객체를 받게될 시 `Serializer(many=True)`를 추가

- ex. `member = MemberSerializer(many=True)` : 어떤 Serializer 클래스가 member 필드를 가질 때, 멤버가 두 명 이상일 수 있으므로.
- 코드에 이상이 없음에도 DRF 페이지에서 `null` 값이 등장한다면 `many` 여부를 확인





## Authentication

views.py 의 클래스가 가지는 함수는 `(self,request)` 를 기본적으로 가지고, 이 때 `request.user` 로 user을 불러올 수 있다. 이 때 DRF는 백엔드에서 **쿠키,세션**을 보고 user을 식별 하고 그 과정에서 AUthentication 클래스가 쓰인다.



### Token Authentication

django 문서 : https://www.django-rest-framework.org/api-guide/authentication/#tokenauthentication

작동 방식

1. 처음에 user에게 token을 주고 해당 토큰을 DB에 저장

2. DEFAULT_AUTHENTICATION_CLASSES에 토큰인증이 되어있다면 자동으로 request에서 토큰을 찾아 user을 파악
   - **APIView가 실행될 때 마다 토큰을 검색**

규칙 : `headers`의 `Authorization `안에 토큰을 넣어야 한다.

- `KEY : Authorization`
- `VALUE : Token 토큰을적는위치 `(Token을 써주고 한칸 띄워서 붙여넣는다)



#### 코딩 순서

1. `settings.py` 에서 `INSTALLED_APPS(또는 커스텀한 THIRD_PARTY_APPS)`에 `"rest_framework.authtoken"` 추가

   - 이를 위해 새로운 DB컬럼이 추가된다. **즉, `migrate` 필요** (`makemigrations` 필요X)

2. `settings.py` 에 다음을 추가

   ```python
   REST_FRAMEWORK = {
       "DEFAULT_AUTHENTICATION_CLASSES": [
           "rest_framework.authentication.TokenAuthentication", # 추가
       ]
   }
   ```

3. `users/urls.py` 에 url 추가 (view는 django가 이미 가지고 있다)

   ```python
   from rest_framework.authtoken.views import obtain_auth_token
   
   urlpatterns = [
       ...,
       path("token-login",obtain_auth_token),
   ]
   ```

4. `token-login`으로 로그인한 유저가 POST하면 토큰을 반환받는다.

5. 이후 유저가 무언가를 반환받기 위해선 **규칙**에 따라 토큰을 보내주어야 한다.





### JWT (JSON Web Token) Authentication

**토큰을 저장해야하는 기존의 방식과는 다르게 DB의 소모가 없는것이 장점. 단, 유저의 강제 로그아웃이 불가능** 



작동 방식

1. 유저가 아이디, 패스워드를 주면
2. 토큰을 생성하고 유저 정보를 토큰에 넣어서 유저에게 준다. (ID 같은 데이터)
3. 유저는 토큰을 가지고 있다가 다시 보낸다.
4. 토큰을 열어 이전에 넣었던 정보를 확인.



#### 코딩 순서

1. `pip install pyjwt` 또는 `poetry add pyjwt`

   ```python
   ## jwt 사용 코드
   import jwt
   
   # 암호화. 유저에겐 암호화된 정보가 보임
   encoded_jwt = jwt.encode({"some":"payload"},"secret",algorithm="HS256")
   # 복호화
   decode_jwt = jwt.decode(encoded_jwt,"secret",algorithm=["HS256"]) 
   print(decode_jwt) # {"some":"payload"}
   ```

2. `users/urls.py` 에 url 추가 (**view 따로 추가**)

   ```python
   from . import views
   
   urlpatterns = [
       ...,
       path("token-login",views.JWTLogin.as_view()),
   ]
   ```

3. ```python
   # views.py
   
   class JWTLogIn(APIView):
       def post(self, request):
           username = request.data.get("username")
           password = request.data.get("password")
           if not username or not password:
               raise ParseError
           user = authenticate(
               request,
               username=username,
               password=password,
           )
           if user: # 유저 정보를 암호화하여 반환
               token = jwt.encode(
                   {"pk": user.pk},
                   settings.SECRET_KEY, # django의 SECRET_KEY를 사용
                   algorithm="HS256",
               )
               return Response({"token": token})
           else:
               return Response({"error": "wrong password"})
   ```

   - 암호화할 때, 위의 예시에선 `pk`만 넣음.
     - 복호화의 가능성이 있기 때문에 민감한 정보X. 
     - **JWT의 보안점은 암호화**가 아닌 **우리가 준 토큰인지, 수정이 있었는지 알 수 있다는 것**. 

4. 여기부터 decode 단계. settings.py에 다음을 추가

   ```python
   REST_FRAMEWORK = {
       "DEFAULT_AUTHENTICATION_CLASSES": [
        ...,
   	"config.authentication.JWTAuthentication", # 추가
       ]
   }
   ```

5. `config.authentication.py`생성 후 클래스 추가

   ```python
   from multiprocessing import AuthenticationError
   import jwt
   from django.conf import settings
   from rest_framework.authentication import BaseAuthentication
   from rest_framework.exceptions import AuthenticationFailed
   from users.models import User
   
   class JWTAuthentication(BaseAuthentication):
       def authenticate(self, request):
           token = request.headers.get("Jwt")
           if not token:
               return None
           decoded = jwt.decode(
               token,
               settings.SECRET_KEY,
               algorithms=["HS256"],
           )
           pk = decoded.get("pk")
           if not pk:
               raise AuthenticationFailed("Invalid Token")
           try:
               user = User.objects.get(pk=pk)
               return (user, None)
           except User.DoesNotExist:
               raise AuthenticationFailed("User Not Found")
   ```

6. 이하 Token 방식과 동일한 POST 방식









































