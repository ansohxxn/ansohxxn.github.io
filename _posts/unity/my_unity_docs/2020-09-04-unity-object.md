---
title:  "Unity C# > Object 내장 함수/변수들 정리" 

categories:
  -  UnityDocs
tags:
  - [Game Engine, Unity]

toc: true
toc_sticky: true

date: 2020-09-04
last_modified_at: 2020-09-04
---

공부하면서 알게 된 **유니티 Object 내장 함수/변수**들을 정리한 문서입니다.😀
{: .notice--warning}

- 유니티 공식 매뉴얼 <https://docs.unity3d.com/kr/current/Manual/UnityManual.html>
- Scripting Overview <http://www.devkorea.co.kr/reference/Documentation/ScriptReference/index.html>


# 오브젝트

- 오브젝트를 참조하고 있는 변수로 함수를 호출할 수 있다. *ex) winUI.SetActive(true);*
- 오브젝트들의 내장되어 있는 변수나 함수들.

## 변수/프로퍼티

- `gameObject` 
  - 자기 자신(컴포넌트 or 스크립트)가 붙은 오브젝트를 뜻함. 
  - 그러니 자기 자신이라고 해석해도 괜춘!
- `name`
  - 해당 오브젝트의 이름.

## 함수

### GetComponent<Component>()
- `GetComponent<컴포넌트 이름>()`
  - UnityEngine에서 지원하는 함수다.
  - 이 스크립트가 붙어 있는 오브젝트에 `<>`안에 적혀 있는 컴포넌트가 실존하여 오브젝트에 붙어 있는 상태라면 그 <u>붙어 있는 컴포넌트를 리턴해준다.</u>
  - 이 방법을 사용하면 슬롯 없이 그냥 코드에서 바로 해당 컴포넌트를 변수에 연결시킬 수 있다.

  ```c#
  Rigidbody rb = GetComponent<Rigidbody>(); // 오브젝트가 Rigidbody 컴포넌트를 갖고 있다면 그 컴포넌트를 이제 rb 변수가 참조하게 된다.
  ```
<br>

### GetComponentInChildren<Component>();
- `GetComponentInChildre<컴포넌트 이름>()`
  - <u>내 자식 오브젝트들 중에서</u> `<>`안에 적혀 있는 컴포넌트가 실존하여 오브젝트에 붙어 있는 상태라면 그 <u>붙어 있는 컴포넌트를 리턴해준다.</u>

<br>

### AddComponent<Component>()
- `AddComponent<추가할 컴포넌트 이름>()`
  - 오브젝트이름.AddComponent\<ScoreManager>() 
    - ScoreManager라는 <u>컴포넌트를</u> 해당 오브젝트에 <u>붙여준다.</u>
    - ScoreManager라는 스크립트가 해당 오브젝트에 붙게 됨.
  - 오브젝트이름.AddComponent\<Rigidbody>() 
    - Rigidbody 컴포넌트가 해당 오브젝트가 붙게 됨. 

<br>

### Destroy(Object)
- 매개변수에 있는 오브젝트를 파괴한다.
- `Destroy(gameObject)` : 자기 자신 파괴
- `Destroy(gameObject, float a)` : a초 뒤에 자기 자신을 파괴한다. 이렇게 시간을 줄 수도 있음.

<br>

### SetActive(bool)
변수가 참조하고 있는 오브젝트를 보이게끔 켜주는 함수. 체크가 해제 되어 보이지 않는 오브젝트를 `SetActive(true)` 해주면 안보이던 오브젝트가 다시 화면에 보이게 되고 반대로 `SetActive(false)` 해주면 더 이상 화면에 보이지 않게 된다.
- 오브젝트 끌 때는 `SetActive(false)`
- 컴포넌트나 스크립트를 끌 때는 `enabled = false`

<br>

### Instantiate(GameObject)
- 게임 플레이 도중에 매개변수에 들어온 <u>오브젝트를 복사</u>하여 게임 도중에 생성해낸다.
- Instantiate(GameObject, Vector3(position), Vector3(rotation))
  - 위치와 회전 벡터를 추가로 매개변수로 넣어주어 찍어낼 오브젝트의 위치와 회전값을 설정할 수도 있다.
  - 위치, 회전 지정 안해주면 랜덤 위치나 원점에서 생성됨.

<br>

### FindObjectOfType<Object>();
- 씬 상의 모든 오브젝트들을 뒤져서 직접 `<>`안에 있는 이름과 일치하는 오브젝트를 찾아서 <u>리스트로</u>리턴해준다.
- 느리다. 이런 성능상에 문제 때문에
  - Awake() 함수 안에서만 구현되거나
  - 아주 적은 횟수로 호출되게끔 해야한다.

<br>

### DontDestroyOnLoad(GameObject) 함수
- 다른 Scene으로 변경되더라도 파괴되지 않고 유지할 오브젝트를 지정하는 함수다.

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}