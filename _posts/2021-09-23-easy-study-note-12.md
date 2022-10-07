---
layout: single
title: "Intro to Machine Learning(3)"
categories: mldl
tag: [Artificial Intelligence, Kaggle, Kaggle Courses, Machine Learning, 기계학습, 머신러닝, 머신러닝기초, 인공지능, 인공지능기초, 캐글]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 당신의 첫번째 기계 학습 모델

당신의 첫번째 모델을 만들어보세요. 만세!



#### 모델링을 위한 데이터 선택하기

당신의 데이터셋은 변수가 너무나도 많습니다. 어떻게 데이터의 이 압도적인 양을 이해할 수 있도록 줄일 수 있을까요?



우리의 통찰력을 사용해서 몇가지 변수를 집어내고자 합니다. 이후 코스들에서는 자동으로 변수들의 우선순위를 정하는 전형적인 기술을 소개하려고 해요.



일단, 변수/열(columns)을 선택하기 위해서는 데이터셋의 모든 열 목록을 봐야합니다. 이 목록은 데이터프레임의 **columns** 속성으로 볼 수 있습니다.(아래 코드의 맨 아래줄)

```python
import pandas as pd

melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'
melbourne_data = pd.read_csv(melbourne_file_path)

melbourne_data.columns
# 지금 수업에서 사용하는 melbourne data는 결측값을 가지고 있습니다. 
# 그래서 아래의 코드로 처리할껀데..
# 당신이 실습에서 다룰 Iowa 데이터는 결측값을 가지고 있지 않아요.
# 결측값을 다루는 것은 이후 튜토리얼에서 배울거에요.
# 따라서, 지금은 가장 간단한 옵션을 사용해서 데이터 중 결측값을 처리하겠습니다.

# 일단 지금은 신경쓰지마세요. 코드는 이렇습니다.
# dropna 메소드는 결측값을 제거합니다.
# (na는 "not available"의 준말로 생각되네요.)

melbourne_data = melbourne_data.dropna(axis=0)
```

데이터의 부분 집합을 선택하는 것에는 많은 방법이 있습니다. 이에 대해서는 우리 캐글의 Pandas 코스에서 깊이 있게 다룹니다. 하지만 지금은 두가지 목표에 집중하도록 하겠습니다.



\1. 예측 타겟을 선택하기 위한 dot(.) 표기법

\2. 특성(features)을 선택하기 위한 열 목록으로 선택하기





#### 예측 타겟 선택하기

점 표기법dot-notation으로 변수(열)를 꺼낼 수 있습니다. 이 하나의 열은 시리즈(Series)로 저장됩니다. 시리즈는 대충 하나의 열 데이터로 이루어진 데이터 프레임을 의미합니다.



우리는 **예측 타겟**이라고 부르는 우리가 예측하길 원하는 열을 이 점 표기법으로 선택할 것입니다. 멜버른 데이터에서 주택 가격을 저장하는 코드는 아래와 같습니다. 관습적으로 예측 타겟은 y라고 합니다.

```python
y = melbourne_data.Price
```



#### 특성(Features) 선택하기

추후 예측에 사용될, 우리 모델에 입력되는 열들을 **특성(features)**이라고 합니다. 지금 다루는 데이터의 경우에는 그것들이 주택 가격을 결정하는데 사용될 것입니다. 때로는 타겟을 제외한 모든 열을 특성으로 사용할 것이고, 때로는 더 적은 특성을 사용하는 것이 나을 때도 있을 것입니다.



일단 지금은 약간의 특성만 가지고 있는 모델을 만들어보겠습니다. 나중에 각기 다른 특성들로 구성된 여러 모델들을 어떻게 반복하고 비교하는지 알아볼 것입니다.



우리는 괄호안에 열 이름들(column names)로 구성된 리스트를 제공하므로써 여러가지 특성을 선택합니다. 이 리스트 내에 있는 각각의 요소들은 따옴표로 표현된 문자열이어야 합니다.



아래에 예제가 있습니다.

```python
melbourne_features = ['Rooms', 'Bathroom', 'Landsize', 'Lattitude', 'Longtitude']
```

관습적으로 이것을 대문자 X라고 부릅니다.

```pyhton
X = melbourne_data[melbourne_features]
```

