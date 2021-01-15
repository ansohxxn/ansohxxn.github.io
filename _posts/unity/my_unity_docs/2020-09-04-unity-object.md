---
title:  "Unity C# > GameObject 내장 함수/변수들 정리" 

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


# GameObject 클래스

- UnityEnine에 내장되어 있다.
- 모든 오브젝트는 동일하게 이 GameObject 타입이다.
  - 오브젝트는 컴포넌트들을 부품으로서 가진다 (Has-A)
  - 모든 오브젝트들은 Transform 컴포넌트를 가진다. 
- 반면에 컴포넌트들은 서로 각각 다양하고 다른 타입이다.
  - 모든 컴포넌트는 자신의 주인인 오브젝트에 접근할 수 있다.  

## 변수/프로퍼티

- `gameObject` 
  - **자기 자신**(컴포넌트 or 스크립트)가 붙은 오브젝트를 뜻함. 
    - *GameObject gameObject = new GameObject()*이 숨겨져 있다고 보면 된다.
- `name`
  - 해당 오브젝트의 이름.
- `activeSelf`
  - 오브젝트가 활성화 상태면 True 리턴, 아니면 False 리턴.

## 함수

- 그냥 호출하면 👉 이 스크립트가 붙어 있는 오브젝트(나 자신)에 대해 함수 호출
  - GetCOmponent\<Rigidbody>();
- 다른 오브젝트로 호출하면 👉 그 오브젝트에 대해 함수 호출
  - obj.GetCOmponent\<Rigidbody>();

### GetComponent<Component>()
- `GetComponent<컴포넌트 이름>()`
  - 두 가지 호출 방법
    - 그냥 *GetComponent\<컴포넌트 이름>()*만 쓰면 **이 스크립트가 붙어 있는 오브젝트(나 자신)**에 붙어 있는 모든 컴포넌트들 중에서 해당 컴포넌트를 찾음
    - *obj.GetComponent\<컴포넌트 이름>()* 처럼 어떤 게임 오브젝트에서 호출한거면 호출한 그 게임오브젝트의 컴포넌트들에서 찾는다.
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

### GetComponentsInChildren<Component>()
- <u>내 자식 오브젝트들 중에서</u> `<>`안에 적혀 있는 컴포넌트가 실존하여 오브젝트에 붙어 있는 상태라면 그 <u>붙어 있는 컴포넌트들을 모두 모아 배열로 리턴해준다.</u>
  - 손자의 증손자들까지 recursive하게 모~~~~~~든 자식들을 중에서 해당하는 컴포넌트를 찾는다.

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
  - <u>생성한 오브젝트를 리턴</u>함.
- Instantiate(GameObject, Vector3(position), Quaternion(rotation))
  - 위치와 회전 벡터를 추가로 매개변수로 넣어주어 찍어낼 오브젝트의 위치와 회전값을 설정할 수도 있다.
  - 위치, 회전 지정 안해주면 랜덤 위치나 원점에서 생성됨.


<br>

### FindObjectsOfType<Object>();
- 씬 상의 모든 오브젝트들을 뒤져서 직접 `<>`안에 있는 <u>타입(오브젝트 형이라던가 컴포넌트 형)과 일치하는 오브젝트</u>들을 찾아서 **리스트**로 리턴해준다.
- 느리다. 이런 성능상에 문제 때문에
  - Awake() 함수 안에서만 구현되거나 아주 적은 횟수로 호출되게끔 해야한다. 매프레임 실행되는 Update() 같은 함수에서는 호출 하지 말자 !
- 's'가 빠진 FindObjectOfType\<Object>() 함수는 처음 나오는 오브젝트 하나만 리턴해준다.

<br>

### Find(string)

- 클래스 함수라 `GameObject.Find(string)`으로 호출해야 한다.
- **활성화 되어 있는** 오브젝트들을 뒤져서 직접 `<>`안에 있는 <u>이름(string)과 일치하는 오브젝트</u>를 찾아 리턴해준다.
- 오브젝트가 많다면 느리다.

<br>

### DontDestroyOnLoad(GameObject) 
- 다른 Scene으로 변경되더라도 파괴되지 않고 유지할 오브젝트를 지정하는 함수다.
- 인수로 넘긴 오브젝트가 절대 삭제되지 않도록 한다.

<br>

### GameObject.FindGameObjectWithTag(String)

```c#
GameObject.FindGameObjectWithTag("Monster");
```
해당 태그가 붙은 오브젝트를 찾아서 리턴한다.

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}