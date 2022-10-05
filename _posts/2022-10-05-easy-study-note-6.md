---
layout: single
title: "Makefile 첫 사용"
categories: etc
tag: [makefile]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

일단 출처에 남긴 글을 쭉보면서 따라해봤다.



지금 적을 것은 강의 시간에 임의로 만들어두었던 classEx1.cpp 파일을 Makefile을 통해 컴파일 + 빌드하고 실행까지 해보는 것이었다.



먼저, 내 예제 코드는 다음과 같다.

```
#include <iostream>

using namespace std;

class Circle{
public:
	int radius;
	double getArea();
};//중요 ;

double Circle::getArea(){
	return 3.14 * radius * radius;
}

int main(){
	Circle donut; //도넛 인스턴스 생성
	donut.radius = 10; //도넛의 멤버 변수 radius를 10으로 초기화
	double area = donut.getArea();
	cout << "donut 크기는 " << area << endl;
	
	Circle pizza;
	pizza.radius = 30;
	area = pizza.getArea();
	cout << "pizza 크기는 " << area << endl;
}
```



원래 이걸 컴파일 후 실행 하려면 vi의 기본모드에서

```
:!g++ <filename> && ./a.out
```

이런식으로 했어야했는데, 이번에는 같은 디렉토리 내에 Makefile을 만들었다.

만든 Makefile은 아래와 같다.



```
app.out : classEx1.o
	g++ -o app.out classEx1.o
```

이걸 저장하고, 명령줄에

```
make
```

이렇게 쳤더니, app.out과 classEx1.o 파일이 생성됐다.



아직 다 읽은 건 아니지만, Makefile의 첫 사용을 기념에서 글을 써본다ㅎㅎㅎ



출처 : https://www.tuwlab.com/27193