자, describe 메소드와 head 메소드로 우리가 주택 가격을 예측하는 데 사용할 데이터를 빠르게 살펴보죠! (head 메소드는 상단의 몇 행을 보여주는 메소드입니다.)

```python
X.describe()
X.head()
```

위와 같은 명령들로 당신의 데이터를 눈으로 확인하는 작업은 데이터 사이언티스트의 업무 중에서도 중요한 업무입니다. 데이터셋에서 깊게 조사해볼 만한 놀라운 사실들을 자주 찾을 수 있을거에요!



#### 당신만의 모델 만들기

당신의 모델을 만드는 것에 scikit-learn 라이브러리를 사용할 것입니다. 코드에서 이 라이브러리는 sklearn이라고 쓰고,  예제 코드에서 확인해볼 수 있을 겁니다. scikit-learn은 일반적으로 데이터프레임의 형태로 저장된 데이터 타입을 모델링하는 라이브러리들 중에 가장 많이 쓰인다고 할 수 있습니다.



모델을 만들고 사용하는 단계는 아래와 같습니다.



- **Define** : 모델이 어떤 타입일까요? 의사결정트리? 다른 모델 타입? 모델 타입의 다른 매개변수들도 지정됩니다. 모델 타입 지정!
- **Fit** : 제공된 데이터로부터 패턴들을 찾아냅니다. 이 단계가 모델링의 핵심입니다.
- **Predict** : 말그대로 예측하는 단계입니다.
- **Evaluate** : 모델의 예측이 얼마나 정확한지를 측정하는 단계입니다.



아래는 scikit-learn으로 의사결정트리 모델을 정의하고, 특성과 타겟 변수로 fitting하는 예제입니다.

```python
from sklearn.tree import DecisionTreeRegressor

# 모델 정의. 각 수행마다 동일한 결과를 보장하기 위해 random_state의 숫자를 지정.
melbourne_model = DecisionTreeRegressor(random_state=1)

# 모델 학습(fit)
melbourne_model.fit(X, y)
```

많은 기계학습 모델들은 모델  학습 내에 약간의 무작위성을 허용합니다. random_state의 수를 지정한 것은 각 수행에서 동일한 결과를 보장하기  위함입니다. 이는 좋은 관행이라고 여겨집니다. 어떤 숫자를 사용하든, 모델의 품질은 정확히 어떤 값을 선택하느냐에 따라 크게  달라지지 않을 거에요.



이제 우리가 예측에 사용할 수 있는 학습된 모델을 만들었습니다.



실제로 당신은 우리가 이미 가격을 가지고 있는 주택보다 시장에 나오는 새 주택에 대해 예측하고 싶을 겁니다. 하지만 먼저, 예측 기능이 어떻게 작동하는가를 보기위해 학습 데이터의 첫 몇 줄에 대해 예측해볼 것입니다.

```python
print("Making predictions for the following 5 houses :")
print(X.head())
print("The predictions are")
print(melbourne_model.predict(X.head()))
```

------

[요약]

scikit-learn라이브러리로 모델만들기 순서



1. 변수 y에 주어진 데이터 프레임에서 예측값으로 설정하고 싶은 열을 시리즈로 저장.
    - 이때 '데이터프레임.해당열' 이런식으로 dot-notation하여 선택하면 시리즈 타입 리턴함.
2. 변수 X에 예측에 사용할 열들에 해당하는 값을 저장
    - 먼저, 다른 리스트 자료형 변수 안에 사용할 열들의 열 이름을 문자열로 저장
    - 이 리스트에 대해서 '데이터프레임[리스트]'하면 해당 열들에 대한 레코드 값 형태의 새로운 데이터프레임 리턴함.
3. 변수 X가 제대로 만들어졌는지 X.head() 메소드로 확인
4. scikit-learn 라이브러리 import
5. 모델 타입 지정(회귀?의사결정트리? 등등)
    - 모델 타입에 대한 매개변수 설정(random_state 등)
6. 모델 학습
    - 모델.fit(X, y)
7. 모델 사용(예측하기)
    - 모델.predict(X)





이렇게 Intro to Machine Learning의 세번째 수업을 번역하며 공부해봤습니다.



혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다!



출처 : https://www.kaggle.com/dansbecker/your-first-machine-learning-model
