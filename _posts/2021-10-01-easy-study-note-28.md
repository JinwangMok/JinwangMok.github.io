---
layout: single
title: "Pandas(3)"
categories: mldl
tag: [kaggle, pandas, pandas apply(), pandas map(), pandas 매핑, pandas 연산, pandas 요약 함수, 캐글, 캐글 pandas, 캐글 코스 번역]
toc: true
toc_sticky: true
author_profile: true
search: true
typora-root-url: ../

---

## 요약 함수Summary Function와 매핑Maps

여러분의 데이터에서 유의미한 통찰들을 추출해보세요.

### 소개

여러분은 직전 튜토리얼에서 데이터프레임DataFrame이나 시리즈Series에서 관련 데이터를 어떻게 선택하는지를 학습했습니다.  exercise에서 증명했다시피 올바른 데이터를 데이터셋에서 골라내는 것은 작업을 끝내는 데에 있어서 중요합니다.



하지만, 데이터가 항상 원하는 형식format으로 메모리에서 나오는 것은 아닙니다. 때때로 이 데이터를 작업에서 사용할 수 있도록 형식을 수정하는 일을 해야만 합니다. 이 튜토리얼에서는 입력을 "딱 맞게" 얻기 위해 데이터에 적용할 수 있는 여러 다른 명령들을 다룰 것입니다.



