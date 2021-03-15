---
title:  "[Github 블로그] utterances 으로 댓글 기능 만들기 (+ disqus 비추후기)" 

categories:
  - Blog
tags:
  - [Blog, jekyll, Liquid, HTML, minimal-mistake]

toc: true
toc_sticky: true
 
date: 2021-03-15
last_modified_at: 2021-03-15
---

## 🚀 disqus 비추후기

- disqus 댓글 플랫폼을 비추하는 이유
  1. 광고 !!!!!! (아래 사진 속 광고는 모두 disqus 하나에 함께 딸려있는 광고다..)
  2. 무겁다.

![image](https://user-images.githubusercontent.com/42318591/111132481-c1ff1e00-85bc-11eb-887e-b2f476967827.png)


블로그를 거의 1년 가까이 해오는 동안 블로그의 댓글 플랫폼으로 `disqus` 을 사용해왔다. 그러나 언제부턴가 저런 못생긴ㅠㅠ.. 큰 격자형태의 광고가 댓글의 앞뒤로 붙는 것이였다. 모바일로 보면 더 가관이였다.. 해당 광고 소스를 찾지 못해서 한참을 헤매다가 이 광고는 `disqus` 자체 광고라는 것을 알게 되었다. `disqus`에 광고가 붙는다는 것을 전혀 몰랐었다. 처음엔 안 붙었는데 어느 순간부터 나도 모르는 사이에 광고가 붙기 시작했던거보면 어느 정도의 시간이 지나고나면 광고가 붙게되는듯 하다. 이 광고를 없애려면 한달에 9달러씩을 지불해야 한다더라..😥 

<br>

## 🚀 utterances 가 더 좋다고 생각하는 이유

그래서 다른 댓글 플랫폼을 사용하고자 찾아보다가 ***utterances*** 라는 댓글 플랫폼을 발견하였고 이걸로 갈아타기로 결정하였다. 우선 <u>광고가 없고 가볍다고 하며</u> Github 계정 로그인을 통해 댓글을 달고, 댓글이 달리면 알림이 Github Repository 의 Issue 로 올라오는 시스템이라고 한다. (Issue 가 올라오는 것이니 메일로 알림을 받을 수 있다.) 게다가 댓글에 마크다운을 사용할 수도 있다고 한다. 

<br>

## 🚀 블로그에 utterances 적용시키기

1. 댓글 Issue 가 올라올 저장소를 정하거나 혹은 생성한다.
  - 그냥 깃허브 블로그 Repository 에 하면 될 것 같다.
  - 나 같은 경우는 블로그 Repository 가 `private`라서 댓글 이슈가 올라올 전용 Repository 를 아예 새로 만들었다. <https://github.com/ansohxxn/comments>
2. <https://github.com/apps/utterances> 에서 utterance 를 Install 설치한다.
  - Only Select Repositories 를 통해 댓글 Issue 가 올라올 저장소로 선택한 그 Repository 를 선택한 후 Install 하면 된다.
3. Install 후 나오는 다음 페이지에서 아래와 같이 `repo`에 위 저장소 permalink 를 입력하고 (*github아이디/저장소이름*) 
  
  ![image](https://user-images.githubusercontent.com/42318591/111189434-6c953200-85f9-11eb-82f8-010b41bbe6dd.png)

  ![image](https://user-images.githubusercontent.com/42318591/111189479-761e9a00-85f9-11eb-9ebe-4e0add550f8c.png)




***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}