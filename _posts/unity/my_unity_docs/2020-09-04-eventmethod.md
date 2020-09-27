---
title:  "Unity C# > 이벤트 함수 정리" 

categories:
  -  UnityDocs
tags:
  - [Game Engine, Unity]

toc: true
toc_sticky: true

date: 2020-09-04
last_modified_at: 2020-09-04
---

공부하면서 알게 된 **이벤트 함수**들을 정리한 문서입니다.😀
{: .notice--warning}

- 유니티 공식 매뉴얼 <https://docs.unity3d.com/kr/current/Manual/UnityManual.html>
- Scripting Overview <http://www.devkorea.co.kr/reference/Documentation/ScriptReference/index.html>


# 이벤트 함수

> 실수로 이름 오타나면 절대 실행 안된다!! (당연한 얘기지만.. 😂)

- 오타여도 사용자 지정 함수인 줄 알고 잡아주지도 않음 ㅠ ㅠ OnTriggerEnter 인데 onTriggerEnter 라고 소문자로 써서 계속 실행되지 않았었는데 한참을 헤맸다...
- 이름 실수 하지 않도록 주의 하자!!

<br>

## 👩‍🦰 void Start()
- <u>컴포넌트 초기화</u> 부분
  - 게임이 처음 활성화 되는 순간에 유니티가 Start 메세지를 브로드캐스팅 하여 뿌려 온 컴포넌트들을 각자 구현된 Start 내용대로 초기화 시킨다.
  - 유니티는 게임 시작할때 Start 메세지를 뿌린다.
- Awake() 후 + Update() 전에 1회 동작하는 이벤트 함수다.

<br>

## 👩‍🦰 void Awake()
- Start()와 비슷한데 Start()보다도 먼저 실행되는 시작 함수다.
  - Start 함수의 이전 및 프리팹의 인스턴스화 직후에 호출
- 생성 하자마자 들어가는 1회 동작 함수
- Start 메세지보다 더 빨리 호출되므로 다른 스크립트들의 Start보다 더 빨리 실행되야 하는 내용이 있으면 Awake에 구현하면 된다.

<br>

## 👩‍🦰 void Update()
- 1초에 수십번씩 자신의 상태를 갱신하고 주기적으로 계속 실행 
  - 외부에서 직접 찾아 실행할 필요 없음. 
- <u>스스로 매 프레임마다 실행 됨.</u>
- <u>프레임 속도는 환경마다 다르기 때문에 물리 처리를 update() 함수에서 해주면 환경에 따라 물리 처리 오차가 발생할 수 있어 비추천</u>

<br>

## 👩‍🦰 void FixedUpdate()

> 디폴트로 0.02초 (초당 50회)마다 실행된다.

- <u>Update()처럼 매번 반복 실행</u>되나 프레임에 기반하지 않고 어떤 고정적이고 동일한 시간에 기반하여 실행된다.
  - Update()와의 차이점
    - Update()는 화면 한번 깜빡일때마다 실행되서 렉걸리거나 환경이 안좋거나 하면 그만큼 Update()도 적게 실행되지만
    - Fixedupdate()는 환경에 상관없이 무조건 실행 횟수를 지킨다. 정해진 수만큼 무조건 실행 함
- <u>프레임과 상관없이 고정적인 시간마다 실행되기 때문에 환경에 상관없이 물리처리를 오차 없이 실행시킬 수 있다.</u>
  - 물리처리는 **FixedUpdate()** 안에서 해주기.
- `Time.fixedDeltaTime` 의 시간 간격으로 실행이 된다. 디폴트로 `Time.fixedDeltaTime` 값은 0.02f 이다.

<br>

## 👩‍🦰 void LateUpate()
  - 유니티 함수로, <u>매 프레임마다 실행되지만 Update()함수 보다 늦게 실행된다.</u>
    - <u>Update() 함수 실행이 다 끝난 후에, Update()함수의 종료 시점에 맞춰서 LateUpdate() 가 실행</u>된다.
  - 예를 들어 캐릭터의 이동 방향 계산은 Update() 에서 끝내준 후,Update()에서 계산 끝낸 캐릭터의 이동 방향에 따라 LateUpdate()에서 카메라가 캐릭터를 따라가도록 하는 식으로 구현한다.

<br>

