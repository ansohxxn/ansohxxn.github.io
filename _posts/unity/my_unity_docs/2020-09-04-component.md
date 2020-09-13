---
title:  "Unity C# > 컴포넌트 정리" 

categories:
  -  UnityDocs
tags:
  - [Game Engine, Unity]

toc: true
toc_sticky: true

date: 2020-09-04
last_modified_at: 2020-09-04
---

공부하면서 알게 된 **유니티 컴포넌트**들을 정리한 문서입니다.😀
{: .notice--warning}

- 유니티 공식 매뉴얼 <https://docs.unity3d.com/kr/current/Manual/UnityManual.html>
- Scripting Overview <http://www.devkorea.co.kr/reference/Documentation/ScriptReference/index.html>

# 컴포넌트 

## 👩‍🦰 Transform

> 오브젝트의 위치, 회전, 크기를 나타내는 컴포넌트다.

- 오브젝트를 생성하면 기본으로 붙어있는 디폴트 컴포넌트다.
  - `Transform myTransform`하고 변수 선언을 해준 후 `myTransform.Rotate()` 이런식으로 쓰는 일반적인 방법도 있긴 하지만 Transform 컴포넌트는 모든 오브젝트들이 디폴트로 갖고 있는 컴포넌트기 때문에 그냥 변수 선언 없이 `transform` 소문자 transform으로 바로 사용하는게 가능하다. `transform.Rotate()` 이런 식으로.
  
    - 함수 `SetParent(부모 Transform)` 로 부모 오브젝트를 지정해줄 수 있다.
      ```c#
      effect.transform.SetParent(parent);
      ```

### 변수/프로퍼티

- 오브젝트의 `position`, `rotation`, `scale` 즉 위치, 회전, 크기를 담당한다. 
  - `localPosition`, `localRotation`, `localScale`은 Local좌표계에서의 위치.
- ✨ <u>부모 자식 관계</u> 또한 Transform이 관리한다.
  - `transform.parent` : 내 부모 오브젝트를 뜻한다.

### 함수

#### Translate(Vector3)
- 매개변수로 들어온 벡터값 만큼 평행이동 시킨다.
- `Translate(Vector3), Space.World)`
  - Translate 함수 매개변수로 Space.World를 넘기면 Global 좌표계를 기준으로 평행이동한다.
  - 매개변수 디폴트 값은 Space.self (Local)
    - Translate, Rotate함수는 `Local`인 반면 그냥 바로 변수로 접근하는 `position`, `rotation`같은 것들은 `Global`좌표계 기준이다.
      - 다만 부모가 있는 자식 오브젝트라면 `Local`임

#### Rotate(Vector3)
- <u>현재 회전에서</u> 매개변수로 들어온 Vector3만큼 <u>더</u> x, y, z 방향으로 각각 a, b, c도 만큼 <u>회전</u>을 시킨다.
- Translate과 마찬가지로 Space.Self가 디폴트라 로컬 좌표계를 기준으로 회전한다.

#### SetParent(object)
- 함수 `SetParent(부모 Transform)` 로 부모 오브젝트를 지정해줄 수 있다.
  ```c#
  effect.transform.SetParent(parent);
  ```

<br>

<br>

## 👩‍🦰 Collider

> <u>물리적인 표면</u>을 가지게 됨. 충돌이 감지되는 영역.

- 3D 오브젝트를 생성하면 기본으로 붙어있는 디폴트 컴포넌트다. (빈 오브젝트에선 디폴트가 아님)
- 구 오브젝트에 Rigidbody를 붙여주고 플레이를 누르면 중력을 받아 밑으로 떨어지면서 <u>Plane에 부딪쳐 더 이상 떨어지지 않는데 이는 Collider 컴포넌트 때문이다</u>
- <u>물리적으로 오브젝트끼리 표면에 충돌이 일어났을 때를 감지</u>하고 이를 처리하는 컴포넌트다.

### 변수/프로퍼티

- `Is Trigger`
  - 체크 해주면 ✨<u>물리적인 표면은 없어지지만 충돌은 감지해준다</u>✨
    - 즉 충돌이 일어나면 그 충돌은 감지하지만 물리적인 표면은 없어지기 때문에 뚫고 지나갈 수 있다. 충돌 처리가 없어져서.

<br>

## 👩‍🦰 Capsule Collider

> 캡슐 모양의 Collider로 주로 인체, 나무, 가로등과 같은 긴 형태의 모델의 Collider로 사용 된다. 

Collider는 <u>물리적으로 오브젝트끼리 표면에 충돌이 일어났을 때를 감지</u>하고 이를 처리하는 컴포넌트다. 



<br>

## 👩‍🦰 Mesh Renderer
>  오브젝트에게 그림을 그려주는 컴포넌트

