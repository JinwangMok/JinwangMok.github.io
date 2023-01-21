---
layout: single
title: "캐글(Kaggle) Notebook 실습(Exercise)하는 방법"
categories: mldl
tag: [Kaggle, Kaggle 입문, 캐글, 캐글 code, 캐글 Courses, 캐글 Exercise, 캐글 Notebook, 캐글 노트북, 캐글 실습, 캐글 입문]
toc: true
toc_sticky: true
author_profile: true
search: true
typora-root-url: ../

---

캐글에서 Courses를 선택해 수강하고, 강의 글을 읽은 뒤에 코드 실습을 하라고 합니다.



이 때, 목록에 있는 Exercise의 < > 버튼을 클릭하거나 팝업으로 뜨는 Your turn 아래의 "Start  Exercise"를 클릭하거나 아니면 글 아래에 있는 링크를 따라가면 kaggle notebook으로 연결이 됩니다.

(아예 내비게이션 바에서 Code탭을 선택 후에 접근할 수 도 있지만, Courses 실습 환경이라고 가정했으니..)

![img](https://blog.kakaocdn.net/dn/1eLqd/btrfDf9WUEt/gXGC5j9FNK3oy5qzkE5u91/img.png)

기본적으로 jupyter notebook을 사용해보셨다면, 어렵지 않게 적응하실 수 있으나 혹시 처음 보시는 분들을 위해 적어보자면...



메인이 되는 화면 중앙의 패널이 노트북이고 여기에는 마크다운과 코드 2가지 블록을 삽입할 수 있습니다. 현재 보이는 화면 또한 캐글에서 제공하는 마크다운 블록과 코드 블록을 보여주고 있지요.



노트북 바로 위의 툴바에서 +를 누르면 블록을 삽입할 수 있고, 쓰레기통 아이콘을 누르면 삭제, 가위 아이콘으로 잘라내기, 그 옆 아이콘은 복사, 그 옆은 붙여넣기를 실행합니다.



그리고 그 옆에 삼각형 버튼을 누르면 해당 셀을 실행하는데, 마크다운이라면 마크다운 언어를 기반으로 화면처럼 글을 보여주고, 코드라면 해당 코드를 실행합니다. 또한, 삼각형이 2개인 버튼을 누르면 전체 블록을 실행합니다.



그 옆에 code라는 글자가 써있는 부분은 커서가 위치한 셀의 블록 타입을 의미하며, 이를 바꾸면 해당 블록의 타입을 변경할 수 있습니다.



우측에 보이는 정보들을 자세히 보면 HDD, CPU, RAM을 볼 수 있는데, 이것은 우리가 배당받은 컴퓨터의 리소스를 나타내주는 것입니다. 이 노트북은 사실 캐글이 빌려준 컴퓨터로 돌아가는 것이거든요.



그 옆 전원버튼으로 이 컴퓨터를 종료(정확히는 연결해제)할 수 있고, 그 옆 버튼으로 전체 셀을 초기화 시킬 수 있습니다.



이 외의 내용들은 눌러보면 뭐 다 써있으니 굳이 언급하지 않겠습니다.



캐글 노트북만의 특징은 우측에 보이는 **사이드바**인 것 같습니다.



여기에서 데이터를 추가하거나, 세팅(언어 등)을 바꾸거나, 이 노트북을 예약해서 실행할 수 도 있고, 모르는 것들을 검색할 수 도 있게 해놨습니다. 가장 우측 >| 이렇게 생긴 버튼을 누르면 숨겨집니다.



### 캐글 Exercise의 특징

지금까지는 기능 소개였고... 이 Exercise라는게 진짜 마인드블로잉한데,,, 캐글에서 배우기 쉬우라고 스텝별로 맞았는지 틀렸는지, 또 모르면 힌트도 쓸 수 있게 메소드를 준비해놨습니다. 미쳤죠..;



이걸 사용하려면 먼저 노트북에서 아래의 코드를 실행해야 됩니다.



![img](https://blog.kakaocdn.net/dn/Ij5VP/btrfBq41n4O/JvpFeBk3h1Xbn01i2wtsuK/img.png)

저 코드 블록을 선택하고 실행하면 저렇게 Setup Complete이 뜨고, 이제 step 1부터 진행하면 됩니다.



![img](https://blog.kakaocdn.net/dn/cIlstE/btrfBqYevOf/DkvDdhpGkfNjrt0T2KjvMk/img.png)

저기 드래그한 부분처럼 문제가 나갑니다. 그럼 저렇게 ___되어있는 곳에 정답이 되는 코드를 적으면서 말그대로 활용 연습을 할 수  있는데, 제가 충격받은 것은 틀리면 당연히 에러가 뜨겠지만, 맞추면 correct가 뜨는 저 마지막 줄의 코드였습니다.

![img](https://blog.kakaocdn.net/dn/lvDsK/btrfw4VGUnq/IxE9N1t4AKrXaAHK89Z5kK/img.png)

공부하라고 이렇게 만들어준게 참.. 멋지네요!



또 아래에는 이렇게 힌트를 볼 수 있는 코드가 있습니다. 2번 라인을 주석 해제하고 실행하면 아래와 같이 나타납니다.

![img](https://blog.kakaocdn.net/dn/oLkhw/btrfuOFBgkg/6P1nPktSPdM7Hu5lvAFYKK/img.png)



이렇게 캐글에서 Exercise를 하는 방법에 대해 적어보았습니다.



이렇게 고급진 강의를 무료로 받을 수 있다니하면서 감동받고 있습니다.😂



여기서 이만 줄이고 저는 Exercise하러 가봅니다😆
