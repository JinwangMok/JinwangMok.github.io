---
layout: single
title: "Pandas(4)"
categories: mldl
tag: [kaggle, pandas, pandas groupby(), pandas multi-index, pandas sort, pandas 그룹, pandas 정렬, 캐글, 캐글 pandas, 캐글 코스 번역]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 그룹과 정렬

데이터셋이 복잡할수록 집중해야할 것



### 소개

매핑Maps 메소드는 데이터프레임이나 시리즈 안에 있는 데이터의 전체 열에 대해 한 번에 변형시킬 수 있게 해줬지만, 여러분은 종종 데이터를 그룹화하고 데이터가 있는 그룹에 특정한 작업을 수행하고 싶을 것입니다. 



이제 배울 것이지만, groupby() 명령을 통해 이 작업을 수행할 수 있습니다. 또한 어떻게 데이터를 정렬하는지와 함께, 여러분의 데이터프레임을 찾아보는 더 복잡한 방법과 같은 추가적인 주제에 대해서도 다뤄볼 것입니다.



exercise를 시작하려면 [여기](https://www.kaggle.com/jinwangmok/exercise-grouping-and-sorting/edit)를 눌러주세요! (이 글은 Kaggle의 코스를 번역한 글입니다.)



### 그룹단위 분석

```python
import pandas as pd
reviews = pd.read_csv('../input/wine-reviews/winemag-data-130k-v2.csv', index_col=0)
pd.set_option('display.max_rows', 5)
```

지금까지 우리가 앞서 많이 사용한 함수 중 하나는 value_counts() 함수입니다. 우리는 아래의 함수로 value_counts() 함수가 하는 일을 따라할 수 있습니다.

```python
reviews.groupby('points').points.count()
```



groupby()는 reviews에서 와인이 받은 점수가 같은 것에 따라 할당된 그룹을 만듭니다. 그리고나서, 이 만들어진 그룹들에 대해서  points() 열을 선택하고 해당 점수 값이 얼마나 많이 등장했는지를 카운트합니다. 즉, 87점인 와인들끼리 묶고, 88점인  와인들끼리 묶고... 이렇게 만들어진 그룹에 대해 각 점수가 얼마나 등장했는지 확인하면, 각 고유값이 몇 번 등장했는지 확인할 수 있게 됩니다. value_counts()는 이 동작의 단축어일 뿐입니다.



여러분은 이 데이터셋으로 이전에 사용해봤었던 어떤 요약 함수도 사용할 수 있습니다. 예를 들어, 각 점수 값 카테고리 중 가장 싼 와인을 얻기 위해서는 다음과 같은 코드를 쓸 수 있습니다.

```python
reviews.groupby('points').price.min()
```



여러분이 생성한 각각의 그룹에 대해서 일치하는 값을 가진 데이터만 포함하는 데이터프레임의 조각이라고 생각할 수 있습니다. 이  데이터프레임은 apply() 메소드를 사용하여 직접 접근이 가능하고, 이를 통해 적합하다고 보는 어떤 방식으로든 데이터를 조작할 수 있습니다. 예를 들어, 데이터셋의 각 와이너리의 첫번째 리뷰된 와인의 이름을 선택하는 방법 중 하나는 아래와 같습니다.

```python
reviews.groupby('winery').apply(lambda df : df.title.iloc[0])
```



좀 더 세밀하게 조작하려면, 하나의 열보다 더 많은 열을 기준으로 그룹을 만들 수 있습니다. 예를 들어, 아래는 country와 province를 통해 가장 좋은 와인을 선택해 내는 방법입니다.

```python
reviews.groupby(['county', 'province']).apply(lambda df : df.points.idxmax()])
```



다룰 가치가 있는 또다른 groupby() 메소드는 agg()입니다. agg()는 데이터프레임에 동시에 여러가지 다른 함수를 실행할 수 있게 해주는 메소드입니다. 예를 들어, 아래의 코드를 통해 데이터셋에서 단순한 통계 요약을 생성해 낼 수 있습니다.

```python
reviews.groupby(['country']).price.agg([len, min, max])
```



효과적인 groupby()의 사용은 여러분의 데이터를 통해 강력한 일들을 많이 할 수 있게 만들어 줄 것입니다.



### 다중 인덱스Multi-indexes

지금까지 살펴본 모든 예시에서는 데이터프레임 또는 시리즈 객체를 single-label 인덱스를 통해 작업해왔습니다. groupby()는 실행하는 동작에 따라 사실 살짝 다릅니다. 그것은 때때로 다중 인덱스multi-index라고 불리는 것을 초래할 것입니다.



다중 인덱스는 다중 수준을 갖는다는 점에서 일반적인 인덱스와 다릅니다. 예시를 보겠습니다.

```python
countries_reviewed = reviews.groupby(['country', 'province']).description.agg([len])
countries_reviewed
```

(결과는 아래 링크를 따라 원문에서 확인해주세요 :D)

```python
mi = countries_reviewed.index
type(mi)

# pandas.core.indexes.multi.MultiIndex
```

다중 인덱스는 그 나눠진 구조를 다루기 위해 single-level에는 없는 몇가지 메소드를 가지고 있습니다. 이들은 또한 값을  반환하기 위해 두 수준의 레이블label이 필요합니다. 다중 인덱스의 결과를 다루는 것은 Pandas 뉴비에게 일반적인  걸림돌입니다.



다중 인덱스의 사용 사례는 Pandas의 documentation의 [MultiIndex / Advanced Selection](https://pandas.pydata.org/pandas-docs/stable/user_guide/advanced.html)에 사용 지침과 함께 자세히 설명되어 있습니다.



어쨋거나, 일반적으로 여러분이 가장 많이 사용할 다중 인덱스 메소드는 일반적인 인덱스로 다시 변환시키는 reset_index() 메소드일 것입니다.

```python
countries_reiviewed.reset_index()
```



### 정렬Sorting

다시 countries_reviewed를 살펴보면 그룹화가 데이터를 값에 의한 정렬이 아니라 인덱스에 의한 정렬로 반환하는 것을 볼 수 있습니다. 다시말해 groupby의 결과를 출력할 때, 행의 순서는 안에 들어있는 데이터의 값이 아닌, 인덱스의 값에 따라  정렬된다고 할 수 있습니다.



순서대로 정렬된 데이터를 얻기 위해서 sort_values() 메소드를 통해 아래와 같이 직접 분류할 수 있습니다.

```python
countries_reviewed = countries_reviewed.reset_index()
countries_reviewed.sort_values(by='len')
```



sort_values() 는 기본적으로 가장 낮은 값이 처음에 오는 오름차순입니다. 하지만, 많은 경우 높은 값이 먼저오는 내림차수으로 정렬하기 원할 것입니다. 이 때는 아래와 같이 할 수 있습니다.

```python
countries_reviewed.sort_values(by='len', ascending=False)
```



또, 인덱스에 의한 정렬을 하고 싶을 때는 동료 메소드인 sort_index()를 사용할 수 있습니다. 이 메소드는 동일한 기본 정렬(오름차순)과 인자를 가집니다.

```python
countries_reviewed.sort_index()
```



마지막으로 한 번에 하나의 열 이상의 기준으로 정렬할 수 있다는 것을 알아두세요.

```python
countries_reviewed.sort_values(by=['country', 'len'])
```

------

이렇게 캐글(Kaggle)의 Pandas 네번째 수업을 번역하며 공부해봤습니다.



오늘 배운 내용을 요약하자면, 

- groupby() 메소드를 통해 값이 일치하는 행끼리 그룹을 만들 수 있다.
    - 이렇게 만들어진 그룹은 데이터프레임 형태이고, 여기에 여러가지 통계 함수를 적용하거나, apply() 메소드로 함수를 적용할 수 도 있다.
    - 또한, agg() 메소드를 통해 여러가지 함수를 동시에 적용할 수 있다.
- groupby()는 종종 multi-Index를 반환할 것인데, 이는 여러 수준을 갖는다는 점에서 일반적인 인덱스와 다르다.
    - 이 때 많이 사용할 메소드가 reset_index() 메소드로 이는 multi-Index를 일반 인덱스 형태로 바꿔줄 것이다.
- 이렇게 그룹화된 데이터프레임은 기본적으로 인덱스 기준으로 정렬이 되어있다.
    - 이를 값을 기준으로 정렬하는 메소드가 sort_values()이고, 여기에 인자로 by='len' 등으로 기준치를 설정할 수 있으며 ascending=False로 내림차순 정렬을 할 수 있다.
    - 인덱스 기준으로 정렬하는 메소드는 sort_index() 이고, 인자나 기본 정렬 등이 sort_values()와 동일하다.
    - 키워드 매개변수 by에 단일 column의 키(문자열)를 넣을 수 도 있지만, 여러기준으로 정렬할 때는 리스트 내에 열 이름을 넣어서 정렬할 수 도 있다.

혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다.



출처 : https://www.kaggle.com/residentmario/grouping-and-sorting
