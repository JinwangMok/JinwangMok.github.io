---
layout: single
title: "flowchart, 순서도, 자판기 순서도, 프로그램 설계, 프로그램 설계 도구, 프로그램 설계도, 플로우차트"
categories: etc
tag: [blog, python, jekyll]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

감사하게도 교수님께서 C++ 언어에 대해서만 공부하는게 아니라, 프로그래밍 설계 기법까지 알려주셔서 이렇게 블로그에 정리해보게 되었다.



### 프로그램 설계란?

프로그램 설계란, 프로그램의 설계도를 작성하는 과정을 말한다. 이는 타인과 프로그램 동작의 알고리즘을 공유하는 행위로 프로그래머의 핵심 기술이라고 볼 수 있다. 프로그램 설계는 직관적이고 이해가 쉬워야하며, 설계도를 보고 프로그래밍(코딩)이 가능해야 좋은 설계라고 할 수 있다. 프로그램을 구현하는 것은 C++과 같은 프로그래밍 언어로 할 수 있다. 그렇다면, 프로그램 설계는 어떤 도구로 할 수 있을까?



### 여러가지 프로그램 설계 도구

#### 1. 순서도Flowchart

프로그램 설계 도구에는 대표적으로 **순서도Flowchart**가 있다. 직사각형이나 마름모꼴의 도형으로 표현되는 기호 내에 자연어나 의사코드로 프로그래밍 내용을 적고, 이것들을 순서에 맞게 화살표로 진행방향을 표시하여 프로그램의 설계 내용을 확인할 수 있는 좋은 표현도구다.

아래는 자판기 알고리즘을 표현한 순서도의 예시다.(과제였던거는 안비밀😂)



![img](https://blog.kakaocdn.net/dn/ygcfa/btrfWK2WHwW/frDPxdi6rUV81G6TJ1Mx31/img.png)

아직 배우는 단계라 좋은 설계라고 볼 수 없겠지만, 순서도라는 개념을 이해하기에는 좋을 듯 하여 첨부해본다.



#### 2. 상태도State Diagram

다음으로는 **상태도State Diagram**이다. 상태도는 논리회로를 공부했다면 한번쯤 접해봤을 표현법인데, 간단히 설명하자면 대상의 상태와 상태 변화(상태 천이)에 주목하여  이를 도식화한 것을 의미한다. 오토마타 이론의 대표적인 표현방법으로 유한 상태 기계도라고도 한다. 도식을 표현할 때 목적으로 하는 상태를 추상화한 다음 상태 변화에 대한 설정을 한다. 이때 상태 변화는 입력에 의해 이뤄지고, 어떤 상태로 존재하는가는 출력으로 나타낼 수 있다. 상태 유지는 외부 이벤트 없이도 계속 유지 되고, 변화는 주로 외부 입력에 의해 행해진다. 내부 상태가 출력이 될 수도 있지만, 상태 변화시에도 출력을 만들 수 있다.



정리하자면, 그 프로그램(시스템)에서 나타낼 수 있는 모든 상태(state)를 나타내고, 상태 변화가 가능하다면 어떤 이벤트를 동해 그것이 가능한지를 화살표를 통해 나타낸 것이다. 이에 대한 예시로 헤어드라이기가 있겠다.

![img](https://blog.kakaocdn.net/dn/z4kpL/btrfZpyyPcd/h2dB6wlviEt1ThGLhWh5X0/img.png)

#### 3. 유즈 케이스 다이어그램Use Case Diagram

시스템과 사용자(액터, 변화를 발생시킬 수 있는 대상) 사이의 상호 작용을 보여주는 다이어그램이다. 즉, 유저와 시스템의 상호작용을 표현한 다이어그램으로, 발생가능한 세부사항들에 대해 기술하기 때문에 시스템의 전체적인 구조를 담기 수월하다고 한다.



#### 4. 클래스 다이어그램Class Diagram

클래스 다이어그램은 객체의 관계를 보여주는 다이어그램이다. 객체단위로 노드를 구성하고 화살표로 상속관계를 표현하는 다이어그램이다.



#### 5. 시퀀스 다이어그램Sequence Diagram

시퀀스 다이어그램은 객체 간의 시간의 순서대로 동작 관계를 보여주는 다이어그램이다.

------

이렇게 프로그램 설계 도구에 대해 알아봤다.



이는 결국 도구일뿐 프로그램 설계에는 더 많은 단계와 기법들이 존재한다. 추후에 학습을 정리하며 기법에 대해서도 올릴 기회가 되길🙏🏻



여기서 이 글을 이만 줄인다.📝