---
layout: single
title: "Intro to Machine Learning(4)"
categories: mldl
tag: [Artificial Intelligence, Kaggle, Kaggle Courses, Machine Learning, 기계학습, 머신러닝, 머신러닝기초, 인공지능, 인공지능기초, 캐글]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 모델 검증(Model Validation)

모델의 퍼포먼스를 측정해 대안을 테스트하고 비교할 수 있습니다.





일단 모델은 만들어봤죠. 그런데, 얼마나 좋은 모델인가요?



이번 시간에는 당신의 모델의 품질을 측정해서 모델 검증하는 법을 배울 것입니다. 모델의 품질을 측정하는 것은 당신의 모델을 계속해서 발전시키는 열쇠입니다.





### 모델 검증이란 무엇인가?

당신은 지금껏 당신이 만든 거의 모든 모델을 평가하고 싶을 것입니다. 대부분(전부는 아니지만)의 경우, 모델의 품질과 관련된 수치는 예측 정확도입니다. 다시 말해, 모델의 예측이 실제 값과 얼마나 가까운지가 중요하다는 것입니다.



많은 사람들은 예측 정확도를 측정할 때 큰 실수를 합니다. 그것은 바로 그들이 *학습 데이터*로 예측을 만들고, 그 *학습 데이터* 안에 있는 타겟 값(target values)과 그 예측을 비교한다는 것입니다. 이 접근법의 문제를 잠시 후에 살펴볼 것이지만, 그전에 먼저 어떻게 이것을 하는지 생각해봅시다.



먼저, 모델의 품질을 이해할 수 있는 방법으로 요약해야 합니다. 만약 당신이 1만개의 주택의 예측 가격과 실제 가격을 비교한다면, 좋은 예측값들과 나쁜 예측 값들이 섞여있는 것을 발견할 확률이 높습니다. 이 리스트를 뒤적이는 것은 무의하겠죠. 따라서, 이것을  하나의 지표로 요약할 필요가 있습니다.



여기에는 모델의 품질을 요약할 많은 지표들이 있겠지만, 우리는 먼저 **평균 절대 오차(Mean Absoulte Error)**라고 불리는 것부터 살펴보겠습니다. 마지막 단어인 오차(error)부터 분석해보죠.



각 주택에 대한 예측 오차는 다음과 같습니다.

```
error = actual - predicted
```





자, 만약 주택 값이 $150,000이고 예측 값이 $100,000이라면 오차는 $50,000이 되겠습니다.



평균 절대 오차(MAE) 지표로 각 오차의 절댓값을 얻을 수 있습니다. MAE는 각 오차를 양수로 변환합니다. 이제, 우리는 이  절대값 오차들의 평균을 얻을 수 있습니다. 이것이 모델 품질에 대한 우리의 측정치입니다. 일반영어로는 이렇게 표현할 수 있습니다.

> On average, our predictions are off by about X.
> 평균적으로 우리의 예측은 대략 X로 빗나갔다.

평균 절대 오차(MAE)를 계산하기 위해서는 먼저 모델이 하나 필요합니다. 아래에는 해당 코드입니다.

```
# 여기에는 데이터를 불러오기 코드가 숨겨져있다고 가정
import pandas as pd

# 데이터 가져오기
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path)

# 결측치 걸러내기
filtered_melbourne_data = melbourne_data.dropna(axis=0)

# 타겟과 특성에 해당하는 열 선택
y = filtered_melbourne_data.Price
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'BuildingArea',
						'YearBuilt', 'Lattitude', 'Longtitude']
X = filtered_melbourne_data[melbourne_features]


from sklearn.tree import DecisionTreeRegressor

# 모델 정의
melbourne_model = DecisionTreeRegressor()

# 모델 학습(Fit)
melbourne_model.fix(X, y)
```



일단 모델이 있으면, 아래와 같이 평균 절대 오차(MAE)를 계산합니다.

