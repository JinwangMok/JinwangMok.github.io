---
layout: single
title: "Intro to Deep Learning(5)"
categories: mldl
tag: [batch normalization, dropout, kaggle, 드롭아웃, 딥러닝, 딥러닝 모델 최적화, 배치정규화, 캐글, 캐글 딥러닝, 캐글 코스 번역]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 드롭아웃과 배치 정규화Dropout and Batch Normalization

과적합 방지와 안정적인 학습을 위해 이 특별한 레이어들을 추가해보세요!



### 소개

딥러닝의 세계에는 dense 레이어뿐만 아니라 더 많은 것들이 있습니다. 여러분이 모델에 추가하고 싶어할 여러가지 종류의 수십가지 레이어가 있습니다.(더 궁금하시다면 [Keras docs](https://www.tensorflow.org/api_docs/python/tf/keras/layers/)를 참고해주세요.) 이 중 어떤 것들은 dense 레이어 같고 뉴런들 사이의 연결을 정의하며, 다른 어떤 것들은 다른 종류의 전처리나 변형을 할 수 있습니다.



이번 시간에는 두 가지 특별한 레이어를 살펴볼 것입니다. 이 두 가지는 어떤 뉴런과도 연결되지 않지만, 여러가지 방법을 통해 때때로  모델을 향상시킬 수 있는 몇가지 기능을 추가할 수 있습니다. 이 두 가지 모두 현대 모델 설계에서 흔히 사용됩니다.



### 드롭아웃Dropout

첫번째로 과적합 문제를 해결하는데 도움을 줄 수 있는 "드롭아웃 레이어"입니다.



지난 시간에 신경망이 학습 데이터에서 가짜 패턴을 배우므로써 과적합이 어떻게 발생하는지에 대해 다뤘습니다. 이런 가짜 패턴들을  알아내기 위해 신경망은 자주 가중치의 결합conspiracy의 일종인 아주 특별한 가중치 조합에 의존할 것입니다. 이렇게  구체적이기 때문에 조심히 다룰 필요가 있습니다. 하나를 제거하면 결합이 무너지기 때문이죠.



이것이 드롭아웃의 뒤에 있는 개념입니다. 이 결합을 깨기 위해, 매 학습 단계마다 레이어의 입력 유닛의 일부를 무작위로 버려야drop  out 합니다. 이것은 신경망이 학습 데이터에서 가짜 패턴을 배우는 것을 훨씬 어렵게 만들 수 있습니다. 대신, 가중치 패턴이 더 확실한 경향이 있는 더 넓고 일반적인 패턴을 찾아야합니다.

![img](https://blog.kakaocdn.net/dn/bmR23r/btrg2iDHERn/K2Qxhk78G1qdLOXu0KktYK/img.gif)여기 50% 드롭아웃이 두 히든 레이어들 사이에 추가됐습니다.

여러분은 드롭아웃을 일종의 신경망의 조화ensemble로 생각할 수 있습니다. 에측 값은 더이상 하나의 큰 신경망으로부터 만들어지지  않고, 대신 더 작은 신경망들의 조합으로 만들어집니다. 이 조합에 있는 각 신경망들은 제각기 다른 종류의 오차를 만드는 경향이  있지만 그래도 동시에 이 조합을 어떤 각각의 신경망보다도 전체적으로 좋게 만듭니다.(만약 의사결정 트리의 조합인 랜덤  포레스트Random Forests에 익숙하다면, 그것과 같은 개념이라고 말씀드릴 수 있겠네요.)



### 드롭아웃 추가하기

Keras에서 드롭아웃 비율 인자인 **rate**는 입력 유닛의 몇 퍼센트를 버릴 것인지를 정의합니다. 아래와 같이 드롭아웃을 적용할 레이어 바로 뒤에 Dropout 레이어를 넣으세요.

```
keras.Sequential([
    # ...
    layers.Dropout(rate=0.3), # 다음 레이어로 가는 입력의 30%를 버립니다. drop out.
    layers.Dense(16),
    # ...
])
```

###  

### 배치 정규화Batch Normalization

다음으로 살펴볼 특별한 레이어는 배치 정규화"batch normalization"(또는 "batchnorm")입니다. 이 레이어는 느리거나 안정적이지 않은 학습을 수정하는 것을 도와줍니다.



신경망을 사용하면 모든 데이터를 보통의 크기로, 아마도 scikit-learn의 [StandardScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.StandardScaler.html) 나 [MinMaxScaler](https://scikit-learn.org/stable/modules/generated/sklearn.preprocessing.MinMaxScaler.html)와 같은 것들로 넣는 것은 보편적으로 좋은 생각입니다. 그 이유는 확률 경사 하강법(SGD)이 신경망의 가중치를 데이터가 생성하는  활성화의 크기의 비율로 바꿀 것이기 때문입니다. 매우 다른 크기의 활성화를 생성하는 경향이 있는 특징Features은 불안정한  학습 행동을 만들 수 있습니다.



이제, 만약 신경망에 들어가기전에라도 데이터를 정규화하는 것이 좋다면, 아마도 신경망 안에서 정규화하는 것이 더 좋을 것입니다. 사실, 여러분은 이 작업을 할 수 있는 특별한 종류의 레이어를 가지고 있습니다. 그것이 바로 배치 정규화 레이어batch  normalization layer이죠. 배치 정규화 레이어는 각각의 배치가 들어올 때 주시하고 있다가 먼저 그것의 평균과  표준편차로 그 배치를 정규화합니다. 그리고나서 또 그 데이터를 두 가지의 학습 가능한 재조정 매개변수를 가진 새로운  크기scale로 만듭니다. 배치 정규화는 사실, 그것의 입력에 대한 통합 재조정이라고 볼 수 있는 작업을 수행합니다.



배치 정규화는 때에 따라 예측 품질에 도움이 될 수 있다는 이유로 최적화optimization 작업의 보조로 가장 자주 추가됩니다.  배치 정규화를 포함한 모델은 학습을 완료하는데 더 적은 에폭epochs을 필요로하는 경향이 있습니다. 게다가, 배치 정규화는 또한 때때로 학습을 방해하는get "stuck" 여러가지 문제를 고칠 수도 있습니다. 특별히 만약 여러분이 모델 학습 중에 문제가  생긴다면, 여러분의 모델에 배치 정규화를 추가하는 것을 고려해보세요!



### 배치 정규화 추가하기

배치 정규화는 신경망의 거의 모든 지점point에서 사용될 수 있어 보입니다. 아래와 같이 레이어 뒤에 넣을 수도 있고..

```
layers.Dense(16, activation='relu'),
layers.BatchNormalization(),
```

또는, 아래와 같이 레이어와 그 레이어의 활성 함수 사이에도 넣을 수 있습니다.

```
layers.Dense(16),
layers.BatchNormalization(),
layers.Activation('relu')
```

그리고 만약 배치 정규화를 신경망의 첫번째 레이로 추가한다면, 그것은 마치 적응형 전처리기adaptive preprocessor처럼 동작할 것이고 scikit-learn의 StandardScaler와 같은 것을 대신할 것입니다.



### 예시 - 드롭아웃과 배치 정규화 사용

Red Wine 모델을 계속 개발해봅시다. 이제 용량을 더 많이 늘릴 것이지만, 과적합을 제어하기 위해서 드롭아웃과 최적화 작업의  속도를 높이기 위해 배치 정규화를 추가할 것입니다. 이번에는 배치 정규화가 어떻게 학습을 안정화시킬 수 있는지를 증명하기 위해  데이터 표준화standardizing도 중단해보겠습니다.

```
# 플롯팅 설정
import matplotlib.pyplot as plt

plt.style.use('seaborn-whitegrid')

# matplotlib 기본 설정
plt.rc('figure', autolayout=True)
plt.rc('axes', labelweight='bold', labelsize='large',
		titleweight='bold', titlesize=18, titlepad=10)


import pandas as pd
red_wine = pd.read_csv('../input/dl-course-data/red-wine.csv')

# 학습용, 검증용 구분
df_train = red_wine.sample(frac=0.7, random_state=0)
df_valid = red_wine.drop(df_train.index)

# 특징과 타겟 구분
X_train = df_train.drop('quality', axis=1)
X_valid = df_valid.drop('quality', axis=1)
y_train = df_train['quality']
y_valid = df_valid['quality']
```



드롭아웃을 추가할 때, 아마 Dense 레이어의 유닛 수를 증가시킬 필요가 있을 것입니다.(드롭아웃, 일부를 버릴꺼니까요.)

```
from tensorflow import keras
from tensorflow.keras import layers

model = keras.Sequential([
    layers.Dense(1024, activation='relu', input_shape=[11]),
    layers.Dropout(0.3),
    layers.BatchNormalization(),
    layers.Dense(1024, activation='relu'),
    layers.Dropout(0.3),
    layers.BatchNormalization(),
    layers.Dense(1024, activation='relu'),
    layers.Dropout(0.3),
    layers.BatchNormalization(),
    layers.Dense(1),
])
```



이번에는 우리가 학습을 설정한 것에서 아무것도 변경하지 않습니다.



```
model.compile(
    optimizer='adam',
    loss='mae',
)

history = model.fit(
    X_train, y_train,
    validation_data=(X_valid, y_valid),
    batch_size=256,
    epochs=100,
    verbose=0,
)

# 학습 곡선 표시
history_df = pd.DataFrame(history.history)
history_df.loc[:, ['loss', 'val_loss']].plot()
```



데이터를 학습에 사용하기 전에 표준화하면 일반적으로 더 나은 성능을 얻을 수 있습니다. 하지만, 원시 데이터를 사용할 수 있었다는 것은 더 어려운 데이터셋에서 배치 정규화가 얼마나 효과적인지를 보여주죠.

------

이렇게 캐글(Kaggle)의 Intro to Deep Learning의 다섯번째 수업을 번역하며 공부해봤습니다.



오늘 배운 내용을 요약하자면,

- 모델의 퍼포먼스를 향상시킬 수 있는 여러종류의 특별한 레이어들이 있고, 그 중 드롭아웃과 배치 정규화 레이어가 있다.
- 드롭아웃 레이어는 입력으로 들어오는 유닛을 rate 인수의 비율만큼 버려서 과적합을 예방할 수 있는 레이어이다.
- 배치 정규화는 들어오는 배치를 평균과 표준 편차로 정규화 시켜서 모델의 이상치 학습을 방지하여 모델의 안정성과 속도를 도모합니다.  또한, 이상치를 제거하는 등의 동작이 최적화의 도움이 되다보니 학습에 필요한 에폭 수를 줄이거나 여러가지 문제를 해결하는 것에도  도움이 되어 다양하게 사용됩니다.
    - 배치 정규화는 여러 위치에 유연하게 삽입될 수 있습니다.

혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다.



출처 : https://www.kaggle.com/ryanholbrook/dropout-and-batch-normalization
