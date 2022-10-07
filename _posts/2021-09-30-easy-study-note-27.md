---
layout: single
title: "Intro to Deep Learning(4)"
categories: mldl
tag: [Deep learning overfitting, Deep learning underfitting, Early Stopping, Kaggle, learning curve, 딥러닝 Capacity, 딥러닝 과소적합, 딥러닝 과적합, 캐글, 캐글 코스 번역]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 과적합Overfitting과 과소적합Underfitting

추가 용량이나 조기 정지로 품질을 향상시키기



### 소개

지난 시간의 예시로 돌아가서, Keras는 모델을 학습시켰다는 것을 학습 기록과 에폭에 따른 손실 측정 값에 보관할 것입니다. 이번  시간에는 이 학습 곡선을 어떻게 해석할 것인가와 그것을 어떻게 모델 개발 가이드로 사용할 수 있는지를 배울 것입니다. 특히, 학습 곡선에서 과적합과 과소적합의 증거를 살펴보고 이를 고치기 위한 몇 가지 전략을 살펴볼 것입니다.



### 학습 곡선The Learning Curves 해석하기

여러분은 아마 학습 데이터의 정보가 두 종류라고 생각할 것입니다. 바로, **시그널Signal**과 **노이즈Noise**이죠. 시그널은 일반화하는 부분입니다. 즉, 여러분의 모델이 새로운 데이터로부터 예측을 하는데 도움을 줄 수 있는 부분이죠. 노이즈는  실제 학습 데이터에만 해당되는 부분입니다. 노이즈는 실제 세계의 데이터 또는 (모델이 실제로 예측하는 데 도움이 되지 않는) 모든 부수적인 비정보 패턴에서 발생하는 모든 무작위적인 변동을 의미합니다. 노이즈는 유용해보일지 몰라도, 실제로는 별로 그렇지  않습니다.



여러분은  학습 데이터셋에서 손실을 최소화할 수 있는 가중치나 매개변수를 선택하므로써 모델을 학습시킬 수 있습니다. 하지만, 모델의 품질을  정확히 측정하기 위해서는 새로운 데이터셋의로 평가할 필요가 있다는 것을 아셔야 할 겁니다. 바로, 검증 데이터 말이죠.  (Intro to Machine Learning의 model validation 수업에서 본 적이 있을 것입니다.)



지금까지 모델을 학습시킬 때 학습 데이터의 손실을 에폭이 진행됨에 따라 구분지어 정리해보았습니다. 이제 여기에 검증 데이터도 추가할  것입니다. 이 정리한 내용들을 학습 곡선이라고 부릅니다. 딥러닝 모델을 효과적으로 학습시키기 위해서는 이 학습곡선을 해석할 필요가 있습니다.

