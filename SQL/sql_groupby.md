## GROUP BY & Aggregate함수 & CATS

- 테이블의 레코드를 그룹핑하여 그룹별로 다양한 정보를 계산

  1. 먼저 그룹핑할 필드를 결정
     - GROUP BY로 지정
  2. 그룹별로 계산할 내용을 결정
     - Aggregate 함수 사용
     - COUNT, SUM, AVG, MIN, LISTAGG, ...
       - 보통 필드이름을 지정하는 것이 일반적

- GROUP BY를 이용한 문제해결

  - 월별 세션수를 계산하는  SQL

    - raw_data.session_timestamp사용 (sessionId와 ts필드)

      - SELECT

        ​	LEFT(ts,7) AS mon,

        ​	COUNT(1) AS session_count

        FROM raw_data.session_timestamp

        GROUP BY 1 (-- GROUP BY mon, GROUP BY LEFT(ts,7))

        ORDER BY 1;

  - 가장 많이 사용된 채널 파악

    - 체크사항 : 가장 많이 사용되었다의 정의는? 필요한 정보는? 사용해야할 테이블은?

    - SELECT

      ​	channel,

      ​	COUNT(1) AS session_count,

      ​	COUNT(DISTINCT userId) AS user_count

      FROM raw_data.user_session_channel

      GROUP BY 1 	(-- GROUP BY channel)

      GROUP BY 2 DESC;	(-- ORDER BY session_count DESC)

  - 가장 많은 세션을 만들어낸 사용자의 ID는 무엇인가

    - SELECT

      ​	userId,

      ​	COUNT(1) AS count

      FROM raw_data.user_session_chaneel

      GROUP BY 1

      ORDER BY DESC

      LIMIT 1;

  - 월별 유니크한 사용자 수 (MAU)

    - SELECT

      ​	TO_CHAR(A.ts,'YYYY-MM' AS month),

      ​	COUNT(DISTINCT B.userId) AS mau

      FROM raw_data.session_timestamp AS A

      JOIN raw_data.user_session_channel AS B ON A.sessionid=B.sessionid (JOIN ~ ON ~)

      GROUP BY 1;

      ORDER BY 1 DESC;

  - 월별 채널별 유니크한 사용자 수

    - SELECT

      ​	TO_CHAR(A.ts,'YYYY-MM) AS month,

      ​	channel,

      ​	COUNT(DISTINCT B.userId) AS mau

      FROM raw_data.session_timestamp as A

      JOIN raw_data.user_session_channel B ON A.sessionid = B.sessionid

      GROUP BY 1,2

      ORDER BY 1 DESC, 2;





### CATS : SELECT를 가지고 테이블 생성

- 간단하게 새로운 테이블을 만드는 방법

- 자주 조인하는 테이블들이 있다면 이를 CATS를 사용해서 만들어두면 편함

  DROP TABLE IF EXISTS adhoc.jinhyo_session_summary; (내 이름을 넣어서 이후의 혼란 방지)

  CREATE TABLE adhoc.jinhyo_session_summary AS

  SELECT B.*, A.ts FROM raw_data.session_timestamp A

  JOIN raw_data.user_session_channel B ON A.sessionid=B.sessionid;

#### 항상 시도해봐야하는 데이터 품질 확인 방법들

- 중복된 레코드들 체크하기

  SELECT COUNT(1)

  FROM adhoc.jinhyo_session_summary;

  

  SELECT COUNT(1)

  FROM (

  ​	SELECT DISTINCT userId, sessionId,ts,channel

  ​	FROM adhoc.jinhyo_session_summary

  ​	);

  - 중복이 없다면 위 두 개의 코드가 같은 결과를 내어야 함

- 최근 데이터의 존재 여부

  SELECT MIN(ts),MAX(ts)

  FROM adhoc.jinhyo_session_summary;

- 값이 비어있는 컬럼들이 있는지 체크하기

  SELECT

  ​	COUNT(CASE WHEN sessionid is NULL THEN 1 END) sessionid_null_count,

  ​	COUNT(CASE WHEN userId is NULL THEN 1 END) sessionid_null_count,

  ​	COUNT(CASE WHEN ts is NULL THEN 1 END) sessionid_null_count,

  ​	COUNT(CASE WHEN channel is NULL THEN 1 END) sessionid_null_count,

  FROM adhoc.jinhyo_session_summary;
