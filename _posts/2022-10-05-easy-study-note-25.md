---
layout: single
title: "Intro to Deep Learning(2)"
categories: mldl
tag: [Activation Function, deep learning, kaggle, layers, ReLU, 딥러닝, 딥러닝 레이어, 딥러닝 활성 함수, 정류 선형 유닛, 캐글]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

(이 글은 캐글의 Intro to Deep Learning 강의를 번역한 것입니다. 글 하단의 출처를 통해 캐글 사이트에 방문하시면, 캐글 노트북을 통해 직접 모델을 만드실 수 도 있으니 한번쯤 방문해보시는 것을 추천드립니다.)



## 심층 신경망Deep Neural Networks

복잡한 관계들을 파해치기 위해 히든 레이어를 신경망에 추가해보세요!



### 소개

이 수업에서는 심층 신경망이 유명해진 어떤.. 복잡한 관계들을 학습하는 신경망을 만드는 방법을 살펴볼 것입니다.



여기서 중요한 아이디어는 모듈방식modularity입니다. 모듈방식이란 복잡한 네트워크를 그보다는 단순한 기능적인 유닛으로부터  만들어내는 것을 말합니다. 여러분은 선형 유닛이 선형 함수를 어떻게 연산하는지를 앞선 수업에서 보셨습니다. 이제는 이 하나하나의  유닛을 더 복잡한 관계로 얽힌 모델로 묶고, 수정하는 것을 배울 것입니다.



### 레이어Layers

일반적으로 신경망은 뉴런들을 **레이어**로 구성합니다. 공통 입력 집합을 가진 선형 유닛을 모을 때, 여러분은 **dense** 레이어를 얻게 됩니다.

