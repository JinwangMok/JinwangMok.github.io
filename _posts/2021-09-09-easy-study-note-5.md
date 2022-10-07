---
layout: single
title: "MacOS에서 Vim 개발 환경 구축하기"
categories: [vim, vimrc, vim설정, 맥OS vim, 컴공갬성]
tag: [blog, python, jekyll]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false

---

오늘은 MacOS환경에서 Visual Studio를 대체할 개발환경을 구축해보고자 한다.



일단 사건의 발단은 이번 학기 수강에 C++ 프로그래밍을 수강하는데, 맥북으로 실습을 따라하려니까 일반 Visual Studio는  윈도우용이라 설치가 안되고 Visual Studio for MacOS는 .NET이랑 C# 등..만 지원하고 C++ 이  안되더라..ㅠㅠ



총  3편에 나눠서 글을 작성할 계획이며, 이번 글에서는 vim에 대해 간략히 정리해보고,  내가 사용하고 있는 .vimrc 설정파일을 업로드해보려고 한다.(나머지 2편에서는 vim의 자동완성플러그인 설치, 컴파일 및 빌드를 도와줄 Makefile 만들기를  해보려고 한다.)



필자도 공부중이구 쓴지 얼마 안돼서 틀린 내용이 있을 수 있다. 있다면 댓글로 알려주세요ㅠㅠㅠㅠ(돌변)





### 1. vim이란?



처음 vim을 사용하게 된 건, 맥북을 사기전 아이패드만으로 코딩을 해보려고 이것저것 시도해보다가, 유튜브에서 개발자맛님의 아이패드로 리눅스 서버에 원격접속해서 코딩하는 영상 + vim 영상들을 보고 시작하게 됐다.



쉽게 말해 vim은 vscode나 메모장 같은 <텍스트 에디터>다. 근데 이제 여기에 여러가지 플러그인을 설치하거나 몇가지 설정을 변경해서 나만의 편집툴로 바꿀 수 있고, 자유도가 꽤나 높기 때문에 매력적으로 느끼는 것 같다.(그리고 cli에서는  어차피 별로 선택지가 없다.)



이 vim의 사용법은 매우 간단하다.

먼저, 운영체제에 맞게 패키지 매니저로 vim을 설치한 뒤에 명령줄에 $ vi 라고 치면 바로 나온다.(엄밀히 말하면 vim이라고 쳐야하는데, vi라고 쳐도 vim으로 호환되서 나오고 하니 이 부분은 패스..)



이제 키를 눌러보면,,, 뭐 별 반응이 없을 것이다. 운 좋게 i나 a를 눌러서 '어 뭐야 왜 되는거야;;'라는 상황이 생겼다면 모를까..

vim에는 크게 3가지 모드가 있다.



Normal, Insert, Visual.



노말모드는 시작 시 상태인데, 이때는 입력은 안되고 이동이나 삭제 등의 명령만 가능하다

삽입모드는 i나 a 등을 누르면 시작되는 모드로, 이제서야 텍스트를 입력할 수가 있다. 물론,, 커서 이동이 힘들다.

비주얼모드는 v를 누르면 시작하고 쉽게 생각하면 드래그모드 정도로 볼 수 있겠다. 텍스트를 선택할 수 있고, 복사할 때 자주 쓴다.



이미 복잡하다. 근데 아직 복잡한게 많이 남았다. 그래도 감성이 있으니까...







### 2. .vimrc 세팅



먼저 .vimrc란 vim의 설정파일이다. 앞에 .가 붙은 이유는 숨김파일이라 그렇다. 따라서 .vimrc를 확인해보고 싶으면,

홈디렉토리에서 $ ls -al 등의 명령어로 보면 되겠다.



.vimrc는 얘만의 문법이 존재한다. 문법이라기엔 좀 그럴 수 있지만 여튼..

일반적인 GUI에서는 설정에 가서 설정하면 되는 것들을 얘는 이 파일을 통해서 설정해줘야한다.



