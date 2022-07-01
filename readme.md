# TFT_Winning_Rate_Calculator
<img src="https://post-phinf.pstatic.net/MjAyMDAzMDRfMTQ0/MDAxNTgzMjc4MDY2MTk1.ohy8FPuqnaVnQ2j62Oy99IzNDLSAxJrApamMqQm6tekg.s-S8C9hbihqFXVkTYWR9dvvT_QlAp0HwqHfD7cC76zYg.JPEG/1-960x540.jpg" width="480" height="270">

#### 1. 프로젝트 개요 : 전략적 팀 전투 승률 계산기
8명이 진행해 순위를 가리는 Riot Games의 게임인 **전략적 팀 전투**(이하 TFT)에서,
  1. 현재 플레이어의 '챔피언'이 가진 '특성'과 '계열'의 조합을 토대로 **승패를 예측**할 수 있을 것이며 이를 통해 더 나은 승률을 얻을 수 있을 것이다.  
  2. 이를 통해 사이트 유저를 모을 수 있을 것이며 이용자가 생기면 이를 통해 배포 페이지 내 광고 삽입 등의 방법으로 수익을 얻을 수 있을 것이다.  
라는 가설을 세우고 프로젝트를 진행함
- 데이터는 Riot Games의 개발자 API를 활용해 게임의 기록을 확보하였으며, 이 기록에는 각 플레이어의 최종 챔피언 구성과 특성 및 계열, 골드 등의 정보가 드러나있음. 이를 MongoDB에 적재한 후 사용함

#### 2. 프로젝트 진행 과정
![image](https://user-images.githubusercontent.com/89769294/176881566-95d4b0e6-309a-4472-b9a3-b2434f0d3e2e.png)
데이터 크롤링 - 데이터베이스에 저장 - 전처리 - 머신 러닝을 활용해 API 서비스 개발 - 대시보드 제작 - 배포
- **데이터 크롤링**
  - **API**를 사용해 , 본인과 같이 게임을 진행한 적이 있는 게이머 ID 정보를 확보하고 이를 통해 매치 리스트를 확보함
 
- **데이터베이스에 저장**
  - **pymongo**를 활용해 크롤링한 데이터를 **MongoDB**에 적재함
  
- **전처리**
  - json 형태의 데이터를 바로 적재한 것이기 때문에 처리하기 불편함
  - 따라서 정보를 불러와 원하는 것만을 편히 볼 수 있게 전처리 후 데이터프레임 형태로 변경 
  
- **머신 러닝을 활용해 API 서비스 개발**
  - 기준 모델(분류 문제이므로 최빈값) 설정 후 Decision Tree, Random Forest, XGBClassifier, LGBMClassifier을 사용해 학습
  - 가장 점수가 높았던 **LGBM** 모델을 사용하기로 결정
  
- **대시보드 제작**
  - 빠르고 간편하게 사용할 수 있어서 [Google Data Studio](https://datastudio.google.com/s/gBCHrmJ5XGE)를 사용함

- **배포**
  - 간단한 구조이기 때문에 **heroku**를 사용해 진행함

#### 3. 결론
  1. 현재 플레이어의 '챔피언'이 가진 '특성'과 '계열'의 조합을 토대로 **승패를 예측**할 수 있을 것이며 이를 통해 더 나은 승률을 얻을 수 있을 것이다.  
  → **예측할 수 있으며 더 나은 승률을 얻을 수 있음**
  2. 이를 통해 사이트 유저를 모을 수 있을 것이며 이용자가 생기면 이를 통해 배포 페이지 내 광고 삽입 등의 방법으로 수익을 얻을 수 있을 것이다.  
  → **프로젝트의 규모상 유저를 모으지 않았으나 유사한 전적 검색 사이트에서 배너 광고를 유지하는 것을 통해 그럴 것으로 추정할 수 있음**

#### 4. 파일 구성
폴더 pycache : python파일을 사용하면 생기는, 더 빠르게 실행하는 것을 도와주는 폴더  
폴더 static : templates를 꾸며주는 역할을 하는 폴더  
폴더 templates : 웹을 통해 들어올 때 보여주는 모습이 적힌 폴더  
**1_get_raw_data.py** : API를 활용해 정보를 확보하고 데이터베이스에 적재하는 파일  
**2_eda.py** : 적재된 데이터를 사용하기 쉽게 EDA를 거치게 하는 파일  
**3_ml.py** : 머신 러닝 모델 학습을 실행하는 파일  
**4_app.py** : 배포 과정에서 실행하기 위한 파일  
Procfile : heroku를  사용하기 위한, 명령어가 기록되어 있는 파일  
df.csv : 적재된 데이터를 EDA를 거치고 데이터프레임 형식으로 변경한 파일  
knn_model.pkl : 3_ml.py에서 가장 우수한 결과를 보였던 LGBM 모델의 학습을 저장한 파일  
requirements.txt : heroku를 사용하기 위한, 환경변수가 기록된 파일  

#### 5. 프로젝트 진행 기간
21.12.08 ~ 21.12.13

#### 6. 프로젝트 과정에서 느낀 것
- **많은 에러**
    - 과정을 진행하며 기존까지 진행한 두 프로젝트보다 수많은 에러를 마주함
    - 이로 인해 Docker를 사용한 배포, 스케쥴링, 만들어낸 예측 결과를 다시 데이터베이스에 넣기 등의 심화 수준 목표는 달성하지 못함
      - 에러 자체를 막을 수는 사실상 없기 때문에 이후부턴 '왜 에러가 발생했는지' 고민하지 않고 '일단 찾아내자'에 집중함

- **기획의 중요성**  
    - 지금껏  해온 프로젝트보다 복잡도와 구현해야 할 것이 증가하며 기획 단계에서 힘을 쏟음
    - 이로 인해 핵심 기능을 구현할 수 있었고 그러지 않았더라면 많았던 에러를 고치느라 핵심 기능마저 구현하지 못했을 것이란 생각이 들었음
      - 앞으로도 기획의 비중을 높게 가져가는 것이 더 나은 결과를 만들 것이라 판단, 유지하기로 
