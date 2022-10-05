---
title: "⛓블록체인 저장소"
layout: archive
permalink: categories/blockchain
author_profile: true
sidebar_main: true
---


{% assign posts = site.categories.blockchain %} <!-- site.categories.XXX는 문서 위에서 category:XXX 로 설정한 값임-->
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}