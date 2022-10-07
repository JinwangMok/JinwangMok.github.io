---
layout: single
title: "Pandas(1)"
categories: mldl
tag: [Artificial Intelligence, Data Analysis, Kaggle, Machine Learning, pandas, pandas csv, pandas 데이터프레임, pandas 시리즈, 데이터분석, 캐글]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 만들기, 읽기, 쓰기

Pandas는 데이터 애널리시스에게 가장 인기있는 파이썬 라이브러리이다.



### 시작

먼저, Pandas를 쓸 때 전형적으로 아래와 같은 코드를 삽입한다.

```python
import pandas as pd
```



### 데이터 만들기

판다스에는 가장 핵심이 되는 두가지 오브젝트가 있는데, 바로 데이터프레임DataFrame과 시리즈Series다.



**데이터프레임**은 표table다. 여기에는 개별 항목의 배열이 포함되어 있으며, 각 항목은 특정 값을 갖는다.

예를 들면, 아래의 코드처럼 사용할 수 있다.

```python
pd.DataFrame({'Yes' : [50, 21], 'No' : [131, 2]})
```

|      | Yes  | No   |
| ---- | ---- | ---- |
| 0    | 50   | 131  |
| 1    | 21   | 2    |

데이터프레임의 개별 항목은 정수 이외에도 문자열 등이 들어갈 수 있다.



데이터프레임 오브젝트는 pd.DataFrame() 생성자로 만들 수 있다. 이 안에 들어갈 문법은 딕셔너리dictionary 자료형으로 볼 수 있는데, **키**에 해당하는 문자열이 표에서 가장 윗 행에 있는 **열의 이름**이고, **값**에 해당하는 리스트 내의 각 항목들이 **해당 열의 각 튜플 값**이다. 이 형태가 새로운 데이터프레임을 생성하는 전형적인 방법이고 또 당신이 가장 많이 볼 형태다.



이렇게 딕셔너리만 사용해서 데이터프레임을 만들면 행번호는 그냥 0, 1, 2 ..이렇게 오름차순 숫자로 매겨질 것인데, 만약 행번호에  특정한 라벨을 할당하고 싶다면 index 매개변수를 DataFrame() 생성자에 전달하면 된다.

```python
pd.DataFrame({'Bob' : ['I liked it.', 'It was awful'],
			  'Sue' : ['Pretty good.', 'Bland']},
              index=['Product A', 'Product B'])
```

보이는 것처럼 index 파라미터에 넘길 값은 리스트 형태로 주어져야하고, 리스트의 각 요소는 0번 인덱스부터 0번 행번호에 할당될 것이다.



**시리즈**

이와 반대로 시리즈는 데이터 값의 시퀀스sequence다. 데이터프레임이 표라면, 시리즈는 하나의 리스트다. 그리고, 실제로 하나의 리스트로 시리즈를 만든다.

```python
pd.Series([1, 2, 3, 4, 5])
```

본질적으로 시리즈는 데이터프레임의 하나의 열이다. 따라서, 앞서 했던 것처럼 행 번호를 index 매개변수로 전달할 수 있다. 하지만,  시리즈는 열 이름이 없고 단지 시리즈의 이름에 해당하는 name이 있을 뿐이다.(근데, 이게 뭐 열 이름이라고 생각해도 무방할 것 같긴한데 여튼..)

```python
pd.Series([30, 35, 40], index=['2015 Sales', '2016 Sales', '2017 Sales'], name='Product A')
```



시리즈와 데이터프레임은 긴밀히 연관되어있다. 데이터프레임이 사실은 시리즈들 덩어리를 풀로 붙여놨다고 생각하는게 도움이 될 것이다. 이에 대해서는 뒤에서 더 살펴보자.



### 데이터 파일 읽기

데이터프레임이나 시리즈를 직접 만드는 것은 편리하다. 하지만, 대부분의 경우 우리가 실제로 직접 사용할 데이터를 만들지 않는다. 대신 이미 있는 데이터를 가지고 작업을 하게 될 것이다.



