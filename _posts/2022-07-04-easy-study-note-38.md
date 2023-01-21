---
layout: single
title: "파이토치 행렬곱 연산"
categories: mldl
tag: [matmul, tensor@, torch@, 파이토치@, 파이토치 @연산, 행렬 원소별 곱 연산, 행렬곱 연산]
toc: true
toc_sticky: true
author_profile: true
search: true
typora-root-url: ../
---

파이토치에는 @ 연산이 있다. 가령 아래와 같이 사용될 수 있다.

```python
t = torch.tensor([[1, 2], [3, 4]])
result = t @ t.T
```

이는 행렬의 곱을 의미한다. 즉, 아래의 두 연산의 결과는 동일하다.

```python
# 1. @ 연산
print(t @ t.T)

# 2. 텐서의 matmul 메소드
print(t.matmul(t.T))

# 출력
"""
tensor([[ 5, 11],
        [11, 25]])
"""
```



추가로 행렬의 원소별 곱 연산은 다음과 같다.

```python
print(t * t)
print(t.mul(t))

# 출력
"""
tensor([[ 1,  4],
        [ 9, 16]])
"""
```
