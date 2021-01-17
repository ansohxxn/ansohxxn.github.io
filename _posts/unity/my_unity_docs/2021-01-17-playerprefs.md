---
title:  "Unity C# > UnityEngine : PlayerPrefs" 

categories:
  -  UnityDocs
tags:
  - [Game Engine, Unity]

toc: true
toc_sticky: true

date: 2021-01-17
last_modified_at: 2021-01-17
---

공부하면서 알게된 것만 정리합니다.😀
{: .notice--warning}


# 👩‍🦰 PlayerPrefs

> 유니티에서 지원해주는 데이터 자료구조 타입이다. 문자열인 Key 값과 그에 따른 value를 묶어 로컬 파일로서 저장이 된다.(<u>로컬 파일로서 저장이 되기 때문에 껏다 켜도 그 값이 유지가 된다</u>) 

- 이를 다루는 여러 함수들도 지원한다. Key 값만 알고 있다면 value를 찾을 수 있다. PlayerPref는 구조가 간단해서 해킹당하기 쉽다.


## 🚀 함수

### ✈ SetInt

> public static void SetInt(string key, int value);

```c#
PlayerPrefs.SetInt("BestScore", score);
```

"BestScore"라는 Key값에 int타입인 score를 파일로서 묶어 저장한다. 

<br>

### ✈ GetInt

> public static int GetInt(string key);

> public static int GetInt(string key, int defaultValue);

```c#
PlayerPrefs.GetInt("BestScore");
```

  - "BestScore"라는 Key값의 int 타입인 value값을 리턴한다.
  - "BestScore"라는 Key값이 없더라도 에러는 발생하지 않는다. Key값이 없으면 0을 리턴함.

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}