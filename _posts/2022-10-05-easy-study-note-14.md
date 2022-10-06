---
layout: single
title: "Intro to Machine Learning(5)"
categories: mldl
tag: [Artificial Intelligence, Kaggle, Kaggle Courses, Machine Learning, Overfitting Underfitting, 과적합, 과소적합, 기계학습, 머신러닝, 인공지능, 캐글]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 과적합(Overfitting)과 과소적합(Underfitting)



당신은 이 단계의 끝에서 과적합과 과소적합의 개념을 이해할 수 있을 것이고 이 개념들을 당신의 모델을 더 정확하게 만드는 것에 적용할 수 있을 것입니다.



### 다른 모델들과의 실험

이제 당신은 모델의 정확도를 측정하는 신뢰할 만한 방법을 알게 되었고, 이를 통해 다른 대체할 수 있는 모델들로 실험하며 어떤 모델이 최고의 예측을 하는지 확인할 수 있습니다. 하지만, 당신은 어떤 대체 모델을 가지고 있나요?



scikit-learn의 공식 문서에서 의사 결정 트리가 당신이 원하거나 필요로 하는 것 이상으로 많은 옵션을 가지고 있다는 것을 확인할 수 있습니다.  가장 중요한 옵셥은 트리의 깊이(depth)를 결정합니다. 우리의 첫번째 강의로 돌아가보면, 트리의 깊이는 예측 전까지 얼마나  많은 분할(split)이 만들어졌는가에 대한 수치라고 배웠습니다. 아래는 비교적 얕은 트리의 예시입니다.

