---
layout: single
title: "Intro to Machine Learning(2)"
categories: mldl
tag: [kaggle courses, Machine Learning, pandas, pandas describe, 기계학습, 머신러닝, 머신러닝기초, 인공지능, 인공지능기초, 캐글]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 기본 데이터 탐험

데이터를 불러오고 이해하기



### Pandas를 이용하여 데이터와 친해지기

어떤 기계학습 프로젝트든지 당신 스스로가 데이터와 친해지는 것이 첫걸음입니다. 당신은 이를 위해 Pandas 라이브러리를 사용할  것입니다. Pandas는 데이터 사이언티스트들이 데이터를 찾고 능수능란하게 다루기 위해 사용하는 주요 툴입니다. 대부분 코드에서  Pandas를 줄여서 pd라고 표현합니다. 이것을 아래와 같이 명령할 수 있습니다.

```python
import pandas as pd
```

Pandas 라이브러리의 가장 중요한 부분은 바로 데이터프레임(DataFrame)입니다. 데이터프레임은 대체로 당신이 표(table)라고  생각하는 데이터 타입을 가집니다. 이것은 엑셀의 sheet나 SQL 데이터베이스의 table과 비슷합니다.



Pandas는 당신이 이 데이터 타입으로 하고 싶은 왠만한 것들에 대해 강력한 메소드를 가지고 있습니다.



예를 들어, 호주 멜버른의 주택 가격의 대한 데이터를 살펴보겠습니다. 실습에서는 아이오와의 주택 가격이 있는 새 데이터 세트에 동일한 프로세스를 적용합니다.



예시 데이터(멜버른)는 ../input/melbourune-housing-snaphot/melb_data.csv에 있습니다.



아래의 코드로 데이터를 불러오고 찾아보겠습니다.

```python
# 쉬운 접근을 위해 파일 경로를 변수에 저장
melbourne_file_path = '../input/melbourne-housing-snapshot/melb_data.csv'

# melbourne_data라는 제목의 데이터 프레임으로 데이터를 읽고 저장
melbourne_data = pd.read_csv(melbourne_file_path)

# melbourne_data의 요약 내용 출력
melbourne_data.describe()
```



### 데이터 description 해석

위 코드로 출력한 결과에는 원본 데이터셋의 각 열(속성, attribute)의 8가지 **숫자**들을 보여줍니다. 먼저 첫번째 숫자, **count**는 각 열이 가진 행 중에 결측값을 제외한 개수를 보여줍니다.



결측값은 여러가지 이유로 나타납니다. 예를 들어, 두번째 침실은 한 침실짜리 주택에서는 없기 때문에 결측값이 됩니다. 결측 값에 대해서는 다음에 다룰 기회가 있을 것입니다.



두번째 숫자는 **mean**, 평균을 의미합니다. 그 밑에 **std**는 표준 편차를 의미하고 이는 값이 수치적으로 얼마나 분포되어 있는지를 나타냅니다.



**min**, **25%**, **50%**, **75%**, **max** 값을 해석하기 위해서는 각 열을 최소값에서 최대값으로 정렬한 것을 상상해야 합니다. 맨 처음(가장 작은) 값은 min입니다.  목록의 1/4만큼씩 이동한다면, 25%보다 크거나 75%보다 작은 값을 찾을 수 있을 것입니다. 이 때 첫번째 1/4의 값을  25%, 두번째 1/4(그러니까 1/2)의 값을 50%, 마지막 세번째 1/4 값을 75%라고 정의합니다. 그리고 가장 큰 값을 **max**로 나타냅니다.



------

이렇게 Intro to Machine Learning의 두번째 수업을 번역하며 공부해봤습니다.



혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다!



출처 : https://www.kaggle.com/dansbecker/basic-data-exploration
