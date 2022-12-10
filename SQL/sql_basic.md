# SQL과 데이터베이스 Basic

#### 데이터 관련 직군 (SQL 필요성)

데이터 직군에서는 SQL이 **필수**

- 데이터 엔지니어
  - 파이썬, 자바/스칼라
  - **SQL**, 데이터베이스
  - ETL/ ELT (Airflow, DBT)
  - Spark, Hadoop
- 데이터 분석가 (특히)
  - **SQL**, 비지니스 도메인 지식
  - 통계 (AB 테스트 분석)
- 데이터 사이언티스트
  - 머신러닝
  - **SQL**, 파이썬
  - 통계



## 관계형 데이터베이스

- **구조화된 데이터**를 저장하고 질의할 수 있도록 해주는 스토리지

  - 엑셀 스프레드시트 형태의 테이블로 데이터를 정의하고 저장
    - 테이블 - columns 과 recored(행) 이 존재 (합쳐서 Table Schema라 부르기도 함)

- 관계형 데이터베이스를 조작하는 프로그래밍 언어 - **SQL**

  - 테이블 정의를 위한 **DDL** (Data Definition Language)
  - 테이블 데이터 조작/ 질의를 위한 **DML** (Data Manipulation Language)

- 대표적 관계형 데이터베이스

  - **프로덕션 데이터베이스** : MySQL, PostgreSQL, Oracle

    - OLTP 라 부름 (OnLine Transaction Preocessing)
    - 빠른 속도에 집중. 서비스에 필요한 정보 저장

  - **데이터 웨어하우스** Redshift, Snowflake, BigQuery, Hive

    - OLAP라 부름 (OnLine Analytical Processing)

    - 처리데이터 크기에 집중. 데이터 분석 혹은 모델 빌딩등을 위한 데이터 저장

      - 보통 프로덕션 데이터베이스를 복사해서 데이터 웨어하우스에 저장

      

  - 만약 회사에 프로덕션 데이터베이스만 있다면? - 큰 쿼리를 날려서 속도가 느려지면서 백엔드 개발자와의 트러블 발생

    - 그래서 별도의 데이터 웨어하우스가 필요 (데이터 웨어하우스 덕분에 서비스 속도에 영향을 안주니까)



- 관계형 데이터베이스의 구조

  - 구조순서

    1. 가장 밑단에는 테이블들이 존재 (엑셀의 시트에 해당)

    2. 테이블들은 데이터베이스(or 스키마)라는 폴더 밑으로 구성

    <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221209124601362.png" alt="image-20221209124601362" style="zoom:50%;" />

    - ㄴ ex
      - raw_data : 외부 데이터 폴더/ analytics : 분석한 데이더 폴더  /. . . . .
        - patient , patientData,  . . . : raw_data 폴더가 가진 테이블
        - patinetSummary, vitalSummary,  . . . : analytics 폴더가 가진 테이블

  - 테이블의 구조 (or 테이블 스키마)

    - 테이블은 레코드들로 구성 (레코드 = 행)

    - 레코드는 하나 이상의 필드(컬럼)로 구성

    - 필드는 이름과 타입과 속성(primary key)으로 구성됨

      <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221209125427279.png" alt="image-20221209125427279" style="zoom:67%;" />

  

  

## SQL (Strucured Query Language)

관계형 데이터베이스에 있는 테이블을 질의하거난 조작해주는 언어

#### 종류

- DDL (Data Definition Language)
  - 테이블의 구조를 정의하는 언어
- DML (Data Manipulation Language)
  - 테이블에서 원하는 레코드들을 읽어오는 질의 언어
  - 테이블에 레코드를 추가/삭제/갱신해주는데 사용하는 언어

#### 빅데이터에서의 SQL 중요성

- 구조화된 데이터를 다루는한 SQL은 데이터 규모와 상관없이 쓰임
- 모든 대용량 데이터 웨어하우스는 SQL 기반
  - Redshift, Snowflake, BigQuery, Hive
- Spark이나 Hadoop도 예외가 아님
  - SparkSQL과 Hive라는 SQL 언어가 지원됨
- 데이터 분야에서의 기본 기술

#### SQL의 단점

- 구조화된 데이터를 다루는데 최적화가 되어있음
  - 정규표현식을 통해 비구조화된 데이터를 어느 정도 다루는 것은 가능하나 제약이 심함
  - 많은 관계형 데이터베이스들이 플랫한 구조만 지원함
    - 구글 빅쿼리는 nested structure를 지원함
  - 비구조화된 데이터를 다루는데 Spark Hadoop과 같은 분산 컴퓨팅 환경이 필요해짐
    - 즉 SQL만으로는 비구조화 데이터를 처리하지 못함
- 관계형 데이터베이스마다 SQL 문법이 조금씩 상이

#### Star schema

- Production DB용 관계형 데이터베이스에서는 보통 스타 스키마를 사용해 데이터를 저장

- 데이터를 논리적 단위로 나눠 저장하고 필요시 조인. 스토리지 낭비가 덜하고 업데이트가 쉬움

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221209172008975.png" alt="image-20221209172008975" style="zoom:80%;" />

#### Denormalized schema

- 데이터 웨어하우스에서 사용하는 방식

  - 단위 테이블로 나눠 저장하지 않음으로 별도의 조인이 필요없는 형태를 말함

- 이는 스토리지를 더 사용하지만 조인이 필요 없기에 빠른 계산이 가능

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221209172405193.png" alt="image-20221209172405193" style="zoom: 80%;" />
