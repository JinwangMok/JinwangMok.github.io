---
title: "Go 저장소"
layout: archive
permalink: categories/golang
author_profile: true
sidebar_main: true
---


{% assign posts = site.categories.go %} <!-- site.categories.XXX는 문서 위에서 category:XXX 로 설정한 값임-->
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}