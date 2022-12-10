# SQL_기본(1)



#### 관계형 데이터베이스 예제 - 웹서비스 사용자/세션 정보

- 사용자 ID : 보통 웹서비스 내에서, 등록된 사용자마다 부여하는 유일한 ID

- 세션 ID : 세션마다 부여되는 ID

  - 세션 : 사용자의 방문을 논리적인 단위로 나눈 것
    - 사용자가 외부 링크(광고 등)를 타고 오거나 직접 방문해서 올 경우 세션 생성
    - 사용자가 방문 후 30분간 interaction이 없다가 뭔가를 하는 경우 새로 세션을 생성
  - 하나의 사용자는 여러 개의 세션을 가질 수 있음
  - 보통 세션의 경우 세션을 만들어낸 접점(경유지)를 채널이란 이름으로 기록
    - 마케팅 관련 기여도 분석을 위해
  - 세션이 생긴 시간도 기록

- 이 정보들을 기반으로 다양한 데이터 분석과 지표 설정이 가능

  - 마케팅 관련, 사용자 트래픽 관련
  - DAU, WAU, MAU 등의 일/주/월별 Active  User 차트 표현 가능
  - 마케팅 채널 기여도 분석에 효과적
    - 어느 채널에 광고를 하는 것이 가장 효과적인가?

- Example

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221209190707678.png" alt="image-20221209190707678" style="zoom:80%;" />





#### 관계형 데이터베이스 예제 - 데이터베이스와 테이블

<img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221209191115123.png" alt="image-20221209191115123" style="zoom:50%;" />



# SQL 기본

- 다수의 SQL 문을 실행한다면 세미콜론으로 분리 필요
  - SQL문1; SQL문2; . . .
- SQL 주석
  - -- : 인라인 한줄짜리 주석
  - /* -- */  :  여러 줄에 걸쳐 사용 가능한 주석
- SQL 키워드는 대문자를 사용한다던지 하는 나름대로의 포맷팅이 필요
  - 팀 프로젝트라면 팀에서 사용하는 공통 포맷이 필요
- 테이블/필드이름의 명명구칙을 정하는 것이 중요
  - 단수형 vs 복수형
    - User vs Users
  - _ vs CamelCasing
    - user_session_chaneel vs UserSessionChannel



#### SQL DDL

- CREATE TABLE

- Primary key 속성을 지정할 수 있으나 무시됨

  - Primary key uniqueness 
    - Big Data 데이터웨어하우스에서는 지켜지지 않음 (Redshift, Snowflake, BigQuery)

- CTAS: CREATE TABLE table_name AS SELECT

  - vs. CREATE TABLE and then INSERT

  <img src="../../AppData/Roaming/Typora/typora-user-images/image-20221209193837293.png" alt="image-20221209193837293" style="zoom:50%;" />

- DROP TABLE

  - DROP TABLE table_name;
    - 없는 테이블을 지우려고 하는 경우 에러를냄
  - DROP TABLE IF EXISTS table_name;
  - vs DELETE FROM
    - DELETE FROM은 조건에 맞는 레코드들을 지움 (테이블 자체는 존재)
    - DROP TABLE은 테이블을 날림

- ALTER TABLE

  - 새로운 컬럼 추가
    - ALTER TABLE  table_name ADD COLUMNS field_name field_type;
  - 기존 컬럼 이름 변경
    - ALTER TABLE  table_name RENAME now_field_name to new_field_name;
  - 기존 컬럼 제거
    - ALTER TABLE  table_name DROP COLUMN field_name;
  - 테이블 이름 변경
    - ALTER TABLE  now_table_name RENAME to new_table_name;

- SELECT

  - SELECT FROM : 테이블에서 레코드와 필드를 읽어오는데 사용
  - WHERE를 사용해서 레코드 선택 조건을 지정
  - GROUP BY를 통해 정보를 그룹 레벨에서 뽑는데 사용하기도 함
    - DAU, WAU, MAU 계산은 GROUP BY를 필요로 함
  - ORDER BY를 사용해서 레코드 순서를 결정하기도 함
  - 보통 다수의 테이블을 조인해서 사용하기도 함

- 레코드 수정 언어

  - INSERT INTO : 테이블에 레코드를 추가
  - UDATE FROM : 테이블 레코드의 필드 값 수정
  - DELETE FEOM : 테이블에서 레코드를 삭제





### SELECT 자세히

<img src="../../AppData/Roaming/Typora/typora-user-images/image-20221209200752883.png" alt="image-20221209200752883" style="zoom:67%;" />

- LMINT : 판다스의 head라 생각 => 상위 데이터로 데이터의 상태 확인



- SELECT  * (테이블의 모든 필드를 가져와라)

  FROM raw_data.user_session_channel ( raw_data의 user_session_channel로 부터)

  LIMIT 10; (10 개만)

- 유일한 채널 이름을 알고 싶은 경우

  - SELECT DISTINCT channel

    FROM raw_data.user_session_channel;

- 채널별 카운트를 하고 싶은 경우  - COUNT 함수 사용

  - SELECT channel, COUNT(1) (그룹안에 속한 채널의 갯수를 센다)

    FROM raw_data.user_session_cahnnel

    GROUP BY 1; 

- 테이블의 모든 레코드 수 카운트

  - SELECT COUNT(1)

    FROM raw_data.user_session_cahnnel;

- channel 이름이 Facebook 경우만 고려해서 레코드 수 카운트

  - SELECT COUNT(1)

    FROM raw_data.user_session_cahnnel

    WHERE channel = 'Facebook';

- CASE WHEN

  - 필드 값의 변환을 위해 사용 가능

    - CASE WHEN 조건 THEN 참일때 값 ELSE 거짓일 때 값 END 필드이름

  - 여러 조건을 사용하여 변환하는 것도 가능

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221209201935754.png" alt="image-20221209201935754" style="zoom:67%;" />



#### NULL

- 값이 존재하지 않음을 나타내는 상수
  - 0 , ""과는 다름
- 필드 지정시 값이 없는 경우 NULL로 지정 가능
  - 테이블 정의시 디폴트 값으로는 지정 가능
- 어떤 필드의 값이 NULL인지 아닌지 비교는 특수한 문법을 필요로 함 (field1 is NULL / field1 is not NULL )





### WHERE 자세히

- IN

  - WHERE channel IN ('Google','Youtube)

    - 구글, 유튜브 채널만

    - NOT IN 이라면 구글 유튜브를 빼고 

- LIKE and ILIKE

  - LIKE is a case sesitive string match

    - WHERE channel LIKE 'G%' -> 'G*'

    - WHERE channel LIKE'%o%' -> '* o*'

  - ILIKE is acase-insensitive string match : 대소문자 구분을 안하고 싶을 때

- BETWEEN

  - 범위를 사용할 때 

- 위 오퍼레이터들은 CASE WHEN 사이에서도 사용 가능



### STRING Functions

- LEFT(str,N) : str을 N만큼 떼어낸다
- REPLACE(str,exp1,exp2) : exp1에서 str을 찾아서 exp2를 리턴
- UPPER(str),LOWER(str),LEN(str) : 아는것과 같음
- LPAD, RPAD : 문자의 왼/오른쪽에 padding을 주는 것
- SUBSTRING : 시작과 끝을 정해서 추출 가능
