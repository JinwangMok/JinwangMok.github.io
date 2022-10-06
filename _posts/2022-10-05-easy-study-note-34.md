---
layout: single
title: "Intro to Deep Learning(6)"
categories: mldl
tag: [binary classification, cross-entropy, kaggle, keras, sigmoid, 딥러닝, 시그모이드, 캐글, 캐글 딥러닝, 캐글 코스 번역]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 이진 분류Binary Classification

딥러닝을 다른 보편적인 작업에 적용해봅시다.



### 소개

이 수업에서 지금까지 신경망이 어떻게 회귀regression 문제를 풀 수 있는지를 배웠습니다. 이제는 신경망을 또 다른 보편적인  기계학습 문제에 적용해볼 것입니다. 바로, 분류classification죠. 지금까지 배운 거의 모든 것들은 여전히 적용됩니다.  가장 큰 차이점은 사용하는 손실 함수와 마지막 레이어에서 만들어내길 원하는 출력값의 종류입니다.



### 이진 분류

두 가지 클래스 중에서 한가지로 분류하는 것은 보편적인 기계학습 문제입니다. 여러분은 아마 소비자가 구매를 할지, 신용카드의 거래가 사기인지, 깊은 우주의 신호가 새로운 행성의 증거를 보여주는지, 의료 검사가 질병의 증거인지 예측하고 싶을 것입니다. 이것들이  모두 이진 분류 문제의 예시입니다.



여러분의 원시 데이터에는 클래스들이 아마 "Yes"와 "No" 또는 "Dog"와 "Cat"와 같은 문자열로 표현되어 있을 것입니다. 이  데이터를 사용하기 전에 클래스 레이블class label을 할당할 것입니다. 한 종류는 0, 다른 하나는 1로 말이죠. 이렇게  숫자형 레이블로 할당하는 것은 신경망이 사용할 수 있는 형식으로 그 데이터를 넣을 수 있게 만듭니다.



### 정확도Accuracy와 교차 엔트로피Cross-Entropy

정확도는 분류 문제의 성공을 측정하는데 사용되는 많은 지표metrics중 하나 입니다. 정확도는 전체 예측값 중 맞은 예측값의  비율입니다. 즉, "정확도 = 맞은 개수 / 전체 개수" 라고 표현할 수 있지요. 항상 정확하게 예측하는 모델은 1.0이라는  점수의 정확도를 갖게 될 것입니다. 다른 모든 것이 동일하다면, 정확도는 데이터셋의 클래스들이 거의 동일한 빈도로 나타날 때마다  사용하기에 합리적인 지표입니다.



정확도(그리고 거의 모든 다른 분류의 지표)의 문제는 손실 함수로 사용될 수 없다는 것입니다. 확률 경사 하강법(SGD)은 부드럽게 변화하는  손실 함수를 필요로 하지만, 개수의 비율로 구성된 정확도라는 값은 변화가 급격changes in "jumps"합니다. 따라서,  손실 함수로 동작할 대체값을 찾아야 합니다. 이 대체값이 *교차 엔트로피cross-entropy* 함수입니다.



이제, 손실 함수가 학습중인 신경망의 목표를 정의한다는 점을 기억해봅시다. 회귀에서, 우리의 목표는 기대 결과값과 예측 결과값 사이의  거리를 최소화하는 것이었습니다. 그리고 이 거리를 측정하기 위해 MAE(평균 절대 오차)를 선택했었죠.



대신, 분류에서 원하는 것은 확률probabilities들 사이의 거리입니다. 그리고 이것이 교차 엔트로피가 제공하는 것이죠. 교차  엔트로피는 하나의 확률 분포probability distribution로부터 또 다른 것 사이의 거리에 대한 일종의 측정값입니다.



