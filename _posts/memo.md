---
layout: single
title: ""
categories: 
tag: []
toc: true
toc_sticky: true
author_profile: true
search: true
typora-root-url: ../
published: false
---

# 주제들

categories: [cpp, python, go, papers, money, math, mldl, debug, books, blockchain, etc] 중 택 1

## 카테고리 추가시 해야할 일

- `Blog/_includes/nav_list_main` 파일 수정
- `Blog/_pages/categories` 에 md 파일 추가
- 

## 단행본

- W and B 사용법
- 효율적인 논문을 위한 MS 워드 사용법
- 효율적인 논문을 위한  한글 사용법
- 깃허브 사용법
- 깃 가이드
- enum에 대하여
- 혼동행렬, precision과 recall에 대하여
- logistic과 logit, 소프트맥스에 대하여
- 허깅페이스 BERT 커스텀 포지션 임베딩 만드는 법
- layer normalization과 batch normalization
- YOLO
- Pooling이란
- LabView
- 태양광 패널의 원리와 효율 그리고 사업성 계산
- 친환경 발전 방법 비교
- 세금/경제 용어
- TF-IDF

## 장기 시리즈

- OpenCV, C++, Python, Go, Rust 문법
- 유체역학
- 물리학
- 미적분
- 선대수
- 통계
- 확률
- 독후감
- 코세라 스터디 내용 정리
- 캐글 활동 정리
- 논문 리뷰
- 기후 위기 관련 연구들
- docker

---

[10.11]

오늘 공부 흐름: (연구 주제를 정하라)=>(GAN-Chat)=>(토크나이저가 필요하다)=>(sentencepiece, Auto Encoder)=>(토크나이저 모델의 학습 방법을 MLM이나 NSP 같이 모음, 자음 단위로 하는 건 어떨까?)=/음성인식/=>(음성인식 기능은 적용될 수 있을까?)=>(Kospeech)=>(Hydra)=>(Auto Sound Recogintion)=>(TTS)=>(Diffusion model)=>(ELBO, Evidence of Lower BOund)=>(KL Divergence)

> 결과적으로 뭔가를 얻어낸 것은 없다. 하지만 느낀 것들은 존재한다.
>
> - <u>다음 지식들에 대한 공부의 필요성: KL Divergence, ELBO, Diffusion Model(최근 이슈), Auto Encoder</u>
> - <u>아카이브와 paperwithcode를 매일 확인하고 오늘의 논문을 확인하는 습관</u>
> - 토크나이저의 종류와 그에 대한 깊은 연구의 필요성(추후 연구 분야)
> - 연구 분야에 대한 고찰: 한국어형 질의 응답 서비스 관련 원천 연구(토크나이저, 음성인식(STT), TTS, 모델 아키텍처, 경량화, 정보 검색 등) => 미래 먹거리와 직결. 경쟁력있는 분야로 보임. 
>     - 기업)솔트룩스 알아보기(최신 기술, 적용 분야 등)
>     - 기업형 상담 시스템은 자사 혹은 여러 케이스에 대한 정보 검색도 뒷받침 되어야함
>     - 그리고, 이미지처리는.. 자연어처리를 정말 잘 할 때까진 메인으로 두거나 하지 말자.. 관련 논문 따라가는 정도
>     - 돈이 되는 분야, 소위 팔리는 분야일 것이고 결국 AI 비서와도 연결됨
>     - 임희석 교수님 랩실 컨택 생각. 그 외 이상구(SNU), 황승원(SNU) 교수님 등
> - <u>토크나이저의 성능 향상 연구를 차후 목표로 설정!</u>
>     - 관련 실험과 평가 데이터 수집(논문을 많이 비교해보는게 좋다)
