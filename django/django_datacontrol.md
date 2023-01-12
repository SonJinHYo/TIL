# python trminal로 데이터 다루기

## `python manage.py shell` : django 프레임워크에 기반한 python terminal 실행
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


```
>>> room = Room.objects.get(name="cool room")
>>> room.id
2
>>> room.owner
User:James
.
.
.

```
