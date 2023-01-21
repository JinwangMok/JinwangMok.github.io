---
layout: single
title: "CMake 배우기"
categories: etc
tag: [C++ 개발환경, cmake, CMakeList.txt, MacOS C++, makefile, vimrc]
toc: true
toc_sticky: true
author_profile: true
search: true
typora-root-url: ../

---

처음에는 Makefile이랑 CMake 가 무슨 차이인지 심지어 둘이 다른건지도 잘 몰랐는데, 잘 정리된 글 하나로 개념을 머릿속에 정리할 수 있었다. 간단히 이해한 내용을 적어본다.





### 1. CMake는 Makefile로 make하는 것과 달리 헤더파일의 정보를 알아서 찾아보고 그때 그때 변경사항을 Makefile에 업데이트한다.



Makefile은 컴파일과 빌드하는 과정을 편리하게 도와주지만, 헤더파일을 스스로 확인하면서 의존성을 체크하는게 아니라 Makefile에 명시한  것을 기반으로 컴파일을 하기 때문에 프로젝트의 규모가 커지면서 여러 파일들이 서로의 의존성 관계가 복잡해지면 관리하기 어렵다는  단점이 있다.



(이 때 Makefile 내에 아래와 같은 매크로를 만들어서 make clean 명령어로 해결한다고 하고 이를 클린 빌드라고 한다고 한다.)

```
clean:
	rm -f *.o
    rm -f $ (TARGET)
```



하지만, CMake를 사용하면 알아서 각각의 파일의 의존성 관계를 파악하기 때문에 Makefile의 Incremental build  기능을 효과적으로 이용할 수 있게 된다. Incremental build란 쉽게 말해 필요한 파일만 빌드해서 자원을 아끼는 것을  의미한다. 대규모 프로젝트에서 효과적이라고 함.





### 2. CMake는 Makefile을 만든다.



CMake는 전혀 다른 기술로 make를 처리하는 것이 아니었다. CMake는 Makefile을 **CMakeLists.txt** 파일로 빌드한다.



만약 한 디렉토리에 (main.c, apple.c, apple.h, banana.c, banana.h) 이렇게 파일이 있고 빌드하려 한다면,



Makefile은 아래와 같다.

```
OBJS=main.o apple.o banana.o
TARGET=app.out
 
all: $(TARGET)
  
clean:
    rm -f *.o
    rm -f $(TARGET)
 
$(TARGET): $(OBJS)
    $(CC) -o $@ $(OBJS)
  
main.o: apple.h banana.h main.c
apple.o: apple.h apple.c
banana.o: banana.h banana.c
```



CMakeLists.txt는 아래와 같다.

```
ADD_EXECUTABLE( app.out main.c apple.c banana.c )
```



하지만, 기본적으로 CMakeLists.txt를 빌드하면 위와 비슷한 Makefile을 만들어주는 것이다.





### 3. CMake 사용법



CMake의 사용법은 간단해보인다.



\1. 새로운 프로젝트를 생성하면 내부에 CMakeLists.txt 를 작성한다.



\2. 아래의 커맨드를 입력한다. 

```
cmake CMakeLists.txt
```

이거에 대해 추가적으로 설명하자면, 디렉토리에 CMake를 사용해 Makefile을 최초로 만들 때 한 번만 사용하면 된다고 한다. 그 이후로는 의존성에 변화가 생기면(헤더파일이 막 바뀌고하면) 아래에 나올 make만 치면 알아서 Makefile을 수정한다고 함.



\3. make로 빌드한다.

```
make
```



끝.



내 환경에서는 ..vimrc에 Fn을 리매핑 해놓으면 편리하게 사용할 수 있을 것 같다.

아마.. 이렇게 되려나?

```
nnoremap <F5> :w<CR>:!make && clear && ./app.out<CR>
```



지금은 아래처럼 사용하고 있다. 이것도 F5 버튼 하나로 파일 하나는 실행 시킬 수 있으니 큰 프로젝트가 아니고서야 이걸로도 만족하지만, 나중을 위해 CMake를 배워보는 중이다ㅎㅎㅎ

```
nnoremap <F5> :w<CR>:!g++ % && clear && ./a.out<CR>
```





### 4. 자동 생성되는 4가지와 .gitignore 설정



cmake를 실행하면 아래의 3가지 파일과 하나의 디렉토리가 자동으로 생성된다고 한다.

1. Makefile
2. CMakeCache.txt
3. cmake_install.cmake
4. [CMakeFiles]

위의 4가지는 삭제해도 무방하다고 하고, git을 쓴다면, .gitignore 파일에 다음을 추가해서 위 4가지를 무시하도록 설정해주자.

```
/CMakeCache.txt
/cmake_install.cmake
/CMakeFiles/
/Makefile
```



이거는 CMakeLists.txt 파일만 있다면야 언제든지 cmake CMakeLists.txt 명령으로 만들면되니까 상관없다고 이해했다.



또한, 이걸 다르게 생각해보면.. CMake로 빌드된 프로젝트를 github에서 받았다면 위 명령으로 수동으로 빌드할 수 도 있다는..소리인가??? 거의 마법이다...; 



오늘 여튼 좋은 지식을 배운 것 같다.

드디어.. 점점 C++ 환경을 구축해나가고 있다..!@@