- 마치 물감 같은 컴포넌트
- **<u>Material</u>**을 다룬다.

<br>

## 👩‍🦰 Skinned Mesh Renderer
> Bone 애니메이션을 렌더링 할 때, 애니메이션으로 메시를 변경하기 위해 사용하는 컴포넌트.

<br>

## 👩‍🦰 Camera
> 카메라 컴포넌트

### 변수/프로퍼티

  - `Camera.main`
    - "MainCamera" 태그가 붙은 현재 활성화 되어 있는 카메라.

### 함수

#### `ViewportToWorldPoint(Vector3)`
- 카메라 오브젝트에 내장되어 있는 함수로서 카메라가 찍고 있는 화면(뷰포트)상의 위치를 Vector3로 넘기면 그 위치를 실제 게임 월드의 위치로 리턴해준다.
- 카메라가 찍고 있는 화면은 (x, y) 2D 좌표를 사용하며 화면의 좌측 하단은 (0,0)이며 화면의 우측 상단은 (1,1)이다. 
- z 값은 카메라 오브젝트로부터 실제 카메라가 찍고 있는 화면까지의 실제 게임 월드 거리, 즉 카메라의 깊이를 의미한다.
- (0.5f, 0.5f, 0.0f)를 넘겨준다는 것은 <u>카메라 화면의 정중앙 + 화면과 카메라 오브젝트는 위치를 일치하게</u> 라는 듯이다.

#### `ViewportPointToRay(Vecotr3)`
- 카메라가 찍고 있는 화면(뷰포트)상의 위치를 Vector3로 넘기면 그 화면 상 위치를 가로지르는 `Ray`를 리턴한다. 
- position.z 는 무시 된다.

#### `WorldToScreenPoint(Vector3)`
- 월드 좌표계 위치(Vector3)를 인수로 받아서 이를 <u>카메라 화면 상의 위치로 변환하여 Vector2 를 리턴한다.</u>

<br>

## 👩‍🦰 Rigidbody

> 오브젝트에 <u>현실적이고 사실적인 물리 기능</u>을 추가해준다.
 
- <u>오브젝트가 중력의 영향을 받게 됨</u>
  - `Use Gravity` 체크 해제하면 중력 영향 안받게 됨
- 중력 말고도 질량, 마찰력 등등 현실 세계에 존재하는 물리적인 힘들 설정 가능.
- <u>Collider에 물리학을 입힌다.</u>

### 변수/프로퍼티

- `velocity`
  - 속도 값을 나타내는 변수.
    - 속도를 주면 그 속도에 맞춰 매 프레임마다 이 컴포넌트가 붙어있는 오브젝트가 움직이게 된다.
  - `Vector3`타입.

### 함수

#### `Addforce(x, y, z)`
  - 오브젝트가 x, y, z 축 방향으로 물리적인 힘을 받도록 한다. 
  - 힘의 정도를 매개변수로 받아서 관성, 마찰력, 중력 등등 여러가지를 고려해서 내부적으로 계산한 힘을 처리한다.

#### `AddExplosionForce(float, Vector3, float)`
  - 매개변수로 넘겨받은 정보를 가지고 폭발에 대한 물리처리를 해준다.
  - 첫번째 매개변수 : 폭발력
  - 두번째 매개변수 : 폭발 지점
  - 세번째 매개변수 : 폭발 반경 반지름

#### MovePosition(Vector3)

- 인수로 들어온 그 위치로 이동한다.

#### MoveRotation(Quaternion)

- 인수로 들어온 그 회전값이 되게끔 회전한다.

<br>

## 👩‍🦰 Character Controller

> `Rigidbody`를 사용하지 않고, 즉 현실적인 물리시스템을 사용하지 않고 움직임의 대부분을 개발자의 코드에 의해서 통제하게끔 만드는 컴포넌트다.

- `Collider`를 확장한 컴포넌트.

### 변수/프로퍼티

  - `isGrounded` 
    - bool 타입 변수로 캐릭터가 땅에 닿고 있으면 True를 리턴한다.

<br>

## 👩‍🦰 Shadow
> 오브젝트에게 그림자 효과를 준다.

<br>

## 👩‍🦰 Outline
> 오브젝트의 테두리를 그려준다.

<br>

## 👩‍🦰 Line Renderer
주어진 점들을 이은 선을 그리는 역할을 한다.

### 변수/프로퍼티

- 프로퍼티
  - `positionCount`
    - <u>선에 위치한 정점의 수</u>를 Set 하고 Get 할 수 있는 프로퍼티. 
- 함수
  - `SetPosition`
    - 인수 👉 정점 인덱스, 위치로 사용할 Vector3 
    - 선분 상의 정점의 위치를 지정하는 함수

<br>

## 👩‍🦰 Audio Source

