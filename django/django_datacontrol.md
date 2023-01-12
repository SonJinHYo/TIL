# python trminal로 데이터 다루기

## `python manage.py shell` : django 프레임워크에 기반한 python terminal 실행
 - `>>> from rooms.models import Room` : rooms앱의 models에 있는 Room import
 - `>>> Room.objects` : Room 데이터에 접근
  - `>>> Room.obects.all()` : 모든 데이터 표시
  - `>>> Room.obects.get(name = "delux_room")` : 이름이 delux room인 객체 표시
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
