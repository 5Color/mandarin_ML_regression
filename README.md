# mandarin_ML_regression

데이콘 사이트 -> https://dacon.io/
감귤착과량 예측 경진대회 -> https://dacon.io/competitions/official/236038/overview/description

이번에는 데이콘 대회중 하나인 감귤착과량 에측 프로젝트를 진행할것이다
우선 필요한 라이브러리로는 [ pandas, numpy, sklearn와  tree에 있는 DecisionTreeRegressor, random, os, supervised.automl(AutoML)]
 이런것들이 필요하다. 해당 라이브러리를 import하고, [train.csv, test.csv, sample_submission.csv [제출양식]) 다운로드 하여 데이터셋을 불러온다.
 데이터를 확인했더니 2207 rows × 184 columns의 행과 열이 나온것을 확인할 수 있다.
 
1. 첫번째로 진행할것은 X_train과 y_train학습 독립변수를 만든다.  X는 train데이터셋에서 ID, 착과량(int) column을  drop함수를 이용하여 없애고, y는 train데이터셋에서 '착과량(init)'column만 가져와 종속변수로 만든다.
2. scikit-learn의 from sklearn.tree import DecisionTreeRegressor를 import하여 모델을 생성하고 X_train과 y_train을 fitting한다.
3. 그 다음 test  데이터셋도 같이 전처리 해주는데 ID컬럼만  drop시켜 test변수에 다시 저장하고, 기존에 fitting하여 학습이 완료된
   모델을 사용해 모든 test데이터를 predict해준다.
4. sample_submisson = pd.read_csv("./sample_submission.csv") 대회 제출양식도 똑같이 pandas의 .read_csv()메소드를 이용해 불러오고,
   sample_submisson의 착과량(int)컬럼에 예측했던 저장된 pred변수를 넣어준다. 그리고 .to_csv.메소드로 파일을 'submit.csv'로 저장해준다.
5. 이제 학습을 하기전에 파생변수를 만들 데이터를 정제할것이다.  가끔 파생변수라는것이 학습에 도움이 될때가 있기 때문이다.
   파생변수란 데이터에서 의미있는 통계를 내서 새로운 컬럼변수에 저장하는것이다. train_df, test_df라는 변수명으로 train.csv와 test.csv     를 불러오고,
   전처리를 해준다.
   5-1. target_columns변수 생성하여 데이터에서 새순이라는 단어가 포함되어있는 컬럼만 걸러주는 코드 target_columns =                           train_df.filter(regex="새순").columns를 실행하기                 
   5-2. 새순mean이란 컬럼변수를 만들어 train_df의 target_columns컬럼의 (axis=1 <-가로방향)데이터를 평균내주기
   5-3. 새순std이란 컬럼변수를 만들어 train_df의 target_columns컬럼의 (axis=1 <-가로방향)데이터를 표준편차내주기
   5-4. 새순diff이란 컬럼변수를 만들어 train_df의 ["2022-11-28 새순"]에서 ["2022-09-01 새순"]을 빼준다.
