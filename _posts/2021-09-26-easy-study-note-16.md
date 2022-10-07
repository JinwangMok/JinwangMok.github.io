---
layout: single
title: "Intro to Machine Learning(7)"
categories: mldl
tag: [Kaggle, Kaggle Courses, Machine Learning, 기계학습, 머신러닝, 머신러닝기초, 인공지능, 인공지능기초, 캐글, 캐글 코스 번역]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## Machine Learning Competitions

당신의 성장을 확인하고 계속 발전하기 위해 기계 학습 시합의 세계에 들어오세요!



기계 학습 시합은 당신의 데이터 사이언스 기술과 당신의 진척도를 평가하기에 가장 좋은 방법입니다.



다음 실습으로 당신은 [캐글 학습 유저들을 위한 주택 가격 시합](https://www.kaggle.com/c/home-data-for-ml-course)에 예측치를 만들고 제출하는 것입니다.



이것으로 Intro to Machine Learning 코스를 마치겠습니다.

------

이렇게 Intro to Machine Learning의 모든 수업을 번역하며 공부해봤습니다.



결국 이 코스를 요약하자면,

- scikit-learn이라는 머신러닝 라이브러리를 활용해본다.
- decision tree와 random forest를 사용해본다.
- 만들어진 모델이 잘 동작하는지에 대한 기준을 MAE로 계산해서 모델의 품질을 평가해본다.
- 이를 반복하면 더 나은 모델을 만들어 갈 수 있겠다는 개념을 깨닫는다.

이 정도인 것 같습니다.

또한, 라이브러리를 쓰는 순서를 제 나름 정리해서 코드로 대충 나타내보자면 아래와 같습니다.

```python
# 사용할 모델과 라이브러리 import
import pandas as pd
from sklearn.model_selection import train_test_split
...

# 데이터 읽어오고, (선택적으로) 결측치 제거
file_path = '../...'
my_data = pd.read_csv(file_path)
# my_data = my_data.dropna(axis=0)

# X, y에 해당하는 데이터 선정하고, (학습에 용이하게) train과 validation용으로 구분
my_features = ['column name1', 'column name2',...]
X = my_data[my_features]
y = my_data.target_column
train_X, val_y, train_y, val_y = trian_test_split(X, y, random_state = 0)
														#random_state = 난수제어

#모델 정의 및 학습
my_rf_model = RandomForestRegressor(random_state = 0)
my_rf_model.fit(train_X, train_y)

# 모델 예측 및 평균 절대 오차 측정
my_prediction = my_rf_model.predict(val_X)
my_mae = mean_absolute_error(val_y, my_prediction)

# 이후 이를 가지고 모델 개선
```

머신러닝에 대해 전반적으로 이해할 수 있는 좋은 강의였던 것 같습니다. 번역하면서 공부하니 시간은 좀 걸렸지만, 재밌네요😆



이후 exercise를 통해 competition에 처음 참가해봤는데 약간 감이 잡히는 것 같습니다.

근데, 리더보드를 보니까 MAE가 0인 분들도 있더라구요..?? 외계인 고문하신건가..??



혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다!



출처 : https://www.kaggle.com/alexisbcook/machine-learning-competitions
