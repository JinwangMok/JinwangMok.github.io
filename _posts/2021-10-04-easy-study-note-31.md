---
layout: single
title: "C++ 생성자/소멸자"
categories: cpp
tag: [C++, C++ 11, C++ 생성자, C++ 소멸자, C++ 위임 생성자]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

## 생성자

C++의 생성자는 객체가 생성되는 시점에 자동으로 호출되는 멤버 함수로 클래스 이름과 동일한 멤버 함수이다. 또, 함수 특유의 리턴 타입을 지정도 없다.(없을 뿐 아니라 void라도 있으면 안된다!)

```cpp
class Circle{
    Circle();
    Circle(int radius);
};

Circle::Circle(){
    radius = 10;
    cout << "반지름" << radius << "원 생성" << endl;
}

Circle::Circle(int radius){
    radius = radius;
    cout << "반지름" << radius << "원 생성" << endl;
}
```

대강 이런 느낌이다. 위의 코드는 생성자가 2개로 오버로딩을 통한 다형성을 구현한 사례로 볼 수 있다. 쉽게 말하면, 객체를 생성할 때 값을 지정하거나 안하거나에 따라 달리 동작하게 만들 수 있다는 것이다.



### 생성자의 목적

생성자는 왜 필요할까? 객체, 엄밀하게는 인스턴스를 생성할 때 해야하는 여러가지 내부적인 작업들이 있다. 멤버 변수 값을 초기화 하거나, 객체 크기에 맞는 메모리 할당, 파일을 열거나 네트워크를 연결하는 등 생성하는 시점에 해야하는 이러한 초기화 작업들을 담당하기  위해서 존재하는 것이 생성자이다.

### 생성자의 호출

앞서 말했듯, 생성자는 인스턴스가 만들어지는 시점에 생성된다. 그것도 자동으로! 그리고 임의로 호출하는 것은 불가능하다. 또한, 오버로딩을 통해 다형성을 구현했으므로 매개변수에 따라 중복된 생성자 중 해당하는 하나만 호출된다.



만약 생성자를 하나도 생성하지 않았다면, 컴파일러에 의해 매개변수가 없는 기본 생성자가 자동으로 생성된다. 하지만, 만약 매개변수가  있는 생성자 하나만 생성하고 매개변수가 없는 기본 생성자를 생성하지 않았다면 컴파일 에러가 난다. 어떤 생성자든 생성자가 있기만  하다면, 컴파일러가 자동으로 기본 생성자를 생성하지 않기 때문이다. 이 경우를 주의해야 한다.



아래의 예제 코드로 더 심화된 내용을 다뤄보겠다.

```cpp
class Rectangle{
    int width, length; 
public:
    Rectangle();
    Rectangle(int x);
    Rectangle(int w, int l);
    int getArea();
};
```

### 생성자로 멤버 변수 초기화

위 클래스의 생성자는 총 세가지다. 매개변수가 없는 경우, 한 가지인 경우, 두 가지인 경우. 이렇게 넘겨받은 매개변수의 값으로 인스턴스의 멤버 변수를 초기화하게 되는데, 먼저 각각의 경우별로 멤버 변수를 초기화하면 아래와 같다.

```cpp
Rectangle::Rectangle(){ width = 10; length = 10; }
Rectangle::Rectangle(int x) { width = x; length = x; }
Rectangle::Rectangle(int w, int l) { width = w; length = l }
```



또, 아래와 같이 초기화하는 것도 가능하다.

```cpp
Rectangle::Rectangle() : width(10), length(10) {}
Rectangle::Rectangle(int x) : width(x), length(x) {}
Rectangle::Rectangle(int w, int l) : width(w), length(l) {}
```

이렇게 할 수 있는 이유는 C++ 에서 멤버 변수 또한 객체이기 때문이다. int의 크기를 가지는 객체이므로, int 클래스의 인스턴스 width의 생성자 매개변수로 값을 넘겨주어 초기화시키는 것으로 이해할 수 있다. 조금만 깊게 생각해보면 이해가 될 것이다. 여튼, 이제 마지막으로 위임 생성자를 알아볼 차례이다.

