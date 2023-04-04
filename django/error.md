에러 모음(재정리)

# migration 관련
- `No change detected` : migrate 가 된 후 다시 migrate가 되었을 때
  - 해결방법 :  `python manage.py makemigrations <앱이름>` (직접 migration 항목을 지정)

# Timezone 관련
 - `RuntimeWarning: DateTimeField received a naive datetime` : django model의 `DateTimeField`의 시간과 현재 시간이 안맞을 때
  - `settings.py`에서는 Asia/Seoul 을 사용하고 test시에 시간은 `datetime`으로 입력을 받아 같은 시간대가 적용되지 않았다.
- 해결방법
  1. datetime으로 받는 것이 아닌, `from django.utils import timezone` 을 통해 `timezone` 함수로 시간을 입력받는다.
  2. settings.py의 `USE-TZ=False` 로 바꿔 django가 로컬 시간을 사용하도록 한다.
