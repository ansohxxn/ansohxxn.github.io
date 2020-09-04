---
title:  "Unity C# > UnityEngine.SceneManager 정리" 

categories:
  -  UnityDocs
tags:
  - [Game Engine, Unity]

toc: true
toc_sticky: true

date: 2020-09-04
last_modified_at: 2020-09-04
---

공부하면서 알게 된 **UnityEngine.SceneManager**를 정리한 문서입니다.😀
{: .notice--warning}

- 유니티 공식 매뉴얼 <https://docs.unity3d.com/kr/current/Manual/UnityManual.html>
- Scripting Overview <http://www.devkorea.co.kr/reference/Documentation/ScriptReference/index.html>


# UnityEngine.SceneManager

> `using UnityEngine.SceneManagement`을 해주어야만 사용할 수 있다. 

> scene과 scene을 넘나 드는 작업을 하고 싶을 때 사용.

## 변수/프토퍼티

## 함수

### `SceneManager.LoadScene("씬 이름" 혹은 "씬 인덱스"), 모드)`;

>  해당 씬을 로드한다.
- 첫번째 인수로 씬의 이름 문자열이나 씬의 인덱스를 넘긴다.
- 모드는 `LoadSceneMode.Single`, `LoadSceneMode.Additive` 이렇게 2가지 있는 Single이 디폴트 값이다. 따라서 인수 한개만 넘기면 싱글 모드로 씬을 로드한다.
    - `LoadSceneMode.Single` : 현재 씬의 오브젝트들을 모두 Destroy하고 새롭게 씬을 로드
    - `LoadSceneMode.Additive` : 현재 씬에 새로운씬을 추가적으로 덧대어 로드. 말풍선을 덧붙이는 느낌.

### `SceneManager.GetActiveScene()`

- 현재 활성화 되있는 씬을 리턴한다.
- SceneManager.GetActiveScene().`buildIndex`
  - 현재 활성화 되있는 씬의 인덱스 리턴
- SceneManager.GetActiveScene().`name`
  - 현재 활성화 되있는 씬의 이름 리턴
  
  ```c#
  SceneManager.LoadScene(SceneManager.GetActiveScene().name);  // 현재 활성화 되어있는 씬을 재시작
  ```


***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}