하지만!! 이후 설명할 플러그인들을 하나 둘씩 설치하면 점점 괴물이 되어가므로 만드는 재미는 보장한다.. ㄹㅇ꿀잼



앞서 말했듯이 vim자체도 배우는 중이기 떄문에 이해하기 쉽게 한글로 주석을 가득가득 넣어놨다..ㅎㅎㅎ

부끄럽지만 내 .vimrc파일은 아래와 같다.

```bash
" [vimrc 설정파일]
"
" <단축키>
" <F2> : 
" <F3> : Tagbar
" <F4> : NERDTree
" <F5> : C) gcc로 해당 파일만 단순 컴파일 및 빌드 
" <F6> : C++) g++로 해당 파일만 단순 컴파일 및 빌드
" <F7> : python) 화면 클리어 후 해당 파일 인터프리터로 실행
" <F8> :
" <F9> : 
" <F10> :  IndentGuide 토글
" <F11> : 
" <F12> : 저장 후 make 명령 실행으로 빌드 및 실행
" 
" <space> : 코드 폴딩
"
" , + q/e : 이전/다음 파일 버퍼로 이동
"
" [cpp파일]
" 입력모드에서 :main<CR> 을 입력하면 기본main함수 구조입력
"
"<사용법>
" - 입력모드(insert)
" i: 입력모드 변경
" a: 다음칸 이동 후 입력모드
" o: 다음 줄로 이동 후 입력모드
" shift+i : 현재 줄 맨 앞에서 입력모드
" shift+a : 현재 줄 맨 마지막에서 입력모드
" c-w: 커서위치의 단어를 삭제한 후 입력모드
" 
" - 비주얼모드(visual, v)
" shift+v : 여러줄 선택
" (선택후)= : 정렬
"
"
" - 명령모드(command, esc) 
" h,j,k,l : 좌,하,상,우
" shift+h: 화면 가장 윗줄
" shift+l: 화면 가장 아랫줄
" shift+g: 파일 가장 아랫줄
" shift+4 : 현재 줄 맨끝
" shift+6 : 현재 줄 맨앞
" y : 복사
" d : 삭제
" p : 붙여넣기
" yy: 한 줄 복사
" dd: 한 줄 삭제
" 숫자+yy: 숫자만큼 줄 복사
" 숫자+dd: 숫자만큼 줄 삭제
" ctrl+f: 다음 화면 라인 출력
" ctrl+b: 이전 화면 라인 출력
" shift+j : 아래 라인을 현재 라인 끝으로 올리기
" shift+d : 현재 커서부터 라인 끝까지 삭제
" =-shift+5: 블록 내 코드의 들여쓰기 정렬
" shift+8: 커서와 동일한 문자열 찾기
" (이어서)n: 다음 문자열(찾기)
" (이어서)shift+n: 이전 문자열(찾기)
" u: 되돌리기
" ctrl-r: 되돌리기 취소
" x: 한 문자 삭제
" 숫자+x : 숫자만큼 문자 삭제
"
" -라인모드(/)
" /+문자열: 문자열 찾기(shift+8과 동일)
" :+라인수: 해당 라인위치로 이동 
" :+$: 마지막 라인으로 이동
" :set nonumber: 라인수 표시제거
" :set number: 라인수 표시
" :set paste: 복사한 내용 그대로 붙여넣기(paste모드)
" :set nopaste: paste모드 해제
" :%s/찾을문자열/바꿀문자열: 해당 되는 모든 문자열을 찾아 바꾼다.
" :r 파일이름: 커서위치에 해당파일 붙여넣기
" :w: 저장
" :q: 종료
" :q!: 강제종료
" :!실행명령 : 명령 실행
"------------------------------------------------------------------------------------
"문법 강조
syntax enable
if has("syntax")
    syntax on
endif

"들여쓰기
"set smartindent
set cindent
set autoindent

"UTF-8 Support
set encoding=utf-8

"줄 번호 표시
set nu

"마지막으로 수정된 곳에 커서를 위치하는 기능
au BufReadPost *
\ if line("'\"") > 0 && line("'\"") <= line("$") |
\ exe "norm g`\"" |
\ endif

