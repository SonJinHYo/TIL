# Image Classification



### Autometed plant species identification-Trends and future directions

- ![image-20221206153213752](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206153213752.png)

- ![image-20221206153419165](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206153419165.png)

  - 위와 같이 클래스 구분이가능하다

- 하지만 다음과 같이 시간에 따라 특징이 변하기 때문에 분류에 어려움이 있음

  ![image-20221206153540039](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206153540039.png)

  - 잎 : 연중 대부분 기간이 유효, 보존이 쉬움, 평평한 특성으로 이미지화
  - 꽃 : 연중 짧은 기간 유효, 빛과 날씨 등에 따라 다양해짐, 잎에 비해 3D이미지를 가짐

- DenseNet  - Residual Net처럼 잔차를 이용하여 Dense Block을 만듬

  - Resiudual net에선 잔차를 더했지만 DenseNet에선 concatenate를 한다.
  - ![image-20221206154054090](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206154054090.png)



### EfficientNet

- Model scailing
  - width scailing : 신경망을 넓게 만듦 : channel 사이즈 증가
  - depth scailing : 신경망을 깊게 만듦
  - resolution scailing : resolution을 늘림
  - compound scailing : 위 세 가지를 전부 통합함

![image-20221206154535543](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206154535543.png)

- 더 깊을수록, 더 넓을수록, 더 높은 해상도일수록 정확도가 향상

  - 세 가지 스케일링을 적절히 조합하는 것이 높은 성능으로 이끌 수 있다.

  - 모델이 커질수록 정확도가 포화된다고 해석 가능

    ![image-20221206154618461](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206154618461.png)

    - (FLOPS : 계산량)

- ![image-20221206154725535](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206154725535.png)

  ![image-20221206154751020](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206154751020.png)





# Image Colorization

### Saeed Anwar, et al, Image Colorization : A Survey and Dataset

#### Image Colorization

- 색 정보가 없는 사진들에 색을 입히는 이미지 향상의 한 분야

  <img src="https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206155210234.png" alt="image-20221206155210234" style="zoom:50%;" />

- Challenges

  - 단일 알고리즘으로 다뤄져야 할 이미지 상태들이 너무 다양함
  - 사전 지식으로만 색을 입히기에는 한계가 명확 (복원x, 자연스러운 채색o)
  - 이미지 향상의 전형적인 challendge들을 상속받음



#### Category

- **Plain Networks**

  - 기준 : 여러 개의 컨볼루션층이 있는 간단한 모델들이여야 함 (skip connection이 없거나 naive한 skip만 있는 정도)

    ![image-20221206155642917](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206155642917.png)

  - ex) COlorful Colorization

    ![image-20221206155824589](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206155824589.png)

- **User-Guided Networks**

  - 사용자의 인풋을 필요로 하는 방법

    ![image-20221206155903217](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206155903217.png)

  - ![image-20221206155925923](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206155925923.png)

- **Domain-Specific Networks**

  - 특수한 이미지에 색을 입히는 모델들

  - 적외선 이미지, 레이더 이미지 등

    ![image-20221206160223961](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206160223961.png)

- **Text-based Networks**

  - 텍스트 인풋에 기반해 색을 입히는 모델들

  - ![image-20221206160534086](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206160534086.png)

    ex. Text2Colors

    ![image-20221206160603662](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206160603662.png)

- **Diverse Colorization Networks**

  - 다양한 스타일의 컬러화된 이미지를 생성하는 모델들 (주로 GAN)

  - ![image-20221206160721588](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206160721588.png)

    ![image-20221206160744286](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206160744286.png)

  

- **Multi-Path Networks**

  - 특징을 다른 수준/경로로 배우기 위해 다른 부분을 따르는 네트워크
  - ![image-20221206161126029](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206161126029.png)
  - ![image-20221206161143627](https://raw.githubusercontent.com/SonJinHYo/image_repo/main/image_server/image-20221206161143627.png)

- Exemplar-based Colorization Network