![img](https://blog.kakaocdn.net/dn/TZbi0/btrgoAeuTTP/PG6LyatkbT291K9zgPjDK1/img.png)검증 데이터의 손실은 지금까지 보지못했던 데이터의 예상되는 오차의 추정치를 보여줍니다.

위의 그래프에서 보다시피 학습 데이터의 손실은 모델이 시그널을 학습하거나 노이즈를 학습함에 따라 아래로 내려가는 것을 볼 수  있습니다. 하지만, 검증 데이터의 손실은 모델이 시그널을 학습할 때에만 내려가는것을 확인할 수 있습니다. (시그널, 즉 초반에  패턴을 잡는 부분에서는 내려가지만 이후 노이즈가 발생하는 부분에서 검증 데이터의 손실 그래프는 올라가고 있습니다.) 뭐든 모델이  학습 데이터 셋에서 얻는 노이즈가 새로운 데이터에서 일반적이지 않을 것이라는 것이죠. 따라서, 모델이 시그널을 배울 때는 두  곡선(학습, 검증) 모두 내려가지만, 노이즈를 배울 때는 두 곡선 사이에 차이gap가 생깁니다. 그 차이의 크기는 모델이 노이즈를 얼마나 학습했는가를 말해줍니다.



이상적으로는 모델이 모든 시그널만 학습하고 노이즈는 하나도 학습하지 않도록 만들 수 있습니다. 하지만, 그런일은 절대 일어나지 않죠. 대신  우리는 거래trade를 합니다. 여러분은 모델이 더 많은 노이즈를 학습하는 비용으로 더 많은 시그널을 배우게 할 수 있습니다. 이 거래가 우리에게 유리한 한, 검증 데이터의 손실은 계속 내려갈 것입니다. 하지만 특정 포인트 이후에는 그 반대로 비용이 우리가  얻은 이익을 넘어서게 될 것입니다. 그리고, 이에 따라 검증 데이터의 손실은 올라가기 시작할 것입니다.



![img](https://blog.kakaocdn.net/dn/coQwjw/btrgsJ2AOQQ/qImgc8TJSrkGIKKs9AQra0/img.png)과소적합과 과적합

이 거래는 모델을 학습시킬 때 두가지 문제가 생길 수 있다는 것을 가리킵니다. 바로, 부족한 시그널 학습과 너무 많은 노이즈 학습입니다.

과소적합Underfitting은 모델이 충분한 시그널을 학습하지 못했기 때문에 손실이 충분히 낮지 않은 것이고, 과적합Overfitting도 모델이 너무 많은  노이즈를 학습했기 때문에 손실히 충분히 낮지 않습니다. 딥러닝 모델을 학습하기 위한 재주는 이 둘 사이의 균형을 찾는 것입니다.



이제, 노이즈의 양을 줄이는 동안 시그널을 더 학습시킬 수 있는 몇가지 방법을 알아보겠습니다.



### 용량Capacity

모델의 용량은 모델이 학습할 수 있는 패턴의 크기와 복잡도를 의미합니다. 신경망에서는 대체로 얼마나 많은 뉴런을 가졌고, 어떻게 서로  연결되어 있는가에 따라 결정될 것입니다. 만약 여러분의 신경망이 데이터에 과소적합 상태라면, 모델의 용량을 늘려보는 것이  좋습니다.



신경망의 용량은 두가지 방법으로 늘리 수 있는데 첫번째는 **넓게wider** 즉, 레이어 당 뉴런의 개수를 늘리는 방법이고 두번째는 **깊게deeper** 즉, 레이어 자체를 늘리는 방법입니다. 넓게 만든 신경망은 선형 관계를 학습하는 데 시간적으로 더 유리하고, 깊게 만든 신경망은 비선형 관계를 학습하는데 더 유리합니다. 뭐가 더 좋다고 할 수는 없고, 데이터셋에 따라 달라지는게 좋습니다.

```python
model = keras.Sequential([
	layers.Dense(16, activation='relu'),
    layers.Dense(1),
])

wider = keras.Sequential([
	layers.Dense(32, activation='relu'),
    layers.Dense(1),
])

deeper = keras.Sequential([
	layers.Dense(16, activation='relu'),
    layers.Dense(16, activation='relu'),
    layers.Dense(1),
])
```

exercise에서 신경망의 용량이 어떻게 모델의 품질을 더 좋게 만드는지 알아볼 것입니다.



### 조기 정지Early Stopping

앞서서 모델이 너무 열심히 노이즈를 학습하면 검증 데이터셋의 손실이 학습이 진행함에 따라 늘어나기 시작할 것이라고 언급했습니다. 이를  예방하기 위해서, 그냥 단순하게 검증 데이터셋의 손실이 더 이상 내려가지 않을 때 학습을 정지시킬 수 있습니다. 이렇게 학습을  방해하는 방법을 조기 정지Early Stopping이라고 부릅니다.

![img](https://blog.kakaocdn.net/dn/eax3E7/btrgoAlfr1N/56IKePbDLzyVw3vOHTUZUk/img.png)검증 데이터셋의 손실이 최소일 때 정지시켜서 모델을 보존합니다.

한 번 검증 데이터셋의 손실이 다시 올라가기 시작하는 것을 감지할 때, 가중치를 최소 손실이 일어났을 때의 값으로 초기화시킬 수 있습니다. 이렇게하면 모델이 노이즈를 학습하고 데이터에 과적합하는 것을 막을 수 있습니다.



또한, 이렇게 조기 정지로 학습시키는 것은 신경망이 아직 시그널 학습을 마치기도 전에 학습을 너무 일찍 정지할 수 도 있다는 위험성을  가지고 있다는 것을 의미합니다. 너무 오래 학습해서 과적합되는 것을 막는 것외에도, 시그널을 충분히 학습하지 못해서 모델이 충분히 학습되는 것 또한 막을 수 도 있습니다. 그냥 에폭을 필요 이상으로 큰 수로 설정해두면, 나머지는 조기 정지가 알아서 할  거에요.



### 조기 정지Early Stopping 추가하기

Keras에서 콜백Callback을 통해 조기 정지를 여러분의 학습에 포함시킬 수 있습니다. **콜백Callback**이란 단지 신경망이 학습되는 동안 자주 실행하고자 하는 함수를 의미합니다. 조기 정지 콜백 함수는 매 에폭 이후에 실행될 것입니다. (Keras에서는 [미리 정의된 유용한 콜백함수들](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks)도 있지만, [여러분만의 것](https://www.tensorflow.org/api_docs/python/tf/keras/callbacks/LambdaCallback)도 만들 수 있습니다.)

```python
from tensorflow.keras.callbacks import EarlyStopping

early_stopping = EarlyStopping(
	min_delta = 0.001, # 개선 사항으로 간주할 최소 변경량
    patience = 20, # 정지 전 기다릴 에폭의 수
    restore_best_weights = True,
)
```

이 매개변수들은 다음과 같은 의미입니다. "만약 이전 20번의 에폭에서  검증데이터의 손실이 최소한 0.001의 개선되지 않았다면 학습을 정지하고, 이렇게 찾은 가장 좋은 모델을 유지한다." 여기서 종종 검증 데이터의 값이 오른 이유가 과적합 때문인지, 배치가 무작위로 선정되기 때문이지 말하기는 어렵습니다. 단지, 그 매개변수들은 여러분이 언제 모델을 멈출지 약간의 값을 설정할 수 있게 해주는 것입니다.



예시에 살펴볼 것처럼, 여러분은 이 콜백 함수를 옵티마이저와 손실 함수와 같이 fit 메소드에 전달할 것입니다.



### 예시 - 조기 정지를 통한 모델 학습

이전 튜토리얼의 예제에 이어서 모델 개발을 계속해보겠습니다. 이제 신경망의 용량을 늘려볼 것이지만 또한 조기 정지 콜백 함수를 추가해 과적합을 막을 것입니다.



아래는 예제 준비를 위한 코드입니다.

```python
import pandas as pd
from IPython.display import display

red_wine = pd.read_csv('../input/dl-course-data/red-wine.csv')

# 학습용 검증용 데이터 구별
df_train = red_winde.sample(frac=0.7, random_state=0)
df_valid = red_wine.drop(df_train.index)
display(df_train.head(4))

# [0, 1]로 조정
max_ = df_train.max(axis=0)
min_ = df_train.min(axis=0)
df_train = (df.train - min_) / (max_ - min_)
df_valid = (df.valid - min_) / (max_ - min_)

# 특징, 타겟 구분
X_train = df_train.drop('quality', axis=1)
X_valid = df_valid.drop('quality', axis=1)
y_train = df_train['quality']
y_valid = df_valid['quality']
```



이제, 신경망의 용량을 늘려봅시다. 여러분은 꽤 큰 신경망을 추구하겠지만, 일단 검증 데이터셋의 손실이 증가하는 징후를 보이면 학습을 정지하기 위해 콜백 함수에 의존할 것입니다.

```python
from tensorflow import keras
from tensorflow.keras import layers, callbacks

early_stopping = callbacks.EarlyStopping(
	min_delta = 0.001, # 개선 사항으로 간주할 최소 변경량
    patience = 20, # 정지 전 기다릴 에폭의 수
    restore_best_weight = True,
)

model = keras.Sequential([
	layers.Dense(512, activation='relu', input_shape=[11]),
    layers.Dense(512, activation='relu'),
    layers.Dense(512, activation='relu'),
    layers.Dense(1),
])

model.compile(
	optimizer = 'adam',
    loss = 'mae',
)
```

콜백 함수를 정의한 후에 이를 fit 메소드의 인자로 추가합니다. (여러개를 가지고 있다면, 리스트에 넣어서 추가하세요.) 조기 정지 콜백 함수를 사용할 때는 에폭의 수를 필요한 것보다 많이 지정하세요.

```python
history = model.fit(
	X_train, y_train,
    validation_data = (X_valid, y_valid),
    batch_size = 256,
    epochs = 500,
    callback = [early_stopping], # 콜백 함수는 리스트에 넣어서 넘기세요!
    verbose = 0, # 학습 기록 끄기
)

history_df = pd.DataFrame(history.history)
history_df.loc[:, ['loss', 'val_loss']].plot()
print("Minimum validation loss : {}".format(history_df['val_loss'].min()))
```

![img](https://blog.kakaocdn.net/dn/vrSZH/btrgpK19oLb/Kp9FRoj59R0WkzrOkEjSTK/img.png)

그리고 당연히, Keras는 전체 500 에폭이 되기 전에 잘 학습하고 정지합니다.

------

이렇게 캐글(Kaggle)의 Intro to Deep Learning 네번째 수업을 번역하며 공부해봤습니다.



오늘 배운 내용을 요약하자면,

- 학습 곡선에서 시그널과 노이즈를 발견할 수 있고, 쉽게 생각하면 시그널은 데이터의 패턴을 학습하는 부분을 의미하고, 노이즈는 곡선에서 모델의 오차가 많이 줄어들었을 때 무작위로 그 값이 변동되는(지직이는 것처럼 보이는) 부분을 의미합니다.
- 모델이 노이즈를 과도하게 학습하게 되면 과적합Overfitting이 일어나고, 검증 데이터에 대해 노이즈 부분에서의 오차가 커지게 됩니다.
- 이를 방지하기 위해 학습을 덜 시키게 되면, 노이즈는 고사하고 패턴을 찾지 못해 시그널 부분에서의 오차마저 커지는 과소적합Underfitting이 일어나게 됩니다.
- 이 균형을 맞추기 위해 모델의 용량Capacity을 늘리거나 조기에 정지Early Stopping하는 방법으로 해결할 수 있습니다.
    - 용량을 늘릴 때는 각 레이어의 유닛 수를 늘리는 wider 방식이 있고, 레이어 자체의 수를 늘리는 deeper 방식이 있습니다.
    - 조기 정지 방식은 모델의 오차가 최소에서 다시 늘어나는 지점 즉, 노이즈를 학습하는 지점을 포착해 그 전까지의 가중치를 저장하고 학습을 종료하는 콜백 함수입니다. 

혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다.



츨처 : https://www.kaggle.com/ryanholbrook/overfitting-and-underfitting