![img](https://blog.kakaocdn.net/dn/vcbWZ/btrgnrn7R9w/0c9geggvQ8lYNzhMp1j941/img.png)두 개의 입력과 바이어스를 받는 두 개의 선형 유닛으로 구성된 dense 레이어

신경망의 각 레이어가 비교적 단순한 변형을 수행한다고 생각할 수 있습니다. 레이어의 깊은 누적을 통해 신경망은 더더욱 복잡한 방법으로  그 입력 값을 변형할 수 있습니다. 잘 학습된 신경망에서 각 레이어는 여러분이 조금씩 해답에 가까워지도록 변형할 것입니다.



> 많은 종류의 레이어들
>
> Keras에서 레이어는 굉장히 일반적인 것입니다. 레이어는 본질적으로 어떤 종류의 데이터 변환일 수 있습니다. 합성곱convolutional  레이어와 순환recurrent 레이어와 같은 많은 레이어들은 뉴런을 통해해 데이터를 변환하고, 주로 그들이 형성하는 연결 패턴에  차이가 있습니다. 하지만, 다른 것들은 feature engineering 이나 단순 산술법에 차이가 있습니다. 여러분에게 도움될 레이어가 아주 많이 있습니다. [여기](https://www.tensorflow.org/api_docs/python/tf/keras/layers)를 확인해보세요!

(합성곱은 하나의 함수와 또 다른 함수를 반전 이동한 값을 곱한 다음, 구간에 대해 적분하여 새로운 함수를 구하는 수학 연산자입니다.)

![img](https://blog.kakaocdn.net/dn/Pa2WN/btrgoePDeNj/49BABfFxoyMMfPP5SiPRj1/img.png)합성곱 연산을 설명하는 그래프(출처:https://ko.wikipedia.org/wiki/합성곱)

### 활성 함수The Activation Function

하지만, 그 사이에 아무것도 없는 두 개의 dense 레이어는 그 자체로는 하나의 dense 레이어보다 전혀 낫지 않다는 것이  밝혀졌습니다. Dense 레이어들은 그 자체만으로는 여러분을 선과 평면의 세상에서 절대 나오게 할 수 없습니다. 우리에게 필요한  것은 *비선형*의 무언가입니다. 우리가 필요한 것은 바로, 활성 함수들입니다.

![img](https://blog.kakaocdn.net/dn/8xzwD/btrgoqoQpA9/Px8tYeljqZoIFLVKuJPdY0/img.png)활성 함수를 빼놓고, 신경망은 단지 선형 관계들만을 학습할 뿐입니다. 곡선에 맞게 학습하려면, 우리에게는 활성 함수가 필요할 것입니다.

활성 함수는 단순히 여러분이 각각의 레이어들의 출력에 적용할 수 있는 어떤 함수입니다. 가장 일반적인 것은 정류rectifier 함수 max(0, x)입니다.

![img](https://blog.kakaocdn.net/dn/BWkvI/btrgh3IGmGq/kxk6IXDee4K7XKI6oNMiaK/img.png)

정류 함수는 음수 부분이 0으로 정류된 하나의 선으로 된 그래프를 가지고 있습니다. 뉴런의 출력에 함수를 적용하면 데이터에 굴곡이 생겨서 단순한 '선'에서 멀어지게 됩니다.



여러분이 선형 유닛에 정류 함수를 적용할 때, 우리는 정류 선형 유닛Rectified Linear Unit 즉, ReLU를 얻게  됩니다.(이번 강의에서 정류 함수를 ReLU 함수라고 부르는 것이 일반적입니다.) 선형 유닛에 ReLU 활성화를 적용하면 출력은  max(0, w*x+b)가 되며, 이는 아래와 같은 다이어그램으로 그릴 수 있습니다.

![img](https://blog.kakaocdn.net/dn/bjUuhA/btrgogfFOPA/nfSKmZ96uOOYzgncHAOfA0/img.png)정류 선형 유닛Rectified Linear Unit(ReLU)

### Stacking Dense Layers

이제 여러분은 약간의 비선형성을 갖게 되었습니다. 더 복잡한 데이터 변환을 얻기 위해 어떻게 레이어들을 쌓는지 알아보겠습니다.



![img](https://blog.kakaocdn.net/dn/4Zd0w/btrgoArvDaw/jW4NqxeBXStkLNFKmp2KpK/img.png)Dense 레이어의 누적으로 만든 완전 연결 신경망

그출력 레이어 전에 있는 레이어들은 그 출력을 절대 바로 볼 수 없기 때문에 종종 숨겨져있다(hidden)고 합니다.



이제, 마지막 레이어가 활성 함수가 없는 하나의 선형 유닛이라는 것을 알아두셔야 합니다. 그것은 이 신경망이 회귀regression  작업에 적합하도록 만듭니다. 여기에서 여러분은 임의의 숫자 값을 예측하고자 하기 때문이죠. 분류Classification와 같은  작업에서는 아마 출력에 활성 함수가 요구될 수도 있습니다.



### Sequential 모델 만들기

여러분이 이전 수업에서 한 번 사용해 보셨던 Sequential 모델은 첫번째부터 끝까지의 레이어 리스트를 하나로 연결할 것입니다.  첫번째 레이어는 입력을 받고, 마지막 레이어는 출력을 만들죠. 아래의 코드는 위 그림과 같은 모델을 만듭니다.

```
from tensorflow import keras
from tensorflow.keras import layers

model = keras.Sequential([
	# 숨겨진 ReLU 레이어들
    layers.Dense(units=4, activation='relu', input_shape=[2]),
    layers.Dense(units=3, activation='relu'),
    # 선형 출력 레이어
    layers.Dense(units=1),
])
```

레이어를 [layer, layer, layer, ...]와 같은 리스트 형태로 사용하는 것을 꼭 기억하세요! 구별된 인자로 주지 않습니다! 활성 함수를 레이어에 추가하려면, 그냥 **activation** 인자에 활성 함수의 이름을 넣어주세요.

------

이렇게 캐글(Kaggle)의 Intro to Deep Learning 두번째 수업을 번역하며 공부해봤습니다.



아무래도 번역하지 않는 것이 자연스러운 단어가 더 많고 익숙하다보니, 번역을 생략한 부분도 좀 있습니다.



오늘 배운 내용을 요약하자면,

- 공동의 입력을 갖는 선형 유닛들로 레이어(layer, 층)가 구성된다.
- 레이어를 단순히 깊게(많이) 쌓는다고 해도 품질은 다르지 않다. 이를 위해서는 활성 함수Activation Function이 필요하다.
- 이 활성 함수의 대표적인 종료로 ReLU 함수가 있는데, ReLU는 음수 부분이 모두 0으로 '정류된rectifiered' 유닛을 의미한다.
- ReLU로 얻은 비선형성과 여러겹의 Dense 레이어를 통해 단순한 선형 모델보다 더 좋은 모델을 만들 수 있다.
- 이 때, 출력 이전의 그 출력 값을 알기 어려운 레이어를 Hidden 레이어라고 부른다.
- keras의 Sequential([layer, layer, layer, ...])을 통해 이 모델을 만들 수 있고, activation="활성 함수 이름"으로 활성 함수를 지정할 수 있다.

혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다.



출처 : https://www.kaggle.com/ryanholbrook/deep-neural-networks
