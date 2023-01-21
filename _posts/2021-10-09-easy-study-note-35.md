---
layout: single
title: "Pandas(5)"
categories: mldl
tag: [kaggle, kaggle courses, pandas, pandas dtype, pandas fillna(), pandas replace(), 캐글, 캐글 강의, 캐글 코스 번역]
toc: true
toc_sticky: true
author_profile: true
search: true
typora-root-url: ../

---

## 데이터 타입과 결측값Data Types and Missing values

가장 일반적인 진행 방해 요소를 다뤄봅시다.



### 소개

이번 시간에는 데이터프레임이나 시리즈 내부의 데이터 타입을 조사하는 방법을 배울 것이고 또한, 항목을 찾고 교체하는 방법도 배울 수 있을 것입니다.

이 코스는 [exercise](https://www.kaggle.com/kernels/fork/598826)로 실습을 병행하시면 학습효과 더 좋습니다! (이 글은 캐글의 코스를 번역한 글입니다. 링크는 글 최하단에 있습니다.)

### Dtypes

데이터프레임이나 시리즈 안의 한 열에 대한 데이터 타입을 **dtype**이라고 합니다.

```python
import pandas as pd
reviews = pd.read_csv('../input/wine-reviews/winemag-data-130k-v2.csv', index_col=0)
pd.set_option('max_rows', 5)
```

특정한 열의 타입을 선택하려 할 때, **dtype** 속성을 사용할 수 있습니다. 예를 들어, 아래와 같이 reviews 데이터프레임 안에 있는 price 열의 dtype을 알아낼 수 있습니다.

```python
reviews.price.dtype
# dtype('float64')
```



또, dtype 속성을 데이터프레임 자체에 적용하려면 **dtypes** 속성을 사용할 수 있습니다. 이 경우 데이터프레임의 모든 열의 dtype을 반환합니다.

```python
reviews.dtypes
```



데이터 타입은 pandas가 내부적으로 어떻게 데이터를 저장하고 있는지에 대한 것들을 알려줍니다. float64는 64비트  부동소수점수64-bit floating point number를 의미하며, int64는 부동소수점수 대신 비슷한 크기의 정수인  것을 의미하는 것처럼 말이죠.

하나 기억해둬야할 특이한(그리고 여기에서 명확히 표시되는) 것은 열들은 전체적으로 그 타입대로 구성된 것이 아니라 문자열string으로 존재한다는 것입니다. 이것들은 대신 object 타입을 갖습니다.

변환하는 것이 논리적 오류가 없다면, astype() 함수를 사용하면 한 열의 dtype을 다른 타입으로 변환하는 것도 가능합니다. 예를 들어, int64 데이터 타입으로 존재하는 points 열을 float64 데이터 타입으로 변환하는 것이 가능합니다.

```python
reviews.points.astype('float64')
```



데이터프레임이나 시리즈의 인덱스 또한 dtype을 가지고 있습니다.

```python
reviews.index.dtype
# dtype('int64')
```



또한, Pandas는 카테고리형categorical 데이터나 시간표현timeseries 데이터와 같은 외래exotic 데이터 타입도  지원합니다. 하지만, 이런 데이터 타입들이 더 가끔 쓰이기 때문에 지금은 넘어가고 더 뒤에서 다루도록 하겠습니다.

### 결측 데이터Missing data

결측값인 항목은 NaN이라는 값이 주어지고, 이는 Not a Number의 준말입니다. 기술적인 이유로 이 NaN 값들은 항상 float64 dtype입니다.

Pandas는 결측 데이터에 특화된 몇가지 메소드를 제공합니다. NaN 항목을 선택하려면 pd.isnull()을 사용할 수 있습니다. (또는 그 짝인 pd.notnull()을 사용할 수 도 있겠지요.) 이것은 다음과 같이 사용하도록 되어 있습니다.

```python
reviews[pd.isnull(reviews.country)]
```

이 경우 reivews 데이터프레임에서 country 열의 값이 NaN인 row를 선택하겠지요?



결측값을 대체하는 것은 일반적인 작업입니다. Pandas는 이 문제에 대해 정말 손쉬운 메소드를 제공합니다. 바로,  fillna()입니다. fillna()는 이런 데이터를 줄이는 몇가지 다른 전략을 제공합니다. 예를 들어, 각 NaN을  "Unknown"으로 아래와 같이 간단히 바꿀 수 있습니다.

```python
reviews.region_2.fillna("Unknown")
```



또는 각 결측값을 데이터베이스에서 주어진 레코드record 이후 언젠가 나타나는 null이 아닌 첫번째 값으로 채울 수 도 있습니다. 이것은 매우기backfill 전략이라고 알려져있습니다.



아니면, 바꾸고자하는 null이 아닌 값을 가지고 있을 수 있겠죠. 예를 들어, 이 데이터셋이 만들어진 후에 리뷰어인 Kerin  O'Keefe가 그녀의 트위터 태그를 @kerinokeefe에서 @kerino로 바꿨다고 가정해봅시다. 이것을 데이터셋에 반영하는 한 가지 방법은 아래와 같이 replace() 메소드를 사용하는 것입니다.

```python
reviews.taster_twitter_handle.replace("@kerinokeefe", "@kerino")
```



replace() 메소드는 데이터셋 안에 "Unknown", "Undisclosed", "Invalid"와 같은 구별되어진 결측값들을 교체하기 편하기 때문에 언급할 가치가 있습니다.

------

이렇게 캐글(Kaggle)의 Pandas 다섯번째 수업을 번역하며 공부해봤습니다.

오늘 배운 내용을 요약하자면,

- 데이터프레임이나 시리즈에서 각 열(attributes, features)의 데이터 타입을 dtype이라고 하고, 여기에는 int64, float64 등이 있다.
    - 알아내는 방법은 시리즈.dtype 또는 데이터프레임.dtypes로 알아낼 수 있다. 이 때, 열column은 object 타입을 가진다.
    - 데이터 타입을 변경할 때는 int에서 float처럼 논리적으로 가능하다면, astype()으로 변경할 수 있다.
    - 인덱스도 이 dtype을 가지고 있고, float64이다.
    - Pandas는 int64나 float64 같은 타입 이외에도 외부 데이터 타입(카테고리, 시간정보 등)도 지원한다.
-  결측값을 알아낼 때는 isnull()이나 이 정반대인 notnull() 메소드를 사용할 수 있다. 또한, 이 결측값이 NaN인 경우  fillna()를 통해 값을 넣어줄 수 있고, replace('a', 'b')를 통해 원래 있는 a 라는 값을 b로 바꿀 수  있다.

혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다.

출처 : https://www.kaggle.com/residentmario/data-types-and-missing-values