![img](https://blog.kakaocdn.net/dn/H1jQU/btrg8bKYfcM/bVSHjtKcwD1HGFyOWLCnL1/img.png)교차 엔트로피는 정확하지 않은 확률 예측에 페널티를 줍니다.

여러분이 신경망에게 원하는 의도idea는 정확도 1.0으로 올바른 클래스를 예측하는 것입니다. 예측 확률이 1.0에서 멀어질수록 교차 엔트로피 오차가 커집니다. 



교차 엔트로피를 사용하는 기술적인 이유는 약간 미묘하지만, 이 섹션에서 주목할 것은 단지 분류 오차에 교차 엔트로피를 사용하라는 것입니다. 정확도와 같이 여러분이 다루고자하는 다른 지표들은 이것과 함께 개선되는 경향이 있습니다.



## 시그모이드Sigmoid 함수로 확률Probabilities 만들기

교차 엔트로피와 정확도 함수 둘 다 입력으로 0부터 1까지의 숫자로 된 확률을 요구합니다. Dense 레이어에 의해 생성된 실제 값의 출력을 확률로 변환하기 위해서 새로운 종류의 활성 함수인 시그모이드 활성 함수를 사용합니다. 

![img](https://blog.kakaocdn.net/dn/65LS1/btrg6vRjxfZ/GXnP6IvhTDPbtM3RKgUWB1/img.png)시그모이드 함수는 실수를 0과 1 사이의 수로 매핑합니다.

마지막 클래스 예측을 얻기 위해서, 기준값 확률threshold probability을 정의해야합니다. 대게 이것은 0.5이고, 따라서 반올림하는 것이 올바른 클래스를 줄 것입니다. 0.5보다 작으면 레이블이 0인 클래스, 0.5보다 크거나 같으면 레이블이 1인  클래스로 말이죠. 0.5 기준값은 Keras가 [정확도 지표accuracy metric](https://www.tensorflow.org/api_docs/python/tf/keras/metrics/BinaryAccuracy)에서 기본으로 사용하고 있는 값입니다.



### 예시 - 이진 분류

자, 이제 직접 해봅시다!



[전리층Ionosphere](https://archive.ics.uci.edu/ml/datasets/Ionosphere) 데이터셋은 지구 대기권의 전리층에 초점을 맞춘 레이더 신호로부터 얻은 특징features들을 포함합니다. 이것으로 해볼 것은 신호가 어떤 물체의 존재를 나타내는지 아니면 그냥 빈 공기를 나타내는지 결정하는 것입니다. 

```
import pandas as pd
from IPython.display import display

ion = pd.read_csv('../input/dl-course-data/ion.csv', index_col=0)
display(ion.head())

df = ion.copy()
df['Class'] = df['Class'].map({'good' : 0, 'bad' : 1})

df_train = df.sample(frac=0.7, random_state=0)
df_valid = df.drop(df_train.index)

max_ = df_train.max(axis=0)
min_ = df_train.min(axis=0)

df_train = (df_train - min_) / (max_ - min_)
df_valid = (df_valid - min_) / (max_ - min_)
df_train.dropna(axis=1, inplace=True) # 2번 열의 비어있는 feature 버림
df_valid.dropna(axis=1, inplace=True)

X_train = df_train.drop('Class', axis=1)
X_valid = df_valid.drop('Class', axis=1)
y_train = df_train['Class']
y_valid = df_valid['Class']
```



딱 하나만 제외하고는 회귀 작업에서 했던 것과 똑같이 모델을 정의할 것입니다. 바로, 마지막 레이어가 'sigmoid' 활성 함수를 포함하는 것만 제외하고 말이죠. 이를 통해 모델은 클래스의 확률을 만들 수 있게 됩니다.

```
from tensorflow import keras
from tensorflow.keras import layers

model = keras.Sequential([
    layers.Dense(4, activation='relu', input_shape=[33]),
    layers.Dense(4, activation='relu'),
    layers.Dense(1, activation='sigmoid'),
])
```



교차  엔트로피 손실 함수와 정확도 지표metric를 compile 메소드로 모델에 추가합니다. 두 클래스 문제에서, 'binary'  버전을 사용한다는 것을 확실히 하세요.(더 많은 클래스를 가진 문제에서는 살짝 다를 것입니다.) Adam 옵티마이저는 분류  문제에서도 멋지게 동작하니, 그대로 둡시다.

```
model.compile(
    optimizer = 'adam'
    loss = 'binary_crossentropy',
    metrics = ['binary_accuracy'],
)
```



이 특별한 문제에서 모델은 꽤 조금의 에폭만으로 학습을 완료할 수 있습니다. 따라서, 편의를 위해 조기 정지early stopping 콜백 함수를 포함하겠습니다.

```
early_stopping = keras.callback.EarlyStopping(
    patience = 10,
    min_delta = 0.001,
    restore_best_weights = True,
)

history = model.fit(
    X_train, y_train,
    validation_data = (X_valid, y_valid),
    batch_size = 512,
    epochs = 1000,
    callbacks = [early_stopping],
    verbose = 0, # 에폭이 너무 많으니 결과는 숨기겠습니다.
)
```



항상 그랬듯 학습 곡선을 살펴보고, 또 검증 데이터셋으로부터 얻은 오차와 정확도에 대해 가장 좋은 값을 검사할 것입니다. 조기 정지early stopping은 이 값들을 얻은 가중치로 복원시킨다는 것을 기억하세요.



```
history_df = pd.DataFrame(history.history)
# 5번째 에폭부터 시작
history_df.loc[5:, ['loss', 'val_loss']].plot()
history_df.loc[5:, ['bindary_accuracy', 'val_binary_accuracy']].plot()

print(("Best Validation Loss : {:0.4f}" + \
        "\nBest Validation Accuracy : {:0.4f}")\
        .format(history_df['val_loss'].min(),
                history_df['val_binary_accuracy'].max()))
```

------

이렇게 캐글(Kaggle)의 Intro to Deep Learning의 마지막 수업을 번역하며 공부해봤습니다.



오늘 배운 내용을 요약하자면,

- 딥러닝으로 Classification을 하는 방법에 대해 다뤘습니다.
- Classification에서 중요한 것은 기존의 회귀에서 사용했던 mae같은 손실함수를 사용하지 못한다는 것인데, binary classification의  accuracy 또한 값이 부드럽지 못하고 변동이 커서(?) SGD에서 사용하기 힘들기 때문에 대체제가 필요하고, 그것이 바로  Cross-Entropy라는 것이었습니다.
    - Cross-Entropy는 예측확률이 1.0에서 멀어지면 오차가 커지는데, 자세한 내용을 위해 좀 더 찾아봐야할 것 같습니다.
- 마지막으로, 기존 레이어와의 차이점으로 맨 마지막 출력 레이어에 activation이 sigmoid라는 것이었습니다. 시그모이드는 확률의  Odds함수에 로그를 씌운 식을 정리한 함수인데 미분이 가능한 계단 모양의 함수로 임계값(0.5)에서 기울기가 높아지고 0과 1을 점근선으로 가지면서 분류에 알맞는 함수입니다. 이를 통해 결과값을 이진binary으로 얻을 수 있기에 사용됩니다.

혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다.



출처 : https://www.kaggle.com/ryanholbrook/binary-classification
