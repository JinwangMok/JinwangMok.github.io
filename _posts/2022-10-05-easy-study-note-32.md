---
layout: single
title: "C++ 접근 지정자"
categories: cpp
tag: [C++, C++ private, C++ protected, C++ public, C++ 접근지정자, C++ 캡슐화]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 접근 지정자

C++ 의 접근 지정자는 객체지향의 중요한 특징인 캡슐화를 지원하는 역할을 한다.



이 때, 캡슐화란 알약의 내용물이 캡슐로부터 보호받는 것처럼 객체를 외부로부터 보호하는 것을 의미하는데, 구체적으로는 외부로부터  객체의 중요한 데이터에 접근하는 것을 막아 데이터의 신뢰도를 높이고, 접근해도 괜찮은 데이터는 외부에서도 접근 가능하도록 설계하는 것을 뜻한다.



### 접근 지정자의 종류

접근 지정자의 종류로는 private, public, protected 세가지가 있는데 각각의 접근 권한은 다음과 같다.

- private : 동일한 클래스 멤버 함수만 접근 가능
- public : 외부 함수도 접근 가능
- protected : 자신과 자신을 상속받은 클래스만 접근 가능

마지막으로 코드 예시는 다음과 같다.

```cpp
class myObj{
    private:
        //멤버 함수만 접근 가능
    public:
        //외부 함수도 접근 가능
    protected:
        //자신의 멤버 함수와 자신을 상속받은 객체에서 접근 가능
};
```