이 주제에 대한 exercise를 진행하시려면, [여기](https://www.kaggle.com/jinwangmok/exercise-summary-functions-and-maps/edit)를 눌러주세요!(누차 말씀드리지만, 이 글은 캐글Kaggle의 코스를 번역한 것입니다.)



증명을 위해 Wine Magazine 데이터를 사용하겠습니다.

```python
import pandas as pd
pd.set_option('max_rows', 5)
import numpy as np
reviews = pd.read_csv('../input/wine-reviews/winemag-data-130k-v2.csv', index_col=0)
reviews
```

이 결과로는 약 13만 개의 행과 13개의 열이 나옵니다. 자세한 내용은 아래 출처의 원본을 따라가셔요 :D



### 요약 함수Summary function

Pandas는 데이터를 좀 유용한 방법으로 재구성하는 여러 간단한 '요약 함수'(공식적인 이름은 아니에요.)들을 제공합니다. 예를 들어, describe() 메소드가 있겠지요.

```python
reviews.points.describe()
```

이 결과로는 reviews 데이터프레임의 points 열에 대한 여러 요약 정보들이 출력됩니다. 이 메소드는 주어진 열의 속성에  대한 높은 수준의 요약 정보를 생성합니다. 이 메소드는 유형을 인식type-aware하는데, 이는 입력의 데이터 유형에 기반하여  출력의 그것이 변경된다는 뜻입니다. 위의 결과는 수치 데이터에만 해당됩니다.(아무래도 points, 점수에 해당하니까요.) 문자열 데이터에 대해서는 아래의 코드에서 얻을 수 있습니다.

```python
reviews.taster_name.describe()
```



만약 데이터프레임이나 시리즈의 열에 대해 어떤 특별하고 간단한 요약 통계를 얻고 싶다면, 대게는 이를 실현시키는데 도움이 되는 Pandas 함수가 존재합니다.



예를 들어, 할당된 점수들의 평균 즉, 평균적으로 평가된 와인이 얼마나 좋은지를 확인하려면, mean() 함수를 사용할 수 있습니다.

```python
reviews.points.mean()
```



또, 유일한 값을 가지는 리스트를 확인하려면, unique() 함수를 사용할 수 있습니다.

```python
reviews.taster_name.unique()
```



그리고, 유일한 값을 가지는 리스트와 그 값이 데이터셋에서 얼마나 자주 등장하는지를 확인하려면, value_counts() 함수를 사용할 수 있습니다.

```python
reviews.taster_name.value_counts()
```



### 매핑Maps

Map은 수학에서 빌려온 용어로 함수에서 값들에 대한 하나의 집합과 값들에 대한 또다른 집합을 서로 매핑(연결)하는 것을 의미합니다.  (고로 매핑이라고 편하게 이야기 하겠습니다.) 데이터 사이언스 분야에서는 존재하는 데이터로부터 새로운 표현을 만들어내거나,  데이터를 현재 형식format에서 추후에 원하는 형식으로 변형시키려는 필요를 종종 가집니다. 매핑은 여러분의 작업을 끝내는데에  굉장히 중요한 이 작업을 다루는 것입니다.



여러분이 자주 사용할 매핑 메소드는 두가지가 있습니다.



첫번째로 좀 더 단순한 **map()**을 살펴보겠습니다. 예를 들어서, 우리가 와인의 점수를 다시 0으로 정의하기 원한다고 하면 아래와 같이 할 수 있습니다.

```python
reviews_points_mean = reviews.points.mean()

reviews.points.map(lambda p : p - review_points_mean)
```



map()을 통해 전달한 함수는 시리즈로부터 하나의 값(위의 예시의 점수 값)을 예상할 수 있고, 그 값을 변형시킨 것을 반환할 것입니다.  map()은 lambda 함수로 변형된 모든 값들로 구성된 새로운 시리즈를 반환할 것입니다.



두번째로 **apply()**는 만약 전체 데이터프레임을 각각의 행에 커스텀 메소드를 호출하므로써 변형시키고자 할 때 알맞은 메소드입니다.

```python
def remean_points(row):
	row.points = row.points - review_points_mean
    return row

reviews.apply(remean_points, axis='columns')
```



만약 reviews.apply()를 axis='index' 로 부른다면, 각 행을 변형시키는데 함수를 적용하지 않는 대신, 각 열을 변형시키기 위해 함수를 호출할 것입니다.



map()과 apply() 둘 다 각각 새롭게 변형된 데이터프레임과 시리즈를 반환한다는 것을 알아두세요. 이 두 메소드가 호출될 때, 원본  데이터를 수정하지 않습니다. reviews의 첫번째 행을 보면, 여전히 원본 points(점수) 값을 가진 것을 확인할 수 있죠.

```python
reviews.head(1)
```



Pandas는 여러 내장 공통 매핑 연산을 지원합니다. 예를 들어, 아래는 points 열을 재정의하는 좀 더 빠른 방법입니다.

```python
review_points_mean = reivews.points.mean()
reviews.points - review_points_mean
```



여러분은 이 코드에서 여러 값(시리즈 안에 있는 모든 것)을 가지는 좌항과 하나의 값을 가지는 우항 사이의 연산을 수행하고 있습니다. Pandas는 이 표현을 보고 데이터셋의 모든 값에서 평균 값을 빼고자 한다는 것을 알아냅니다.



또한, Pandas는 시리즈와 그와 동일한 길이의 값 사이에서 위의 연산을 통해 무엇을 하고자 하는지를 이해합니다. 예를 들어서,  데이터셋의 country와 region 정보를 결합하는 쉬운 방법으로 아래와 같이 명령할 수 있습니다.

```python
reviews.country + " - " + reviews.region_1
```



이 연산은 map()이나 apply()보다 빠릅니다. 왜냐면, 이 연산은 Pandas의 내장 스피드 업을 사용하기 때문이죠. 모든 파이썬의 일반적인 연산자들(>, <, == 등)은 이 형식에서 잘 동작합니다.



하지만, 이 연산은 map()이나 apply()처럼 유연하지는 못합니다. 이 두가지 메소드는 조금 더 심화된 동작을 수행할 수가 있습니다. 덧셈이나 뺄셈 하나만 쓰는 것으로는 할 수 없는, 조건문을 적용한다거나 하는 것들 말이죠.

------

이렇게 캐글(Kaggle)의 Pandas 세번째 수업을 번역하며 공부해봤습니다.



오늘 배운 내용을 요약하자면,

- 전체 데이터프레임을 모두 살펴보는 것은 사실상 어려우므로 요약해서 살펴보는 것이 효율적이라고 볼 수 있는데 이를 수행하는 메소드는 아래와 같은 것들이 있다.
    - describe(), mean(), unique(), value_counts()
- 데이터셋의 값 하나하나에 연산 등을 적용할 수 있는 방법이 매핑이며, 이를 수행할 때 map() 메소드와 apply() 메소드를 사용할 수 있다.
    - map() 메소드는 인자로 lambda 등을 통해 함수를 전달하면, 해당 함수의 연산을 선택된 열 등에 적용할 수 있다.
    - apply() 메소드는 만들어져있는 커스텀 함수를 전체 데이터프레임에 행 또는 열 방향으로 적용하고자 할 때 알맞다.
    - 이외에도 파이썬의 기본 연산자(+, -, >, <, == 등)를 좌항에 시리즈, 우항에 값(하나 또는 시리즈와 동일 길이)으로 적용할 수 있고, 속도도 위의 두가지 메소드보다 빠르지만 복잡한 연산은 불가능하다. 단지 덧셈 뺄샘 정도..

혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다.



출처 : https://www.kaggle.com/residentmario/summary-functions-and-maps
