---
layout: single
title: "C++ 시작하기"
categories: cpp
tag: [C++, C++ >>, C++ namespace, C++ std 네임스페이스, C++ using, C++ 기초, C++ 네임스페이스, C++ 문법, C++ 스트림, 프로그래밍 언어]
toc: true
toc_sticky: true
author_profile: true
search: true
typora-root-url: ../

---

## C++ 코드 읽어보기

개인적으로는 어떤 새로운 언어를 배울 때 예제코드부터 읽어보고 그에 맞게 잘 모르겠고, 궁금한 내용을 찾아보면서 배우는게 이해하는 속도가 빠른 것 같다. 해서, 이번에는 간단한 예제 코드를 보면서 C++을 시작해보고자 한다.



### Hello world🌏

```cpp
#include <iostream>

using namespace std;

int main(){
        cout << "Hello world";
}
```



c 언어를 배운 사람이라면, #include는 익숙할테지만 <>안의 내용이 다른 것을 확인할 수 있을 것이다.

또한, 처음보는 using과 namespace 키워드 그리고 namespace인 것 같아보이는 std까지 확인할 수 있다.

익숙한 int main() 함수 내에는 printf() 대신 cout << 이 있는 것으로 보아 이것이 출력을 담당하는 코드라는 것을 눈치로 알 수 있겠다.

그리고, 우리의 소중한 return 0; 도 보이지 않는다.



먼저, 전처리기부터 살펴보자.

#### 전처리기

컴파일 언어인 C++ 이 실제 바이너리 실행파일이 되려면 다음과 같은 단계를 거쳐야 한다.

- 전처리 : 작성된 소스코드가 컴파일되기 전에 전처리기가 소스코드에서 필요한 내용을 치환하는 단계.
- 컴파일 : 이렇게 전처리까지 마친 원시 코드를 목적 코드(C++의 경우 기계어)로 해석하는 단계.
- 링킹 : 이렇게 해석된 기계어에, 실행에 필요한 라이브러리를 연결하는 단계.

사실 전처리 규칙은 C나 C++이 다르지 않은데, 여기서 주목할 것은 C에서는 stdio.h인 표준 입출력 라이브러리가 C++ 에서는 iostream이라는 것이다. 이 둘의 차이는 간단히 객체지향에서 필요한 것들이 있냐 없냐 이정도라고 보면 될 것 같다. 물론, 많은 것들이 다르지만! 이에 대해서는 차후에 더 깊게 다룰 기회가 있다면 다뤄보고, 일단은 여기까지만 다룬다.



#### 스트림stream

여기서 추가적으로 스트림이란 무엇일까에 대해 알아보았다. C++ 은 스트림이라는 흐름을 통해 파일이나 콘솔의 입출력을 다룬다고 한다. 이 스트림 내부에 버퍼가 있고, 버퍼는 임시 메모리 공간으로 입력과 출력을 한번에 처리하거나 수정하기 용이하게 해주는 역할을 한다. 물론, 게임처럼 즉각 반응하는 경우 버퍼를 사용하지 않는 것이 더 좋을 것이다.



위에서 다룬 iostream은 입출력 스트림 관련 클래스 라이브러리 파일인 것이다. 이에 대해서는 아래에서 더 다뤄보겠다.



#### 네임스페이스namespace

C++에는 다양한 식별자가 있고, 이들을 만들 수 있고, 부딪힐 수 있다. 특히, 대규모 프로젝트를 진행할 경우 이름이 동일한 클래스나  함수가 있을 수 있고, 이들이 중복된다면 큰 혼란이 발생하게 될 것이다. C++ 에서는 이 문제를 네임스페이스로 해결한다.

```cpp
namespace mySpace{
    class Circle{
    	public:
        	void getRadius() {}
    };
    void myFunc(Circle) {}
}
```

위와 같이 네임스페이스는 클래스나 함수가 영향을 미칠 선언적 공간을  지정하는 역할을 한다. 네임스페이스 내부에 있는 클래스나 함수에 접근하려면 범위 지정 연산자(scope resolution  operator, ::)를 사용하거나 using을 사용한다. 아래는 사용예시다.

```cpp
#include <iostream>

int main(){
    mySpace::Circle ball;
    ball.getRadius();
    mySpace::myFunc(ball);
}
```

이 경우는 범위 지정 연산자(::)를 사용한 경우이다. 다음은 using을 사용하는 경우 중 첫번째 사용예시이다.

```cpp
#include <iostream>

int main(){
    using mySpace::Circle;
    Circle ball;
    ball.getRadius();
}
```

이렇게 using은 네임스페이스의 식별자를 범위 내에서 사용할 수 있게 해준다. 이 사용법을 using directive라고 한다.(using 지시) 다음은 using의 마지막 사용예시이다.

```cpp
#include <iostream>

int main(){
    using namespace mySpace;
    
    Circle ball;
    ball.getRadius();
    myFunc(ball);
}
```

어찌보면 가장 간편한 사용방법이다. 이 사용방법은 범위 내에서 네임스페이스에 있는 모든 식별자를 사용할 수 있게 해준다. 이 사용법을 using declaration이라고 한다.(using 선언) 

```cpp
using namespace std;
```

따라서, 맨 위에 적혀있는 위와 같은 코드는 std라는 네임스페이스의 식별자들을 전역 범위에 걸쳐서 사용할 수 있도록 using declaration한 것으로 해석할 수 있다. 그렇다면 std는 무엇인가?

#### std::cout

std는 C++ 의 표준 네임스페이스로 iostream 파일 안에 선언되어있다. 여기에 cout, cin, endl 등 수많은 식별자들이 있고, 이들은 모두 객체다. 즉, cout 객체, cin 객체인 것이다. 예를 들어, cout 객체는 출력을 담당하는 객체로  삽입 연산자(<<)를 통해 스트림으로 값을 삽입한다. 이 때, C 언어와 달리 %로 타입을 명시하지 않는데 이는  입출력을 cin, cout 객체가 알아서 처리하도록 설계되었기 때문이다. 따라서, 더 안전하고, 편리하다고 볼 수 있다.



이렇게 Hello world 코드를 분석하면서 C++ 문법에 대해 알아보았다.



잘못된 지식을 바로잡는 지적은 사랑한다.





[출처]

https://tcpschool.com/cpp/cpp_io_streamBuffer

[https://www.cplusplus.com/reference/ostream/ostream/operator%3C%3C/](https://www.cplusplus.com/reference/ostream/ostream/operator<
