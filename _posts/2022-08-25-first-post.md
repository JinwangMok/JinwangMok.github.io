---
layout: single
title: "github jekyll 블로그 이해하기"
categories: test
tag: [blog, python, jekyll]
toc: true
author_profile: false
sidebar:
  nav: "docs"
search: false
---



> 본 포스트는 테디노트님의 유튜브 시리즈 [`깃헙(github) 블로그 만들기 - 시즌1`](https://www.youtube.com/watch?v=--MMmHbSH9k&list=PLIMb_GuNnFwfQBZQwD-vCZENL5YLDZekr) 을 통해 블로그를 개설한 다음 지킬(jekyll) 블로그를 이해하고자 제작중입니다.



# 1. 용어 정리

- **jekyll**: 정적 사이트 생성기(static site generator). 데이터베이스를 활용해 동적으로 웹페이지를 생성하는 것과 달리 YAML, JSON, CSV, TSV 등의 파일과 마크다운(Markdown, md) 파일로 정적인 웹페이지를 생성하는 방식.
- **JSON**: JavaScript Object Notation. 말그대로 자바스크립트의 object 자료형에 해당하는 문법으로 구성된 파일을 의미하며, 키:값으로 구성된 데이터를 의미. pyhton의 dictionary 자료형과 유사함. 예를 들어 { "이름":"홍길동", "나이":18, "취미":["게임", "독서"] } 식의 데이터.
- **YAML**: Yet Another Markup Language. JSON처럼 데이터를 저장하는 형식