> wav 파일이나 mp3파일을 재생시키는 컴포넌트. 마치 카세트 같은 것. 테이프로 쓸 오디오 클립(mp3 파일)만 넣어주면 된다. 

### 옵션
- 옵션 
  - `Play On Awake` : 게임 시작하자 마자 재생할건지
  - `Loop` : 음악이 끝나도 다시 반복할건지
  - 볼륨 크기 설정도 가능

### 함수

#### `Play()`

- 클립을 재생. 단, 인수를 필요로 하지 않으므로 미리 `clip`에 재생시킬 클립이 할당 되어 있어야 한다.
- 소리를 중첩시키지 않기 때문에 이미 재생 중이었던 소리는 중지 시킨후 자신의 클립을 재생시킨다.

#### `PlayOneShot(clip)`

- 클립을 1회 재생. 단 Play()와는 다르게 재생시킬 `clip`을 인수로 받는다.
- Play()와는 다르게 소리를 중첩해서 재생할 수 있다. 이미 재생 중이었던 소리를 중지시키지 않은 채로 자신의 클립을 같이 그 위에 재생시킨다.

<br>

## 👩‍🦰 Particle System
> 이 컴포넌트를 붙이면 오브젝트는 파티클 효과를 일으킬 수 있다.

### 변수.프로퍼티

  - `duration` : Particle System 은 스스로의 러닝타임을 알고 있다. 

### 옵션

- `Stop action`
    - 이펙트가 재생이 끝났을때 실행될 처리.
      - `Destroy`를 할당해주면 이펙트 재생이 끝나자마 이펙트 오브젝트가 파괴된다.
      - `Disable`을 할당해주면 이펙트 재생이 끝나자마 이펙트 오브젝트가 비활성화 된다.
      - `Callback` 을 할당해주면 이펙트 재생이 끝나자마자 **OnParticleSystemStop** 이벤트 함수가 실행된다. 즉, **OnParticleSystemStop** 이벤트 함수안에 내가 원하는 처리 구현해놓아 실행시킬 수도 있다.

### 함수

#### `Play()`
- 파티클 이펙트 재생
- 유니티상의 옵션
  
    

<br>

## 👩‍🦰 Horizontal Layout Group
> 이 컴포넌트가 붙은 오브젝트의 자식 오브젝트들을 수평 정렬 시켜준다.

## 👩‍🦰 Vertical Layout Group
> 이 컴포넌트가 붙은 오브젝트의 자식 오브젝트들을 수직 정렬 시켜준다.

## 👩‍🦰 Grid Layout Group
> 이 컴포넌트가 붙은 오브젝트의 자식 오브젝트들을 바둑판처럼 하나하나를 cell로 하여 정렬 시켜준다.

## 👩‍🦰 Layout Element 컴포넌트
> 이 컴포넌트가 붙은 오브젝트의 자식 오브젝트들의 최소 크기, 최대 크기 등등을 지정해준다. 

- 즉, 우선적으로 설정한 이 사항들을 최우선적으로 지켜주면서 정렬되도록 해준다.

<br>

## 👩‍🦰 Animator

> 애니메이션 컴포넌트

### 변수/프로퍼티

- `applyRootMotion`
  - `true`로 해주면 루트 모션을 켜준다.

### 함수

#### 파라미터 관련 
  - `SetFloat("파라미터 이름", float값)` : 해당 파라미터에 인수로 넘긴 float 값을 준다. 그러면 이 조건에 맞는 애니메이션 트랜지션이 알아서 발동한다.
    - `SetFloat("파라미터 이름", 값, 지연 시간, 시간간격)`
      - ex) **SetFloat("Horizontal Move", moveInput.x * animationSpeedPercent, 0.05f, Time.deltaTime);**
        - 이전에 설정한 파라미터 값에서 방금 설정한 파라미터 값으로 0.05초의 지연시간을 거쳐 이 `Time.deltaTime` 시간 간격동안 부드럽게 변경해주도록 한다.
    - `SetBool`, `SetInt`도 사용법 마찬가지
  - `SetTrigger("파라미터 이름")` : Trigger 타입인 해당 파라미터를 발동시킨다. 그러면 이 Trigger를 가진 애니메이션 트랜지션이 알아서 발동한다.