## 👩‍🦰 void OnTriggerEnter(Collider other)
- On<u>Trigger</u>Enter : `Trigger`인 Collider와 충돌할 때 자동으로 실행된다. 
  - `Is Trigger`가 <u>체크된 Collider와 충돌하는 경우 발생되는 메세지</u>
  - 사실 충돌하는 두 오브젝트 중 하나만 Trigger 라도 두 오브젝트 모두 이 함수가 실행된다.
    - 즉 물리적 충돌은 일어나지 않고 닿기 만 하더라도, 뚫고 지나가지만 그래도 이벤트 발생은 시켜야 하는 경우.
- <u>오브젝트끼리 충돌하면 유니티에서 OnTriggerEnter 메세지를</u> ***<u>충돌한 오브젝트들</u>*** 에게 브로드캐스팅 한다.
  - 충돌한 두 오브젝트끼리는 서로 독립적이고 연관이 없으니 서로 충돌한 사실을 모르지만 두 오브젝트가 충돌하면 유니티에서 두 오브젝트에게 OnTriggerEnter 메세지를 뿌리므로 두 오브젝트는 <u>OnTriggerEnter 함수 안에 구현한 내용대로 충돌 처리를 한다.</u>
- 유니티는 충돌한 상대 오브젝트의 정보를`Collider`타입의 `other`가 참조하도록 넘겨준다.  
  - 충돌한 <u>상대 오브젝트에 붙어있는 Collider 컴포넌트</u> 
    - `Collider` 컴포넌트 👉🏻 <u>물리적 표면</u>
- 이 스크립트가 붙은 오브젝트(나 자신)가 다른 오브젝트와 충돌시 `OnTriggerEnter` 이벤트가 발생하기 위한 조건. 아래 조건을 전부 만족해야 이 이벤트가 발생할 수 있다.
  1. 내가 혹은 상대방 둘 중 하나는 꼭 Rigidbody 컴포넌트를 가지고 있어야 한다. 
    - `IsKinematic` 체크 여부는 상관 없다.
  2. 나 그리고 상대방 둘 다 모두 Collider 컴포넌트를 가지고 있어야 한다.
    - 단, 둘 중 하나라도 `IsTrigger`는 반드시 켜져 있어야 함

```c#
void OnTriggerEnter(Collider other)
{
    Debug.Log("충돌 발생!");
} 
```
- 위 스크립트가 붙어있는 오브젝트에서 충돌이 일어날 때마다 콘솔창에 "충돌 발생" 메세지 출력.

<br>

### OnTriggerEnter, OnTriggerExit, OnTriggerStay의 차이
- OnTrigger<u>Enter</u> : `Enter`는 충돌하는 순간
- OnTrigger<u>Exit</u> : `Exit`는 떼어지는 순간. 더 이상 붙어 있지 않는 순간.
- OnTrigger<u>Stay</u> : `Stay`는 충돌 중인, 붙어 있는 동안.

<br>

## 👩‍🦰 void OnCollisionEnter(Collision other)
- On<u>Collision</u>Enter : Trigger가 체크되지 않은 <u>일반 Collider를 가진 오브젝트와 충돌한 경우</u> 자동으로 실행된다.
- `Collider`와 `Collision`의 차이
  - `Collision`은 충돌한 상대 오브젝트에 대한 많은 정보를 담고 있다. 나랑 부딪친 오브젝트의 Transform, Collider, GameObject, Rigidbody, 상대 속도 등등이 `Collision`에 담겨서 들어 온다. 물리적인 정보가 더 많이 들어 있다.
  - `Collider`는 `Collision`보다는 담고 있는 정보가 적다. 물리적인 정보는 담고 있지 않아서.

```c#
    private void OnCollisionEnter(Collision collision)
    {
        Debug.Log(collision.gameObject.name);
    }
```

- 이 스크립트가 붙은 오브젝트(나 자신)가 다른 오브젝트와 충돌시 `OnCollisionEnter` 이벤트가 발생하기 위한 조건. 아래 조건을 전부 만족해야 이 이벤트가 발생할 수 있다.
  1. 내가 혹은 상대방 둘 중 하나는 꼭 <u>Rigidbody 컴포넌트</u>를 가지고 있어야 한다. 
    - `IsKinematic`은 꺼져 있어야 함
    - 즉, 둘 중 하나는 꼭 충돌로 인한 물리적인 힘에 영향을 받을 수 있는 상태여야 함. 그래서 OnCollisionEnter는 뭔가 물리적인 힘에 의한 충돌 느낌
  2. 나 그리고 상대방 둘 다 모두 Collider 컴포넌트를 가지고 있어야 한다.
    - `IsTrigger`는 꺼져 있어야 함