### 위임 생성자

일단, 위임 생성자는 C++ 11부터 지원한다. m1 맥북 유저인 필자는.. 터미널 환경에서 학습하고 있는데, 이 문제를 해결하려고  CMake를 배웠다(...) 만약, 터미널 환경에서 vim으로 C++을 배우고 있다면, g++ 컴파일러가 컴파일할 때 보는  Makefile을 유연하게 다룰 수 있는 툴인 CMake를 알아보면 도움이 될 것이다. 

여기에서 몇일 고생을 해서 하소연하듯이 팁을 적자면, CMakeLists.txt 파일에



set(CMAKE_CXX_STANDARD 11)

set(CMAKE_CXX_STANDARD_REQUIRED ON) # 필수는 아니고 권장



이 두 줄만 추가하면 된다. 이는 C++ 11 버전으로 컴파일하도록 해줄 것이다!

위임 생성자는 타겟 생성자를 통해 생성자 작업을 대행하는 것을 의미한다. 말은 어려우니까 코드로 보면 간단하다.

```cpp
Rectangle::Rectangle() : Rectangle(10) {} //위임 생성자
Rectangle::Rectangle(int x) : Rectangle(x, x) {} //타겟 생성자이면서, 위임 생성자
Rectangle::Rectangle(int w, int l) : width(w), length(l) {} //타겟 생성자
```

더 설명은 필요 없어보인다.



## 소멸자

생성자와 반대로 소멸자는 객체가 소멸될 때 자동으로 호출되는 함수다. 이 또한 마찬가지로 임의로 호출할 수 없으며, 클래스와 동일한 이름을 갖고 리턴타입을 지정할 수 없다. 다만, 차이점은 소멸자는 반드시 하나만 존재해야하고, 소멸자 앞에 틸트(~)가 붙는다.

```cpp
class Rectangle{
    int width, length; 
public:
    Rectangle();
    Rectangle(int x);
    Rectangle(int w, int l);
    ~Rectangle();
    int getArea();
};

Rectangle::~Rectangle() {}
```

앞선 코드에 소멸자를 추가해보았다. 자세히 살펴보자.

### 소멸자의 목적

소멸자의 목적은 생성자와 반대라고 생각하면 쉽다. 객체가 사라지는 시점에 인스턴스가 할당 받은 메모리를 해제하고, 파일 저장 및 닫기,  네트워크 닫기 등 자원에 대한 마무리 작업을 한다. 만약 소멸자가 없다면, 객체가 만들어질 때마다 메모리 등의 자원이 추가적으로  사용되게 될 것이고, 이것이 반복되면 언젠가 메모리 오버플로로 인해 파란화면을 볼 수가 있기 때문에 중요한 역할을 담당한다.  자바에서 가비지 컬렉션에 해당하는 업무를 담당하는 것이 아닌가.. 생각해본다.

### 소멸자의 호출

소멸자는 해당 인스턴스가 생성된 범위가 끝나는 순간 즉, 함수 내부의 지역 객체는 return되는 순간 생성자의 역순으로 호출된다. 지역 객체 뿐만 아니라 전역 객체도 있을 수 있는데, 전역 객체의 경우 프로그램이 종료될 때 호출이 된다. 하지만,  new 키워드를 이용해 동적으로 생성된 객체는 프로그램이 종료되더라도 메모리에 남아있으므로 반드시 delete 키워드로  소멸시켜줘야한다!

### 기본 소멸자

마지막으로, 소멸자를 선언하지 않은 경우에는 생성자와 마찬가지로 컴파일러가 기본 소멸자를 만들어준다. 따라서, new 키워드로 만든 동적 생성 객체를 delete로 소멸시키는 것을 신경써주도록 하자!