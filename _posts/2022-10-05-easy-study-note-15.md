---
layout: single
title: "Intro to Machine Learning(6)"
categories: mldl
tag: [Kaggle, Kaggle Courses, Machine Learning, Random Forest, 기계학습, 머신러닝, 머신러닝기초, 인공지능, 인공지능기초, 캐글]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 랜덤 포레스트(Random Forests)

더 세련된 기계학습 알고리즘을 사용해봅시다.





### 소개

의사 결정 트리는 당신에게 어려운 결정을 남겨줍니다. 잎사귀(leaves)가 많아 깊이가 깊은 트리는 각 예측이 단지 몇 개의  주택으로만 얻은 역사적인 데이터로 된 것이기 때문에 과적합(Overfitting)이 일어날 것입니다. 또, 잎사귀(leaves)가 적어 깊이가 얕은 트리는 행 데이터들로부터 패턴을 찾기 위한 여러 구분점들을 포착하는 것에 실패해서 품질이 형편없을 것입니다.  과소적합(Underfitting)이 일어나는 것이지요.



최근의 가장 세련된 모델링 기법일지라도, 과적합과 과소적합 사이의 이 긴장감을 마주합니다. 하지만, 많은 모델들이 더 나은 품질을  끌어내기 위한 영리한 아이디어들을 가지고 있습니다. 우리는 그 예시로 랜덤 포레스트(random forest)를 살펴보겠습니다.



랜덤 포레스트는 많은 트리를 사용하고, 각 구성 트리의 예측치의 평균으로 예측을 만들어냅니다. 랜덤 포레스트는 일반적으로 하나의  의사결정 트리보다 훨씬 나은 예측 정확도를 가지고 있고, 기본 매개변수들로도 잘 작동합니다. 만약 당신이 계속 모델링을 한다면 더 좋은 품질을 가진 여러 모델들을 배울 수 있을 것이지만, 이것들 중 대부분은 적절한 매개변수를 얻어내기 까다롭습니다.



### 예시

당신은 이미 여러번 데이터를 가져오는 코드를 보았습니다. 우리는 데이터 가져오기 코드의 끝에서 아래의 변수들을 갖게 됩니다.

- train_X
- val_X
- train_y
- val_y

```
import pandas as pd

# 데이터 가져오기
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path)

# 결측치 걸러내기
melbourne_data = melbourne_data.dropna(axis = 0)

# 타겟, 특성 데이터(열) 선택
y = melbourne_data.Price
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'BuildingArea',
						'YearBuilt', 'Lattitude', 'Longtitude']
X = melbourne_data[melbourne_features]

from sklearn.model_selection import train_test_split

# 타겟, 특성 데이터를 각각 학습용, 검증용으로 나누기
# 나누는 것은 난수 생성기를 기반으로 하므로 random_state에 숫자를 넣으면 스크립트를 실행할 때
# 항상 동일한 결과를 얻을 수 있음.
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 0)
```

우리는 scikit-learn을 통해서 의사 결정 트리를 만든 것과 비슷하게 랜덤 포레스트 모델을 만들 것입니다. 이번에는 DecisionTreeRegressor 대신에 RandomForestRegressor를 사용할 것입니다.

```
from sklearn.ensemble import RandomForestRegressor
from sklearn.metrics import mean_absolute_error

forest_model = RandomForestRegressor(random_state = 1)
forest_model.fit(train_X, train_y)
melb_preds = forest_model.predict(val_X)
print(mean_absolute_error(val_y, melb_preds))
```

### 결론

더 개선할 여지가 있을 것 같지만, $250,000이었던 최고의 의사 결정 트리의 오차에 비해 크게 개선됐습니다. 랜덤  포레스트에는, 우리가 여러번 의사 결정 트리에서 최고 깊이를 변경한 것처럼 품질을 개선할 수 있게 하는  매개변수들(Parameters)이 있습니다. 하지만, 랜덤 포레스트 모델의 가장 큰 특징은 튜닝없이도 대게 합리적으로 잘  작동한다는 것입니다.

------

이렇게 Intro to Machine Learning의 여섯번째 수업을 번역하며 공부해봤습니다.



혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다!



출처 : https://www.kaggle.com/dansbecker/random-forests