"상태바 표시와 커서위치좌표
set laststatus=2
set statusline=\ %<%l:%v\ [%P]%=%a\ %h%m%r\ %F\

"검색어 강조와 검색 시 글자마다 검색 수행
set hlsearch
set incsearch

"커서 위치 하이라이트와 커서 위치 정보 상태줄에 표시
set cursorline
set ruler

"탭을 space로 변경
set expandtab
set tabstop=4

":명령어의 히스토리를 기억할 개수
set history=255

"검색어에 대문자가 포함된다면 대소문자를 무시하지 않는 기능
set smartcase

"한글-영문 매핑
set langmap=ㅁa,ㅠb,ㅊc,ㅇd,ㄷe,ㄹf,ㅎg,ㅗh,ㅑi,ㅓj,ㅏk,ㅣl,ㅡm,ㅜn,ㅐo,ㅔp,ㅂq,ㄱr,ㄴs,ㅅt,ㅕu,ㅍv,ㅈw,ㅌx,ㅛy,ㅋz
set nolangremap

"단어 단위로 문장 잘림
set linebreak

"공백문자 표시
set list listchars=tab:»\ ,trail:·

"플러그인을 로드하지 않음. 플러그인 문제시 주석해제 추천.
"set noloadplugins

"%를 괄호위에서 사용하면 그에 해당하는 짝인 괄호로 이동할 수 있다.
set mps =(:),{:},[:],<:>

"짝 맞는 괄호 강조
set showmatch

"파일 확장명이 c,cpp,java인 경우 = 와 ;를 짝으로 지정.
:au FileType c,cpp,java set mps+==:;

"마우스 사용
set mouse=a

"<Tip>
"NORMAL 모드에서 숫자 위에 커서를 올린 다음 <C-a>를 입력하면 숫자가 1씩 늘어나고, <C-x>를 입력하면 숫자가 1씩 줄어듭니다.
"종종 복잡한 숫자를 변경할 일이 있을 때 편리합니다. 가령 248같은 숫자를 뺄셈하려면 암산을 하거나 계산기를 써야 할 텐데,
"그냥 NORMAL 모드에서 248<C-x>만 입력하면 되거든요. 특히 암산이 어려운 2진수, 8진수, 16진수를 다룰 때 도움이 됩니다.
"Ctrl-a, Ctrl-x의 증감 대상이 되는 숫자의 패턴을 지정
set nf=alpha,octal,hex,bin

"커서 위에 항상 3줄의 여백
set scrolloff=3

"여러 줄로 표현될 때 아래 줄들의 이어짐 기호
set showbreak=+++\

"화면 상단의 탭 라인 항상 노출
set showtabline=2

"수평 스크롤 및 한 번에 스크롤할 열 개수
set nowrap
set sidescroll=2
set sidescrolloff=10

"탭단위로 지우기
set smarttab

" h,l 로도 라인 이동이 가능합니다.
set ww+=h,l

"화면 분활 시 생성되는 방향
set splitbelow
set splitright

"화면 분활 시 이동 단축키 리매핑
nnoremap <C-J> <C-W><C-J>
nnoremap <C-K> <C-W><C-K>
nnoremap <C-L> <C-W><C-L>
nnoremap <C-H> <C-W><C-H>

"코드 폴딩(함수, 클래스 숨기기)
set foldmethod=indent
set foldlevel=99
nnoremap <space> za

"시스템의 클립보드 사용
set clipboard=unnamed

"줄 맨뒤로 이동
inoremap ;; <esc>la

