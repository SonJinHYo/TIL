ORM



# Object Relationship Mapping

django docs : https://docs.djangoproject.com/ko/4.1/topics/db/queries/#retrieving-objects

## 기본 문법 

QuerySet에 관한 django docs : https://docs.djangoproject.com/ko/4.1/ref/models/querysets/#django.db.models.query.QuerySet

`from models.py import Room`

- `Room.objects.all()`  : Room의 모든 객체 불러오기
- `Room.objects.get(조건)` : 조건을 만족하는 **하나의** Room 객체 불러오기
  - ex)  `room = Room.objects.get(name = "moon")` 
  - 조건은 여러 입력 가능. ex) `Room.objects.get(name = "moon", price=15)`
- `Room.objects.filter/exclude(조건)` :  조건에 맞는 **모든 객체를/ 제외한 모든 객체를** 불러오기
  - `rooms = Room.objects.filter(price__gt/get/lt/lte = 15)` : price가 15 보다 **큰/크거나 같은/작은/작거나 같은**인 모든 객체
  - `rooms = Room.objects.filter(name__contains/startswith/ends = "event")`  : name에 event가 **포함되는/ 시작하는 / 끝나는** 객체
- `room.price = 2000` : update data. **단, db에 저장되는 것이 아님**
- `room.save()`  : db에 저장
- `Room.objects.create(Room의 필수 요소)`  : 객체 생성
- `room.delete()` : room 삭제 ( `room = Room.objects.get(삭제할 객체를 찾기 위한 조건)` 과 함께 사용)
- `room.count()` : 객체 갯수 반환
- 등등 문서 참고

##### 주의사항 (최적화)

```python
for room in Room.objects.all():
    # 이 떄 room은 QuerySet 이므로 room의 필요한 값은 다음과 같이 호출
    options = room.value['options'] # room의 options가 필요할 때
    print(option)
    
### db소통 최적화. 위와는 다르게 옵션'만' 가져옴
for option in Room.objects.all().values("options"):
    print(option)
```





### lookup

django의 `__arg`(밑줄2개) 연산자. 잘못된 `arg`를 전달하면 **TypeError**가 뜬다.

연달아 사용할 수 있다. `objects.filter(pub_date__year__gte=2005)`

django docs = https://docs.djangoproject.com/ko/4.1/topics/db/queries/#field-lookups

#### arg list

- 위의 `gt/gte/lt/lte`   : 보다 큰/ 크거나 같은/ 작은/ 작거나 같은
- `exact/contains/startswith/endswith` : 정확히 일치하는/ 포함하는 /시작하는/ 끝나는  값 찾기
  - `iexact` : 대소문자 구별 X 하는 exact. 
  - **이후로도 맨 앞에 i가 추가로 붙으면 대소문자 구별을 제외한 함수** (ex `icontains`, `iendswith`)
- 등등 문서 참고



#### ForeignKey

foreignkey의 뒤쪽에 lookup을 적용시, foreignkey에 접근이 가능

- Room클래스의 ForeignKey가 `owner`일 때,
  -  `Room.objects.filter(owner__username="Son")` : owner의 username이 Son 인 모든 방



## QuerySets

- django의 반환 결과는 queryset 객체이다
- QuerySet은 데이터를 요청받았을 때만 db와 소통한다.
  - 즉, `room = Room.objects.get(pk=21)`을 요청할 경우, `room`이 가진 price와 같은 값은 모르는 것.
  - why? - 수십 수백만의 데이터를 요청할 경우 모든 db테이블을 가져오는건 힘들고 비효율적이기 때문
    - 따라서 구체적인 요청을 할 때만 db와 소통한다.
    - `p = room.price`를 하는 순간 db와 소통하여 room.price를 가져오는 것



## Reverse Accessors

django는 모델 내에서 ForeignKey가 선언될 때, 매번 `_set` 속성을 받아 **역접근을 가능하게 해준다.**

- `room.review_set` :  room의 모든 reivew객체로 역접근

- * **Review 클래스에서** `ForeignKey(related_name = 'reviews')` : `ForeignKey`에서 `realted_name` 선언 시, 역접근 이름 설정 가능 **(권장)**

  - `room.reivews.all()` == `room.review_set.all()`
  - `related_name ` 선언 시 `_set` 속성은 생성 X

##### 선언 시 객체 방향 주의

- **Room모델**이 **Review모델을 여러 개** 가진다 .

  - ```python
    class Review:
        room = models.ForeignKey(
            ...,
            realted_name = "reviews"
        )
    ```

  - **일** 대 **다** => **다** 에서 선언



#### 객체 정보 찾기

- `dir(object)` :  object의 모든 속성, 클래스, 메소드, 클래스의 인스턴스를 보여준다.

`room = Room.objects.get(pk=1)` 일 때,

### object property에 접근

`room.price`, `room.owner.username`



### object의 

- Room에 수  많은 review들이 있다면? (Reviews 클래스와 이어져 있다면?)
  - ``