FPS 게임 같은데서 총알이 사람에게 맞으면 총알 입장에선 사람 오브젝트 정보가 Collision으로 들어오게 되므로 그 사람의 체력을 깎거나 하는 처리를 할 수 있다.


<br>

## 👩‍🦰 void OnEnable()

> 해당 스크립트(컴포넌트) 혹은 스크립트가 붙어있는 오브젝트가 <u>활성화 될 때마다 자동으로 실행된다.</u>

`컴포넌트`가 꺼져 있다가 켜지는 상태마다 발동된다. Start()와 비슷하지만 Start()는 게임 시작시 한번 발동되는 반면 OnEnable()은 컴포넌트가 꺼져있다가 켜질 때마다 1회씩 실행된다. 
- 게임 시작되자마자 `enabled = true` 상태인 컴포넌트들에게 발동
- 게임 도중에`enabled = false`이다가 `enabled = true`가 되는 컴포넌트들에게 발동
  - 오브젝트 끌 때는 `SetActive(false)`
  - 컴포넌트나 스크립트를 끌 때는 `enabled = false`

<br>

## 👩‍🦰 void OnDisable()

> 해당 스크립트(컴포넌트) 혹은 스크립트가 붙어있는 오브젝트가 <u>비활성화 될 때마다 자동으로 실행된다.</u>

`컴포넌트`가 켜져 있다가 꺼지는 상태마다 발동된다. 

<br>

## 👩‍🦰 void OnDestroy()
오브젝트가 Destroy 될 때 자동 호출되는 이벤트 함수다. 마치 소멸자 같은 것.

<br>

## 👩‍🦰 void OnAnimatiorIK()

- 애니메이션의 관절 꺾는 IK 정보가 갱신될 때마다 발동되는 이벤트 함수.
- Animator와 관련된 이벤트로 애니메이터가 붙어있는 오브젝트만 이 이벤트 함수를 동작시킬 수 있다.
- 씬상에 존재하기는 하나 아직 보이지는 않는 그런 오브젝트들을 기즈모를 통해 보여줌으로서 위치 조정을 쉽게 해주는 등등 개발자 편의를 돕는다. 

<br>

## 👩‍🦰 void OnDrawGizmos()

- 유디터 에디터 내에서만, 씬 상에서만 <u>매 프레임 기즈모를 그리는 역할</u>을 한다.
- 이 함수가 소속된 스크립트가 컴포넌트로서 붙은 오브젝트가 Scene 화면 상에서 항상 보이도록 하는 이벤트 함수.
- 씬상에 존재하기는 하나 아직 보이지는 않는 그런 오브젝트들을 기즈모를 통해 보여줌으로서 위치 조정을 쉽게 해주는 등등 개발자 편의를 돕는다. 

<br>

## 👩‍🦰 void OnDrawGizmosSelected()

- 유디터 에디터 내에서만, 씬 상에서만 <u>Hierarchy 창에서 선택된 오브젝트에 한해서만 매 프레임 기즈모를 그리는 역할</u>을 한다.
- 이 함수가 소속된 스크립트가 컴포넌트로서 붙은 오브젝트가 선택됐을때만 보이도록 하는 이벤트 함수.

<br>

## 👩‍🦰 사용자 지정 이벤트
해당 이벤트에 원하는 함수가 들어 있는 스크립트들을 드래그 해 와서 발동시킬 함수들을 선택하면 해당 이벤트를 발동시켰을 때 등록한 함수들도 다 같이 실행된다. 

```c#
using UnityEngine.Events;

public UnityEvent myEvent;

myEvent.invoke();
```

1. *using UnityEngine.Events;*
2. myEvent라는 이름의 사용자 지정 이벤트 변수를 선언한다. 
3. 이제 유니티에서 myEvent 슬롯이 열릴텐데 여기에 원하는 스크립트를 드래그 앤 드롭해준다.
4. 원하는 함수들을 선택한다.
5. invoke() 해주면 등록한 함수들이 모두 실행된다. 


***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}