DRF



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
            return Response(serializer.errors) # 적합한 데이터가 아니라면 에러 반환
    
    
	def put(self,request,pk): 
        serializer = CategorySerializer(
            category,
            self.get_object(pk),
            data=request.data,
            partial=True, # 일부 데이터만 넣을 때 사용
        )
        if serializer.is_valid():
            updated_category = serializer.save()
            return Response(CategorySerializer(updated_category).data)
        else:
            return Response(serializer.errors)
        
        
	def delete(self, request, pk):
        self.get_object(pk).delete()
        return Response(status=HTTP_204_NO_CONTENT)
       
```



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

## Class

- `serializers.Serializer`: serializers.py 클래스의 상속클래스.  `from rest_framework import serializers`
  - ex. `class CategorySerializer(serializers.Serializer):`



### example

```python
class CategorySerializer(serializers.Serializer):
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

