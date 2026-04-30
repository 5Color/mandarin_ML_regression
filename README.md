# mandarin_ML_regression

<img width="1218" height="771" alt="image" src="https://github.com/user-attachments/assets/74ab6d29-c1b7-4568-a2e0-a2cc31130506" />

## 데이콘 사이트 -> https://dacon.io/

## 감귤착과량 예측 경진대회 -> https://dacon.io/competitions/official/236038/overview/description

이번에는 데이콘 대회 중 하나인 감귤착과량 예측 프로젝트를 진행할 것이다.

### import

우선 필요한 라이브러리로는 `pandas`, `numpy`, `sklearn`, `DecisionTreeRegressor`, `random`, `os`, `supervised.automl(AutoML)` 등이 있다.

### DataLoad

해당 라이브러리를 import하고, `train.csv`, `test.csv`, `sample_submission.csv` 제출 양식을 다운로드하여 데이터셋을 불러온다.

데이터를 확인했더니 `2207 rows x 184 columns`의 행과 열이 나온 것을 확인할 수 있다.

### 변수 설정

1. 첫 번째로 진행할 것은 `X_train`과 `y_train` 학습 변수를 만드는 것이다. `X`는 train 데이터셋에서 `ID`, `착과량(int)` column을 `drop` 함수를 이용하여 없애고, `y`는 train 데이터셋에서 `착과량(int)` column만 가져와 종속변수로 만든다.

### 1번째 fitting(학습)

2. `sklearn.tree`의 `DecisionTreeRegressor`를 import하여 모델을 생성하고 `X_train`과 `y_train`을 fitting한다.

3. 그 다음 test 데이터셋도 같이 전처리해준다. `ID` 컬럼만 `drop`시켜 `test` 변수에 다시 저장하고, 기존에 fitting하여 학습이 완료된 모델을 사용해 모든 test 데이터를 predict해준다.

4. `sample_submission = pd.read_csv("./sample_submission.csv")`로 대회 제출 양식도 똑같이 pandas의 `.read_csv()` 메소드를 이용해 불러온다. 이후 `sample_submission`의 `착과량(int)` 컬럼에 예측했던 `pred` 변수를 넣어주고, `.to_csv()` 메소드로 파일을 `submit.csv`로 저장해준다.

### 2번째 학습(AutoML)

5. 이제 학습을 하기 전에 파생변수를 만들 데이터를 정제할 것이다. 가끔 파생변수라는 것이 학습에 도움이 될 때가 있기 때문이다.

파생변수란 데이터에서 의미 있는 통계를 내서 새로운 컬럼 변수에 저장하는 것이다. `train_df`, `test_df`라는 변수명으로 `train.csv`와 `test.csv`를 불러오고, 전처리를 해준다.

5-1. `target_columns` 변수를 생성하여 데이터에서 `새순`이라는 단어가 포함되어 있는 컬럼만 걸러주는 코드 `target_columns = train_df.filter(regex="새순").columns`를 실행한다.

5-2. `새순mean`이라는 컬럼 변수를 만들어 `train_df`의 `target_columns` 컬럼의 데이터를 평균낸다. `(axis=1 <- 가로 방향)`

5-3. `새순std`라는 컬럼 변수를 만들어 `train_df`의 `target_columns` 컬럼의 데이터를 표준편차낸다. `(axis=1 <- 가로 방향)`

5-4. `새순diff`라는 컬럼 변수를 만들어 `train_df`의 `2022-11-28 새순`에서 `2022-09-01 새순`을 빼준다.

5-5. test 데이터도 위 train 데이터의 과정을 따라해준다.

<img width="729" height="401" alt="image" src="https://github.com/user-attachments/assets/463d7c17-cf81-4efe-bad0-60156e8329b2" />

6. `X`, `y` 변수를 설정해주고, AutoML을 돌려 학습을 진행시켜준다. 여기서 모드는 `mode="Compete"`, 평가지표는 `rmse`로 할 것이다.

7. 여러 AutoML 결과 중 `AutoML_3`의 `Ensemble_Stacked`가 가장 좋은 결과를 보였다.

8. 이제 대회에 파일을 제출하기 위한 `submission` 변수에 `sample_submission.csv` 데이터를 불러와주고, `착과량(int)` 자리에 `pred`를 넣어준다.

9. `Ensemble_Stacked_trained`의 이름으로 양식을 제출해주면 된다.

이제 점수를 확인해보자.

`0.07276`점이 나왔다.

1등과 별 차이가 없는 게, 1등은 `0.07118`이다.