데이터는 다양한 형태와 포맷으로 저장될 수 있다. 그중 가장 기본이 되는 것은 CSV 파일이다. 당신이 CSV 파일을 열 때 아래와 같이 생긴 것을 볼 수 있을 것이다.

```
Product A,Product B,Product C,
30,21,9,
35,34,1,
41,11,11
```

CSV 파일은 표table의 값을 콤마(,)로 나눈 파일이기 때문이다. 그래서 이름이 CSV, Comma Separated Values 인 것이다.



이제 우리의 장난감 같은 데이터에서 벗어나서 우리가 데이터프레임으로 실제 데이터 셋을 불러올 때 어떻게 생겼는 알아보자. 우리는 데이터를 데이터프레임으로 읽어오기 위해서 pd.read_csv()라는 메소드를 사용할 것이다.

```python
wine_reviews = pd.read_csv('../input/wine-reviews/winemag-data-130k-v2.csv')
```

그리고 shape이라는 속성을 사용해서 데이터프레임의 결과가 얼마나 큰 지 볼 수 있다.

```python
wine_reviews.shape
```

이 결과로는 (레코드의 수, 속성의 수)를 확인할 수 있다.

이 결과 130,000개의 레코드가 있고, 14가지 속성으로 이루어져 있음을 볼 수 있다. 따라서, 거의 2백만 개의 항목(엑셀로 치면 셀)이 있는 것이다! 

우리는 head() 명령을 사용해서 결과 데이터프레임의 내용 중 첫번째 5개 행만 가져와서 확인할 수 있다.

```python
wine_reviews.head()
```

pd.read_csv() 메소드는 30개가 넘는 옵션 매개변수들을 지원한다. 예를 들어, 위 데이터셋을 실제로 살펴보면 Pandas가 자동으로 선택해서  사용하지 않은 내장 인덱스 열이 unnamed:0으로 있음을 알 수 있다. 새로 index열을 만드는 대신 Pandas가 이 열을 index로 사용하게 하려면, 아래와 같이 index_col을 명시해주면 된다.

```python
wine_reviews = pd.read_csv('../input/wine-reviews/winemag-data-130k-v2.csv', index_col=0)
wine_reviews.head()
```

------

이렇게 캐글(Kaggle)의 Pandas courses를 번역하며 공부해봤습니다.



오늘 배운 내용을 요약하자면,

- Pandas에는 DataFrame과 Series라는 두가지 주요 데이터 타입이 있다.
- DataFrame은 테이블(표)이고, pd.DataFrame()에 key(열이름) : values[값1, 값2..] 형태의 딕셔너리를 넘기므로 만들 수 있다.
    - 여기에 index=[1행 인덱스, 2행 인덱스 ..] 매개변수를 지정해서 인덱스를 직접 설정할 수 도 있다.
- Series는 DataFrame의 단일 열 정도로 생각하면 되며, pd.Series()에 리스트 형태의 값 목록을 넣어 만들 수 있다.
    - 마찬가지로 index 매개변수로 인덱스를 직접 설정할 수 있다.
    - name 매개변수에 값을 지정하여 Series의 이름 즉, 열 이름을 설정할 수 도 있다.
- 가장 흔한 데이터 형식인 csv 파일을 읽어오는 코드는 pd.read_csv(파일 경로)이다.
    - 만약, csv에 인덱스에 해당하는 열이 있다면, read_csv() 메소드의 매개변수로 index_col=해당 열을 지정해주자.
- 데이터의 크기를 (행의 개수, 열의 개수)로 확인할 수 있는 코드는 shape 속성이고, my_df.shape과 같이 사용한다.
- DataFrame의 첫 5줄을 샘플로 가져오는 코드는 head() 메소드로, my_df.head()와 같이 사용한다.
- (추가로) 가공한 DataFrame을 csv파일로 내보내는 코드는 to_csv() 메소드이며 my_df.to_csv('test.csv')와 같이 사용한다.

혹시라도 잘못된 내용이 있다면, 말씀해주시면 감사하겠습니다.



출처 : https://www.kaggle.com/residentmario/creating-reading-and-writing