![img](https://blog.kakaocdn.net/dn/ACXEV/btrfRoT8X4v/hygjjasYSPwtKJJAGEHKb0/img.png)

실제로 맨 위 단계와 맨 아래 예측치(leaf) 사이에 10개의 분할(split)을 가진 트리는 드물지 않게 있습니다. 트리가  깊어질수록 데이터셋은 더 적은 집들로 구성된 잎사귀들(leaves)로 잘개 쪼개질 것입니다. 만약 트리가 단지 하나의 분할만  갖는다면, 그것은 데이터를 2개의 그룹으로 나누는 것입니다. 만약, 각 그룹이 또 다시 분할하면, 우리는 4개의 그룹의 주택을  얻게 될 것입니다. 이를 또 반복하면 8개의 그룹을 만들 것입니다. 이렇게 각 레벨의 분할을 추가하여 계속 2배씩 그룹이 증가하면 10번째 단계에서는 2^10개의 주택 그룹을 갖게 될 것입니다. 1024개의 잎사귀들이죠.



많은 잎사귀(leaves)들 사이에서 주택들을 나눌 때, 또한 각 예측치(leaf)에 더 적은 주택을 갖게 됩니다. 아주 적은  주택들로 이루어진 잎사귀(leaves)들은 주택들의 실제 가치와 꽤 근사한 예측을 만들지만, 아마도 새로운 데이터에 대해서 매운  신뢰도가 낮은 예측을 만들어낼 것입니다. 왜냐하면 각 예측치가 단지 몇 개의 주택만을 기반으로 하기 때문이죠.



이것이 **과적합(Overfitting)**이라고 불리는 현상입니다. 모델이 학습 데이터에 거의 완벽하게 맞지만, 새로운 데이터와 검증에서는 제대로 작동하지 않는 것을 의미하죠. 한편으론 만약 트리를 매우 얕게 만든다면, 그 트리가 주택을 잘 구별된 그룹으로 나누지 못한다고도 할 수 있습니다.



극단적으로 만약 트리가 주택을 단지 2개나 4개로만 나눈다면, 각 그룹은 아직도 주택의 넓은 다양성을 갖게 됩니다. 즉, 구별이 명확하지  않다는 것이죠. 그렇게되면 예측 결과는 아마 대부분의 주택의 실제 가치와 멀게 될 것입니다. 심지어 그것이 학습 데이터에 있다고  하더라도 말이죠. 또한, 같은 이유로 검증에서도 제대로 작동하지 않을 것입니다. 모델이 데이터에서 중요한 특징과 패턴을 찾는 것에 실패할 때, 그래서 모델의 품질이 학습에 사용한 데이터를 사용하더라도 좋지 않을 때, 이를 **과소적합(Underfitting)**이라고 부릅니다.



우리는 검증 데이터로부터 어림잡은 새로운 데이터에서의 정확성을 신경쓰기 때문에, 과적합과 과소적합 사이에서 최적의 지점을 찾고  싶습니다. 시각적으로, 우리는 아래의 그래프에서 빨간색의 검증(Validation) 곡선의 저점을 원합니다.

![img](https://blog.kakaocdn.net/dn/okGKP/btrfU2JK14v/XLNtJr4fCl5ay81Ar5x8DK/img.png)

### 예시

트리의 깊이를 조절하는 몇가지 대안들이 있고, 많은 사람들이 트리를 통과하는 일부 경로가 다른 경로보다 더 깊은 깊이를 갖도록  허용합니다. 하지만, max_leaf_nodes 인자는 과적합과 과소적합을 제어하는 매우 실용적인 방법을 제공합니다. 모델이 더  많은 잎사귀들(leaves)을 만들도록 허용하면, 우리는 위의 그래프에 표시된 과소적합 지역에서 과적합 지역으로 이동하게 됩니다.



우리는 효용 함수(Utility Function)를 사용하여 max_leaf_node에 대해 다른 값의 MAE 점수를 비교하는 것을 도울 수 있습니다.

```
from sklearn.metrics import mean_absolute_error
from sklearn.tree import DecisionTreeRegressor

def get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y):
	model = DecisionTreeRegressor(max_leaf_nodes = max_leaf_nodes, random_state = 0)
    model.fit(train_X, train_y)
    preds_val = model.predict(val_X)
    mae = mean_absolute_error(val_y, preds_val)
    return(mae)
```

데이터는 이미 보고 실습해본 코드를 사용해서 train_X, val_x, train_y, val_y로 불러왔습니다.



```
# 데이터 불러오기 코드는 이 지점에서 실행됩니다.
import pandas as pd

# 데이터 가져오기
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path)

# 결측치 걸러내기
filtered_melbourne_data = melbourne_data.dropna(axis = 0)

# 타겟, 특성 데이터(열) 선택
y = filtered_melbourne_data.Price
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'BuildingArea',
						'YearBuilt', 'Latitude', 'Longtitude']
X = filtered_melbourne_data[melbourne_features]


from sklearn.model_selection import train_test_split

# 타겟, 특성 데이터에서 각각 학습용, 검증 데이터로 나누기.
train_X, val_X, train_y, val_y = train_test_split(X, y, random_state = 0)
```

for 문으로 max_leaf_nodes에 대해 다른 값들로 모델의 정확성을 비교해볼 수 있습니다.

```
# max_leaf_nodes에 대한 다른 값들에 대해 평균 절대 오차 비교
for max_leaf_nodes in [5, 50, 500, 5000]:
	my_mae = get_mae(max_leaf_nodes, train_X, val_X, train_y, val_y)
    print("Max leaf nodes: %d   \t\t Mean Absolute Error:  %d" %(max_leaf_nodes, my_mae))
```

위 리스트 중에서 최적의 잎사귀(leaves)수는 500이다.



### 결론

모델은 둘 중 하나를 겪을 수 있다.

- 과적합(Overfitting) : 미래에 재발하지 않을 가짜 패턴을 포착하여 덜 정확한 예측을 유도하거나
- 과소적합(Underfitting) : 연관성있는 패턴을 포착하는데 실패하여 마찬가지로 덜 정확한 예측을 유도하거나

우리는 모델 학습에 사용되지 않은 검증 데이터를 사용해서 후보 모델의 정확도를 측정한다. 이것은 우리가 많은 후보 모델들을 실험해보고 가장 좋은 것을 선택할 수 있게 해준다.

------

이렇게 Intro to Machine Learning의 다섯번째 수업을 번역하며 공부해봤습니다.



혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하곘습니다.



출처 : https://www.kaggle.com/dansbecker/underfitting-and-overfitting