"esc 딜레이 줄이기
set ttimeoutlen=5 
"괄호 자동 완성
inoremap ( ()<Esc>i
inoremap { {}<Esc>i
inoremap {<CR> {<CR>}<Esc>O
inoremap [ []<Esc>i
""inoremap < <><Esc>i
inoremap ' ''<Esc>i
inoremap " ""<Esc>i
"cpp파일이면 <> 대신 <<가 나오도록 설정
"autocmd FileType cpp :inoremap <<space> <<<space>
"autocmd FileType cpp :inoremap ><space> >><space>
autocmd FileType cpp :inoremap :main<CR> #include<space><iostream><CR><CR>using<space>namespace<space>std;<CR><CR>int<space>main(){<CR>}<esc>ko
autocmd FileType c :inoremap :main<CR> #include<space><stdio.h><CR><CR>int<space>main(){<CR>}<esc>ko
autocmd FileType html :inoremap :html5<CR> <!DOCTYPE html><CR><html<space>lang="en"><CR><head><CR><meta<space>charset="UTF-8"><CR><meta<space>name="viewport"<space>content="width=device-width, initial-scale=1.0"><CR><esc>i<title>Document</title><CR></head><CR><body><CR><CR></body><CR></html><esc>kki
"---------------------------------------------------------------------------
"<플러그인>
set nocompatible
filetype off

set rtp+=~/.vim/bundle/Vundle.vim

call vundle#begin()

"기본 플러그인 매니저 Vundle :PluginInstall
Plugin 'gmarik/Vundle.vim'
"컬러 스킴
Plugin 'morhetz/gruvbox'
"tagbar 단축키 <F3>
Plugin 'majutsushi/tagbar'
"Nerdtree 단축키 <F4>
Plugin 'scrooloose/nerdtree'
"Nerdtree 확장플러그인. NERDTreeMirrorToggle사용가능. 잘안씀
Plugin 'jistr/vim-nerdtree-tabs'
"vim-indent-guides 단축키 <F11>
Plugin 'nathanaelkane/vim-indent-guides'
"vim with git status(added, modified, and removed lines)
Plugin 'airblade/vim-gitgutter'
"vim with git command(e.g., Gdiff)
Plugin 'tpope/vim-fugitive'
"vim status bar
Plugin 'vim-airline/vim-airline' 
"상단 파일바
Plugin 'vim-airline/vim-airline-themes'
"활성창 강조
Plugin 'blueyed/vim-diminactive'
"자동완성 플러그인 ycm
Bundle 'valloric/youcompleteme'
"코드 폴딩
Plugin 'tmhedberg/SimpylFold'
"파이썬 들여쓰기
Plugin 'vim-scripts/indentpython.vim'
"syntax 교정 플러그인
Plugin 'vim-syntastic/syntastic'
"python syntax 플러그인
Plugin 'nvie/vim-flake8'
"파일 찾기 플러그인
Plugin 'kien/ctrlp.vim'

call vundle#end()

"colorscheme
set background=dark
colorscheme gruvbox



"NerdTree
nmap <F4> :NERDTreeToggle<CR>
let NERDTreeIgnore=['\.pyc$', '\~$'] "ignore files in NERDTree

"taglist 매핑
nmap <F3> :Tagbar<CR>

"indent guide 설정
nmap <F10> :IndentGuidesToggle
"let g:indentguides_spacechar = '┆'
"let g:indentguides_tabchar = '|'
let g:indent_guides_enable_on_vim_startup = 1
let g:indent_guides_start_level=1
let g:indent_guides_guide_size=1

"vim-airline 설정
let g:airline#extensions#tabline#enabled = 1 " turn on buffer list
let g:airline_theme='hybrid'
set laststatus=2 " turn on bottom bar
let mapleader = ","
nnoremap <leader>q :bp<CR>
nnoremap <leader>w :bn<CR>

"blueyed/vim-diminactive 설정(화면분할)
let g:diminactive_enable_focus = 1

"simplyFold docstring 보기
let g:SimplyFold_docstring_preview=1

"youcompleteme
let g:ycm_autoclose_preview_window_after_completion=1
map <leader>g  :YcmCompleter GoToDefinitionElseDeclaration<CR

"python with virtualenv support(에러나서 주석처리)
"py << EOF
"import os
"import sys
"if 'VIRTUAL_ENV' in os.environ:
"  project_base_dir = os.environ['VIRTUAL_ENV']
"  activate_this = os.path.join(project_base_dir, 'bin/activate_this.py')
"  execfile(activate_this, dict(__file__=activate_this))
"EOF

"python syntax
let pyton_highlight_all=1

"C  실행 단축키<F5>
nnoremap <F5> :w<CR>:!gcc % && clear && ./a.out<CR>

"C++ 실행 단축키<F6>
nnoremap <F6> :w<CR>:!g++ -std=c++11 % && clear && ./a.out<CR>

"파이썬 실행 단축키<F7>
"nnoremap <buffer> <C-9> :exec '!python' shellescape(@%, 1)<cr>
nnoremap <F7> <esc>:w<CR>:!clear && python3 %<CR>

"Node js 실행 단축키<F8>
nnoremap <F8> <esc>:w<CR>:!clear && node %<CR>

"저장 후 make 명령실행
nnoremap <F12> :w<CR>:!make && ./app.out<CR>

filetype plugin indent on

"쓸 곳이 마땅치 않아서 적어두는 CMake 사용법
"1. CMakeLists.txt에 아래의 코드를 적어둔다.
"   1)cmake_minimum_required ( VERSION 3.1 ) #버전은 달라도 됨.
"   2)project ( 프로젝트명 ) #최대한 위에 있는거 권장
"   3)set ( SRC_FILES <file> <file> ... ) #소스 파일들을 담는 변수 선언
"   4)ADD_EXECUTABLE ( app.out ${SRC_FILES}) #고정적으로 이렇게 쓰면 됨
"2. 저장하고 나와서 cmake CMakeLists.txt 를 실행한다. -> Makefile을 만듬
"3. make 하면 끝! 또, 이때부터는 make clean 도 사용가능..! 아마 install도됐던거 같기도...
"ssh root@106.10.57.26 -p 2021

```

하나씩 배우면서 추가하다보니 .vimrc가 이렇게나 길어졌다.. 그래도 이 코드들을 쭉 읽다보면 대충 내용은 다 이해가 될 것이다.

어떤 것은 몇 달전에 인터넷의 어느 누군가의 세팅을 퍼오기도 했고, 어떤 것은 문법을 공부해서 내가 직접 만들기도 했다.

그리구 아직 써보지 않은 것들도 많다... 아직도 수정중인 파일이라 직접적으로 가져다가 쓰면 오류가 나던지 이상해질 수 있으니 호옥시나 가져가시려면 잘 읽어보고 필요한 내용만 긁어가셔라! (C++ 컴파일 & 빌드 관련해서는 아예 써놓지도 않았다..ㅎ)



그치만, 특별히 C++을 위해 만든 기능도 있다!

\1. 입력모드에서 "<" 입력 후 스페이스바를 누르면 "<" 대신 "<<"가 입력됨.(반대방향도 마찬가지. cout << 이떄 유용)

\2. 입력모드에서 ":main"을 입력 후 엔터를 치면(입력할 떄는 한글자밖에 안보임), iostream과 std네임스페이스를 포함해 main함수의 구조를 만들 수 있음.



아래는 사용예시 영상을 올려놓는다.

​          ![img](https://scrap.kakaocdn.net/dn/F8foY/hyLxH76Ovc/W0Mb5F8ueY30UsKJ2gh9O0/img.jpg?width=2878&height=1798&face=0_0_2878_1798)        



컴파일하고 실행할 때는 아직도 g++을 쓰는게 어색해서, 그냥 노말모드에서 명령어로 사용하곤 한다..

명령어는 일단 아래와 같다!

```bash
:!g++ currentfilename.cpp
:!./a.out
```

일단 이게 아주 원시적이고 불편한 방법인 것을 알린다.

하지만, 아는게 이것뿐이라 당장은 이렇게밖에 사용해보지를 않았다ㅠㅠ

추후에 더 배워서 편리하고 좋은 방법을 업데이트 해보겠다!