```
from sklearn.metrics import mean_absolute_error

predicted_home_prices = melbourne_model.predict(X)

mean_absolute_error(y, predicted_home_prices)
```



### "표본 내(In-Sample)" 점수의 문제

우리가 방금 산출한 측정치는 **표본 내In-Sample** 점수라고 부릅니다. 우리는 모델을 만드는 것과 평가하는 것에 대해 동일한 표본을 사용했습니다. 이것이 왜 평균 절대 오차(MAE)가 나쁜지를 보여주는 대표적인 예시입니다.



거대한 실제 부동산 시장에서, 문의 색깔이 주택 가격과 관련되지 않았다고 상상해보겠습니다.

그런데 당신이 모델을 만들 때 사용한 데이터의 표본에서는 초록색 문이 있는 모든 집이 굉장히 비쌌습니다. 모델의 임무는 주택 가격을  예측할 수 있는 패턴을 찾는 것이기에 모델은 이 패턴을 볼 것이고, 초록 문을 가진 주택을 항상 비싸게 예측할 것입니다.



이 패턴이 학습 데이터로부터 유래된 것이기 때문에, 모델은 학습 데이터에서는 정확해보일 것입니다.

하지만 만약 이 패턴이 새로운 데이터에서 유지되지 않는다면, 모델은 실제로 사용할 때 매우 부정확할 것입니다.



모델들의 실용적인 가치는 새로운 데이터의 예측치를 구하는 것이기 때문에 모델을 만들 때 사용되지 않은 데이터에 대해서 성능을 측정합니다. 이에 대한 가장 간단한 방법은 모델 구축 단계에서 약간의 데이터를 제외시키고, 그 데이터를 모델의 새로운 입력 데이터에 대한  정확도를 평가할 때 사용하는 것입니다. 이렇게 제외시키는 데이터를 **검증 데이터(Validation data)**라고 부릅니다.



### 코딩해봅시다!

scikit-learn 라이브러리는 데이터를 두 조각으로 쪼개기 위한 train_test_split이라는 기능을 가지고 있습니다. 이렇게 쪼갠 데이터 중 하나는 학습 데이터로 모델을 학습할 때 사용하고, 다른 하나는 검증 데이터로 평균 절대  오차(mean_absolute_error)를 계산할 때 사용합니다. 

```
from sklearn.model_selection import train_test_split

# 타겟과 특성에 해당하는 데이터를 각각 학습용과 검증 데이터로 나눕니다.
# 나누는 것은 난수 생성기를 기반으로 합니다.
# 따라서, random_state 인자에 숫자를 넣으면 매번 스크립트를 실행할 때마다
# 데이터를 동일하게 나누는 것을 보장할 수 있습니다.
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 0)

# 모델 정의
melbourne_model = DecisionTreeRegressor()

# 모델 학습
melbourne_model.fit(train_X, train_y)

# 검증 데이터로부터 예측 가격 얻기
val_predictions = melbourne_model.predict(val_X, val_y)
print(mean_absolute_error(val_y, val_predictions))
```



### Wow!

이전에 표본 내 데이터로 평균 절대 오차를 구했을 때는 약 $500였는데, 표본 외 데이터로 구하니 $250,000네요.



이것이 거의 정확히 맞는 모델과 대부분의 실용적 목적에서 쓸모없는 모델의 차이입니다. 참고로 검증 데이터의 주택 가격의 평균은 $1,100,000입니다. 따라서, 새 데이터의 오차는 평균 주택 가격의 거의 1/4입니다.



이 모델을 향상시키는 것에는 더 나은 특성을 찾기위해 실험을 한다거나, 다른 모델 타입을 쓰는 등 여러 방법이 있을 겁니다.

------

이렇게 Intro to Machine Learning의 네번째 수업을 번역하며 공부해봤습니다.



혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다!



출처 : [https://www.kaggle.com/dansbecker/model-validation﻿](https://www.kaggle.com/dansbecker/model-validation)
