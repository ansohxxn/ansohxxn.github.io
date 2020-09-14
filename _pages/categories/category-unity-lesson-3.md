---
title: "케이디님의 '유니티로 생존 게임 만들기' 강의 필기"
layout: archive
permalink: categories/unity-lesson-3
author_profile: true
sidebar_main: true
---

<!-- 공백이 포함되어 있는 카테고리 이름의 경우 site.categories['a b c'] 이런식으로! -->

***

{% assign posts = site.categories.['Unity Lesson 3'] %}
{% for post in posts %} {% include archive-single2.html type=page.entries_layout %} {% endfor %}