#### IK 관련

  - 관절 IK
    - `SetIKPositionWeight(AvatarIKGold, float)`
      - 관절 꺾을 대상과 함께 Position면에 있어 IK의 우선순위를 결정한다.
      - float 값이 1.0에 가까울수록 IK가 더 강하게 적용됨
    - `SetIKRotationnWeight(AvatarIKGold, float)`
      - 관절 꺾을 대상과 함께 Rotation면에 있어 IK의 우선순위를 결정한다.
      - float 값이 1.0에 가까울수록 IK가 더 강하게 적용됨
    - `SetLookAtWeight(float)`
      - 시선 처리
      - float 값이 1.0에 가까울수록 IK가 더 강하게 적용됨
    - `SetIKPosition(AvatarIKGold, Vector)`
      - Position면에 있어 관절 꺾을 대상과 중심이 되는 위치 
      - 해당 위치를 중심으로 해당 아바타의 관절을 꺾어 이동시킴
    - `SetIKRotation(AvatarIKGold, Quaternion)`
      - Rotation면에 있어 관절 꺾을 대상과 중심이 되는 위치 
      - 해당 위치를 중심으로 해당 아바타의 관절을 꺾어 회전시킴
    - `SetLookAtPosition(Vector)`
      - **시선**을 해당 타겟의 위치로 이동시킨다.
    
<br>

## 👩‍🦰 Mask

> 이미지 UI의 자식 요소들 중 이미지 영역을 튀어나온 부분은 잘라준다.
- 주로 이미지 UI에 붙으며 이미지 컴포넌트가 없다면 동작하지 않는다.

- [참고 포스트](https://ansohxxn.github.io/unity%20lesson%201/chapter10-2/)

<br>

## 👩‍🦰 Rect 2D Mask

> 이미지말고 그냥 이미지를 기반으로 한 사각형으로부터 튀어나온 자식 요소들을 잘라낸다.

- 그냥 Mask 컴포넌트와 다르게 <u>이미지 컴포넌트가 없어도, 즉 그래픽이 없어도 동작한다.</u>
- [참고 포스트](https://ansohxxn.github.io/unity%20lesson%201/chapter10-2/)

<br>

## 👩‍🦰 Toggle Group

> 해당 오브젝트를 토글 그룹으로 설정한다.

  - 이 오브젝트에 토글 UI들을 자식으로 붙인 후 이 오브젝트를 토글 그룹 컴포넌트를 붙여 토글 그룹으로 설정하는 식으로 구현한다.
  - <u>토글 중 하나만 선택되게</u> 하며 <u>토글 하나가 선택되면 다른 토글들은 해제되도록 해준다.</u>
  - [참고 포스트](https://ansohxxn.github.io/unity%20lesson%201/chapter10-3/)

<br>

## 👩‍🦰 Nav Mesh Agent

> `AI`로 이 컴포넌트를 오브젝트에 붙이면 해당 오브젝트가 목표까지 최단 거리를 계산해 추적하는 역할을 하며 충돌을 회피하는 기능을 제공한다. 

- 지나갈 수 있는 영역과 없는 영역을 미리 구워 놔서 `NavMesh` 데이터들을 만들어 놔야 한다. 

### 변수/프로퍼티

  - `radius`
    - Agent의 반경
    - 이 반경을  Agent들간의 충돌을 계산할 수도 있고 장애물을 어떻게 돌아갈 것인지에 계산될 수 있다.
  - `stoppingDistance`
    - Agent가 목표를 추적하다가 목표 위치에 가까워졌을시 서서히 정지하는 근접 거리.
  - `speed`
    - Agent의 최대 이동 속도
  - `remainingDistance`
    - 현재 경로에서 목표 지점까지 남아있는 거리
  - `desiredVelocity`
    - NavMeshAgent의 `desiredVelocity`은 음 목적지로 향하는 목표 속도를 나타낸다. 실제 속도는 아님! 현재 속도로 설정하고 싶은 원하는 속도 값. `desiredVelocity` 속도로 움직이게 하고 싶지만 실제론 관성이나 어떤 장애물에 의해 실제 속도(`velocity`)와는 차이가 날 수 있다.
      - 예를 들어 우리가 원하는 속도가 50인데 현재 캐릭터가 장애물에 막혀 제자리에서 뛰고 있다면 속도는 실제로는 0 이다.
  - `isStopped`
    - True 해주면 Agent는 움직임을 멈추고 정지.
    - False 해주면 멈췄었던 Agent가 다시 움직임.
  - `enabled`
    - false 하면 AI 추적을 중지하고 내비메쉬 컴포넌트를 비활성화
    - true 하면 AI 추적 시작하고 내비메쉬 컴포넌트를 활성화
    - `isStopped`과의 차이
      - <u>Nav Mesh Agent들끼리는 내비게이션 추적에 있어 서로를 장애물로 인식하고 피하고 다니기 때문에</u> 완전히 비활성화 해주지 않고 그냥 추적만 멈추는 `isStopped = true`를 사용해주게 되면 나중에 좀비가 많이 죽었을 때 쓸데없이 크게 돌아와야 하기 때문에 이런 경우에는 아예 `enabled = false`를 통해 완전히 비활성화 해주는 것이 좋다.

### 함수

#### `SetDestination(Vector3 target)` 
    - 목표 위치를 인수로 넘기면 agent가 해당 목표 지점까지 움직이게 하는 함수.


***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}