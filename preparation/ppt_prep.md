0. 현재 상황 및 목표 : 앱 개발을 앞두고, 데이터분석으로 성공적인 product전략을 도출하고자 함

   

1. 문제 정의 : 앱 인스톨 횟수에 영향을 주는 유의미한 요소들을 파악하고자 함

   

2. 데이터 수집 kaggle에서 수집된 데이터를 토대로 9,xxx개 표본의 14개 특징데이터 추출 및 이를 개량

   

3. 데이터 처리 및 분석

   1. 처음 수집한 데이터의 형태가 대부분 object type

   2. 분석 불가하다 판단 - 숫자로 이루어진 데이터를 모두 float64 및 int64로 변환

   3. Install에 영향을 주는 유의미할 수 있을 것 같다 판단한 feature 선별 - Review, Rating, Price

   4. 하지만 correlation 결과는 낮음.

      ```
      The following are the top 9 strongly correlated values with Installs:
      reviews         0.643121
      And_1st_Ver     0.229019
      Ver_Year        0.089378
      Rating          0.084611
      Ver_Mon         0.056445
      Size            0.007329
      And_Last_Ver    0.000174
      Price          -0.011691
      Type           -0.051043
      Name: Installs, dtype: float64
      ```

   5. 그래서 위 feature들과 다른 feature들과의 상관관계 유무 여부 확인 - 하지만 큰 correlation을 찾지 못하여 데이터 정리를 추가로 진행하기로 함.

      ![image-20200508114725938](C:\Users\Gk\AppData\Roaming\Typora\typora-user-images\image-20200508114725938.png)

   6. Install의 range가 너무 크다 판단하여, log10을 실행하기로함

   7. unique 값이 20개가 있었고, 0은 log10을 취할 시 -inf가 되므로 0으로 통일

   8. 후 상관관계 재확인하였으나, correlation이 심지어 떨어짐.

      ```
      The following are the top 9 strongly correlated values with Installs_log:
      Rating          0.586801
      And_1st_Ver     0.325531
      reviews         0.254668
      Ver_Mon         0.103135
      Ver_Year        0.091992
      And_Last_Ver    0.084122
      Size           -0.000768
      Price          -0.046416
      Type           -0.237945
      Name: Installs_log, dtype: float64
      ```

   9. Install(log10)의 더 이상의 분석은 무의미하다 판단 - 다른 요소들 분석 시작

   10. Price에 대하여 유료/무료, 이상한 $250 app 제거 (outlier), 또는 대부분의 app이 모여있는 range (under $10) 등의 분석을 실행했지만, 유의미한 correlation은 찾지 못함.

       ![image-20200508115430793](C:\Users\Gk\AppData\Roaming\Typora\typora-user-images\image-20200508115430793.png)

       ```
       The following are the top 9 strongly correlated values with Price:
       Type            0.220601
       Size            0.017400
       And_Last_Ver    0.002625
       Ver_Year        0.002134
       Rating          0.000339
       Ver_Mon         0.000189
       reviews        -0.009271
       And_1st_Ver    -0.015473
       Installs_log   -0.046416
       Name: Price, dtype: float64
       ```

       ```
       The following are the top 9 strongly correlated values with Price under $250 (without Outlier):
       Type            0.498002
       Size            0.012249
       Ver_Mon         0.008285
       And_Last_Ver    0.005860
       Ver_Year        0.003770
       reviews        -0.020419
       Rating         -0.031025
       And_1st_Ver    -0.034287
       Installs_log   -0.129088
       Name: Price, dtype: float64
       ```

       ```
       The following are the top 9 strongly correlated values with Price under $10:
       Type            0.823270
       Size            0.033029
       And_Last_Ver    0.009253
       Ver_Year        0.005846
       Ver_Mon        -0.003651
       Rating         -0.009730
       reviews        -0.031616
       And_1st_Ver    -0.047043
       Installs_log   -0.168328
       Name: Price, dtype: float64
       ```

   11. Review와 Install_log의 상관관계를 확인

       1. Review 또한 range가 컷기 때문에 log10 실행

       2. 후 상관관계 분석

          ![image-20200508120111368](C:\Users\Gk\AppData\Roaming\Typora\typora-user-images\image-20200508120111368.png)

          ![image-20200508120326086](C:\Users\Gk\AppData\Roaming\Typora\typora-user-images\image-20200508120326086.png)

       3. 