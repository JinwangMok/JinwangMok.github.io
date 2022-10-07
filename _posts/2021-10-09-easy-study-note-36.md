---
layout: single
title: "Pandas(6)"
categories: mldl
tag: [kaggle, pandas, pandas concat(), pandas join(), pandas rename(), pandas rename_axis(), 캐글, 캐글 강의 번역, 캐글 코스 번역]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 이름 바꾸기 및 결합하기Renaming and Combining

데이터는 여러 곳으로부터 모입니다. 이들을 사용할 수 있게 만들어 봅시다.



### 소개

대부분 데이터는 열 이름, 인덱스 이름 또는 만족스럽지 않은 형태의 다른 이름 규칙과 함께 여러분에게 도착할 것입니다. 이런 경우,  어떻게  pandas 함수가 맘에 안드는 이름들에서 그것보다 나은 것으로 바꾸는지 배워야할 것입니다.

또, 어떻게 여러가지 데이터프레임 또는(혹은 그리고) 시리즈로부터 데이터를 결합해내는지에 대해 배워볼 것입니다.

(이 글은 캐글의 코스 중 pandas를 번역한 글입니다. 효과적인 학습을 위해 [exercise](https://www.kaggle.com/kernels/fork/638064)를 함께 해보시면 좋습니다! 강의의 출처는 글 최하단의 링크를 참조하세요.)

### 이름 바꾸기Renaming

```python
import pandas as pd
pd.set_option('max_rows', 5)
reviews = pd.read_csv('../input/wine-reviews/winemag-data-130k-v2.csv', index_col=0)
```



처음으로 소개할 함수는 rename()입니다. 이것은 인덱스 이름 또는(혹은 그리고) 열 이름을 바꿀 수 있게 해줍니다. 예를 들어, 데이터셋의  points 열을 score로 바꿀 때 다음과 같이 할 수 있습니다.

```python
reviews.rename(columns={'points': 'score'})
```



rename()은 인덱스 또는 열 키워드 매개변수를 각각 지정하여 인덱스 또는 열의 값을 변경할 수 있습니다. rename()은 다양한 입력  형식을 지원하지만, 보통 파이썬 딕셔너리 형식이 가장 편리합니다. 아래는 인덱스의 몇가지 요소를 rename하는 예시입니다.

```python
reviews.rename(index={0: 'firstEntry', 1: 'secondEntry'})
```





여러분은 아마 굉장히 자주 열의 이름을 바꿀 것입니다. 하지만, 인덱스 값의 이름을 바꾸는 것은 매우 가끔일 것입니다. 따라서, set_index()가 보통 더 편리합니다.

행 인덱스와 열 인덱스는 둘 다 각각 그들의 name 속성을 가질 수 있습니다. 무료로 쓸 수 있는 rename_axis()  메소드는 이들의 이름을 바꿀 수 있게 해줄 것입니다. 아래는 그 예시입니다. (행/열 인덱스에 이름 매기기 :  rename_axis())

```python
reviews.rename_axis('wines', axis='rows').rename_axis('fields', axis='columns')
```





### 결합하기Combining

데이터셋에서 명령들을 수행할 때, 종종 서로 다른 데이터프레임 또는(혹은 그리고) 시리즈를 사소하지 않은 방식으로 결합해야 할 수도  있습니다. Pandas는 이를 수행하는 세가지 주요 메소드를 가지고 있습니다. 복잡성이 증가하는 순서대로, concat(),  join() 그리고 merge() 메소드입니다. merge()가 할 수 있는 것 중 대부분은 join()으로 더 단순하게 할 수  있습니다. 따라서, merge()는 일단 제외하고 나머지 두 메소드를 살펴보겠습니다.

가장 단순한 결합 메소드는 concat()입니다. 요소들의 리스트가 주어지면, 이 함수는 그 요소들을 축에 따라 결합합니다. 

이 메소드는 서로 다른 데이터프레임 또는 시리즈 객체이지만, 동일한 필드(열)를 가지고 있는 데이터의 경우 유용합니다. 하나의 예로, 나라를 기준으로 데이터를 구분한 [유튜브 영상 데이터셋](https://www.kaggle.com/datasnaek/youtube-new)을 사용해보겠습니다. (예제에서는 Canada와 UK입니다.) 만약 동시에 여러 국가를 연구하고 싶다면, concat() 메소드를 통해 아래와 같이 이 데이터들을 결합시킬 수 있습니다.

```python
canadian_youtube = pd.read_csv('../input/youtube-new/CAvideos.csv')
british_youtube = pd.read_csv('../input/youtube-new/CBvideos.csv')

pd.concat([canadian_youtube, british_youtube])
```



결합 메소드를 복잡성에 따라 분류했을 때, 가운데에 있던 메소드는 join()입니다. join()은 공통 인덱스를 가진 서로 다른  데이터프레임 객체를 결합하도록 해줍니다. 예를 들어, 캐나다와 UK 두 곳에서 같은 날의 트렌딩 비디오를 선택할 때 다음과 같이 할 수 있습니다.

```python
left = canadian_youtube.set_index(['title', 'trending_data'])
right = british_youtube.set_index(['title', 'trending_data'])

left.join(right, lsuffix='_CAN', rsuffix='_UK') # suffix는 접미사
```



lsuffix와 rsuffix는 여기서 꼭 필요한 매개변수입니다. 왜냐하면 Canadian과 British 두 데이터셋 모두 동일한 열 이름을  갖고 있기 때문입니다. 이를 구분해줄 필요가 있지요. 만약 그렇지 않다면(왜냐면 예를 들어 사전에 이름을 바꿀 수 있으니까요),  lsuffix나 rsuffix는 별로 필요하지는 않습니다.

------

이렇게 캐글(Kaggle)의 Pandas 마지막 수업을 번역하며 공부해봤습니다.

오늘 배운 내용을 요약하자면,

- rename() 메소드를 사용해 열이나 인덱스의 이름을 변경할 수 있다.
    - 매개변수로 columns={현재 열 이름 : 바꿀 열 이름}을 건내서 열이름 변경할 수 있다.
    - 매개변수로 index={0: 이름, 1 : 이름 , ...}을 건내서 인덱스 값을 변경할 수 있지만, 차라리 set_index()를 쓰는게 더 편하다.
    - rename_axis(이름, axis='rows'또는'columns')로 행이나 열 인덱스의 이름을 정할 수 있다.
- 결합에는 concat(), join(), merge() 이렇게 있는데 merge()로 할 수 있는 건 join()으로 더 간단히 할 수 있다.
    - concat()은 매개변수로 리스트 안에 데이터프레임이나 시리즈로 된 요소들을 넣어 건내면 여러가지 개체를 결합할 수 있다.
    - join()은 호출하는 개체와 첫번째 인자로 주어지는 개체를 결합하며, 두 데이터프레임의 열 이름이 같은 경우 lsuffix와 rsuffix 키워드 매개변수로 접미사를 붙여 각 데이터를 구분할 수 있다.
    - concat()은 pandas 내장 메소드로 pd.concat()으로 사용하는 반면, join()은 객체 함수라서 특정 df.join()으로 사용하는 것 같다.

혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다.

출처 : https://www.kaggle.com/residentmario/renaming-and-combining
