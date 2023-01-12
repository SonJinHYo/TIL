# django의 python teminal로 데이터 다루기

## `python manage.py shell` : django 프레임워크에 기반한 python terminal 실행 (일반 python terminal이랑 다름)
 - `>>> from rooms.models import Room` : rooms앱의 models에 있는 Room import
 - `>>> Room.objects` : Room 데이터에 접근



#### `>>> Room.obects.all()` : 모든 데이터 표시


#### `>>> Room.obects.get(name = "delux_room")` : 이름이 delux room인 객체 표시
 - get() 함수는 단 하나의 결과만 반환한다.
 - 반환 결과가 2개 이상이면 에러
 
#### `>>> Room.obects.get(pet_friendly = True)` : pet_friendly = True 인 모든 방을 표시
 - filter() 함수는 조건을 만족하는 모든 결과 반환.
 - 반환 결과가 2개 이상 가능
 - 내장된 기능을 사용한 조건 적용 가능 : `>>> Room.obects.get(price__gt = 15)`
   - 변수__( )
     -  gt,lt,gte,lte : 큰것,작은것,크거나 같은거, 작거나 같은것 (위 예시는 price > 15 인 것)
     -  contains="sss" : 변수들 내에 sss가 포함된 것들
     -  stratswith="sss" : 변수들 내에 sss로 시작하는 것들
 - 조건 2개 이상 가능 : `>>> Room.obects.get(pet_friendly = True, get(price__gt = 15)`

#### `>>> Amenity.objects.create(name="Amenity from the console", description = "How cool is this!")`
 - create() 함수는 생성함수
 - 단 생성하기 위해선 해당 클래스의 요소들을 알고 있어야 올바르게 생성이 가능

#### `>>> object.delete()`
 - delete() 함수는 삭제함수. 해당 object를 삭제

#### `objects.count()` : 해당 objects 갯수 반환

## QuerySet
 - 데이터를 더 잘 다룰수 있게 연산자까지 포함한 Set


## lookup ( __ ) : 
 - 변수 뒤에 2개 underbar을 붙여서 조건을 다는 것 (위 filter에 예시)
 - lookup 종류 : https://docs.djangoproject.com/en/4.1/ref/models/querysets/#field-lookups

### reverse accesses
 - `Room.objects.filter(owner__username = jane)`
  - owner Room의 요소, username은 users.User의 요소이다
  - lookup을 이용하여 Room에서 User로 역으로 접근
  - 
#### reverse accesse를 이용해서 특정 room에 있는 user의 모든 room 보기
```
>>> me = Room.objects.get(pk = 2)
>>> me.room_set

# 만약 me가 어떤 속성을 가지는지 모른다면
>>> dir(me) # me가 가지는 모든 속성을 표시 
```

#### room_set이 생성되는 시점
``` python
# owner은 users.User을 ForeignKey로 사용하고
owner = models.ForeignKey(
    "users.User", # 이 때 user에 room_set 생성
    on_delete=models.CASCADE,
)
```

##### room_set 대신 `user.rooms` 로 접근하려면
 - ForeignKey에 related_name 추가
``` python
owner = models.ForeignKey(
    "users.User",
    on_delete=models.CASCADE,
    related_name = "rooms",
)
```
