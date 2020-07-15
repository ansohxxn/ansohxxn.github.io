---
title:  "Unity C# > 컴포넌트, 함수, 변수, UnityEngine, 패키지 등등 총 정리" 

categories:
  -  GameEngine
tags:
  - [Game Engine, Unity]

toc: true
toc_sticky: true

date: 2020-06-13
last_modified_at: 2020-06-13
---

<u>유니티에서 제공하는</u> **컴포넌트**, **함수**, **변수** 등등을 **총 정리**하는 포스트입니다!  
공부하거나 프로젝트 하면서 알게 된 것들을 차근 차근 추가하여 정리해 나갈 거에요. 😀
{: .notice--warning}

***

> 유니티 공식 매뉴얼 <https://docs.unity3d.com/kr/current/Manual/UnityManual.html>

> Scripting Overview <http://www.devkorea.co.kr/reference/Documentation/ScriptReference/index.html>

## 👩‍🦰 컴포넌트
- `gameObject` : 자기 자신(컴포넌트 or 스크립트)가 붙은 오브젝트를 뜻함. 그러니 자기 자신이라고 해석해도 괜춘!

### Transform
- 오브젝트를 생성하면 기본으로 붙어있는 디폴트 컴포넌트다.
  - `Transform myTransform`하고 변수 선언을 해준 후 `myTransform.Rotate()` 이런식으로 쓰는 일반적인 방법도 있긴 하지만 Transform 컴포넌트는 모든 오브젝트들이 디폴트로 갖고 있는 컴포넌트기 때문에 그냥 변수 선언 없이 `transform` 소문자 transform으로 바로 사용하는게 가능하다. `transform.Rotate()` 이렇게.
- 오브젝트의 `position`, `rotation`, `scale` 즉 위치, 회전, 크기를 담당한다. 
  - `localPosition`, `localRotation`, `localScale`은 Local좌표계에서의 위치. 
- 톱니 바퀴 버튼을 누른 후 Reset을 누르면 오브젝트 위치가 원점으로 돌아간다.
- ✨ <u>부모 자식 관계</u> 또한 Transform이 관리한다.
  - `transform.parent` : 내 부모 오브젝트를 뜻한다.


#### `Translate(Vector3)`
- 매개변수로 들어온 벡터값 만큼 평행이동 시킨다.
- `Translate(Vector3), Space.World)`
  - Translate 함수 매개변수로 Space.World를 넘기면 Global 좌표계를 기준으로 평행이동한다.
  - 매개변수 디폴트 값은 Space.self (Local)
    - Translate, Rotate함수는 `Local`인 반면 그냥 바로 변수로 접근하는 `position`, `rotation`같은 것들은 `Global`좌표계 기준이다.
      - 다만 부모가 있는 자식 오브젝트라면 `Local`임

  
#### `Rotate(Vector3)`
- <u>현재 회전에서</u> 매개변수로 들어온 Vector3만큼 <u>더</u> x, y, z 방향으로 각각 a, b, c도 만큼 <u>회전</u>을 시킨다.
- Translate과 마찬가지로 Space.Self가 디폴트라 로컬 좌표계를 기준으로 회전한다.

<br>

### Collider
- 3D 오브젝트를 생성하면 기본으로 붙어있는 디폴트 컴포넌트다. (빈 오브젝트에선 디폴트가 아님)
- <u>물리적인 표면</u>을 가지게 됨
- 구 오브젝트에 Rigidbody를 붙여주고 플레이를 누르면 중력을 받아 밑으로 떨어지면서 <u>Plane에 부딪쳐 더 이상 떨어지지 않는데 이는 Collider 컴포넌트 때문이다</u>
- <u>물리적으로 오브젝트끼리 표면에 충돌이 일어났을 때를 감지</u>하고 이를 처리하는 컴포넌트다.
- `Is Trigger`
  - 체크 해주면 ✨<u>물리적인 표면은 없어지지만 충돌은 감지해준다</u>✨
    - 즉 충돌이 일어나면 그 충돌은 감지하지만 물리적인 표면은 없어지기 때문에 뚫고 지나갈 수 있다. 충돌 처리가 없어져서.

<br>

### Mesh Renderer
- 오브젝트에게 그림을 그려주는 컴포넌트
- 마치 물감 같은 컴포넌트
- **<u>Material</u>**을 다룬다.

<br>

### Camera
- 카메라 컴포넌트
- *변수*
  - `Camera.main`
    - "MainCamera" 태그가 붙은 현재 활성화 되어 있는 카메라.
- *함수*
  - `ViewportToWorldPoint(Vector3)`
    - 카메라 오브젝트에 내장되어 있는 함수로서 카메라가 찍고 있는 화면(뷰포트)상의 위치를 Vector3로 넘기면 그 위치를 실제 게임 월드의 위치로 리턴해준다.
      - 카메라가 찍고 있는 화면은 (x, y) 2D 좌표를 사용하며 화면의 좌측 하단은 (0,0)이며 화면의 우측 상단은 (1,1)이다. 
      - z 값은 카메라 오브젝트로부터 실제 카메라가 찍고 있는 화면까지의 실제 게임 월드 거리, 즉 카메라의 깊이를 의미한다.
      - (0.5f, 0.5f, 0.0f)를 넘겨준다는 것은 <u>카메라 화면의 정중앙 + 화면과 카메라 오브젝트는 위치를 일치하게</u> 라는 듯이다.

<br>

### Rigidbody
- 오브젝트에 <u>현실적이고 사실적인 물리 기능</u>을 추가해준다.
  - <u>오브젝트가 중력의 영향을 받게 됨</u>
    - `Use Gravity` 체크 해제하면 중력 영향 안받게 됨
  - 중력 말고도 질량, 마찰력 등등 현실 세계에 존재하는 물리적인 힘들 설정 가능.
- `Addforce(x, y, z)`
  - ***함수***다.
  - 오브젝트가 x, y, z 축 방향으로 물리적인 힘을 받도록 한다. 
  - 힘의 정도를 매개변수로 받아서 관성, 마찰력, 중력 등등 여러가지를 고려해서 내부적으로 계산한 힘을 처리한다.
- `AddExplosionForce(float, Vector3, float)`
  - 매개변수로 넘겨받은 정보를 가지고 폭발에 대한 물리처리를 해준다.
  - 첫번째 매개변수 : 폭발력
  - 두번째 매개변수 : 폭발 지점
  - 세번째 매개변수 : 폭발 반경 반지름
- `velocity`
  - ***변수***다.
  - 속도 값을 나타내는 변수.
    - 속도를 주면 그 속도에 맞춰 매 프레임마다 이 컴포넌트가 붙어있는 오브젝트가 움직이게 된다.
  - `Vector3`타입.

<br>

### Character Controller
- `Rigidbody`를 사용하지 않고, 즉 현실적인 물리시스템을 사용하지 않고 움직임의 대부분을 개발자의 코드에 의해서 통제하게끔 만드는 컴포넌트다.
- `Collider`를 확장한 컴포넌트.

<br>

### Shadow
오브젝트에게 그림자 효과를 준다.

<br>

### Outline
오브젝트의 테두리를 그려준다.

<br>

### Audio Source
mp3파일을 재생시키는 컴포넌트. 마치 카세트 같은 것. 테이프로 쓸 오디오 클립(mp3 파일)만 넣어주면 된다. 
- 옵션 
  - `Play On Awake` : 게임 시작하자 마자 재생할건지
  - `Loop` : 음악이 끝나도 다시 반복할건지
  - 볼륨 크기 설정도 가능

<br>

### Particle System
이 컴포넌트를 붙이면 오브젝트는 파티클 효과를 일으킬 수 있다.
- 변수
  - `duration` : Particle System 은 스스로의 러닝타임을 알고 있다. 

<br>

### Horizontal Layout Group
이 컴포넌트가 붙은 오브젝트의 자식 오브젝트들을 수평 정렬 시켜준다.

### Vertical Layout Group
이 컴포넌트가 붙은 오브젝트의 자식 오브젝트들을 수직 정렬 시켜준다.

### Grid Layout Group
이 컴포넌트가 붙은 오브젝트의 자식 오브젝트들을 바둑판처럼 하나하나를 cell로 하여 정렬 시켜준다.

### Layout Element 컴포넌트
이 컴포넌트가 붙은 오브젝트의 자식 오브젝트들의 최소 크기, 최대 크기 등등을 지정해준다. 즉, 우선적으로 설정한 이 사항들을 최우선적으로 지켜주면서 정렬되도록 해준다.

<br>

### Animator
애니메이션 컴포넌트
- 함수
  - 파라미터 
    - `SetFloat("파라미터 이름", float값)` : 해당 파라미터에 인수로 넘긴 float 값을 준다. 그러면 이 조건에 맞는 애니메이션 트랜지션이 알아서 발동한다.
      - `SetBool`, `SetInt`도 사용법 마찬가지
    - `SetTrigger("파라미터 이름")` : Trigger 타입인 해당 파라미터를 발동시킨다. 그러면 이 Trigger를 가진 애니메이션 트랜지션이 알아서 발동한다.
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

### Mask
- 이미지 UI의 자식 요소들 중 이미지 영역을 튀어나온 부분은 잘라준다.
- 주로 이미지 UI에 붙으며 이미지 컴포넌트가 없다면 동작하지 않는다.
- [참고 포스트](https://ansohxxn.github.io/unity%20lesson%201/chapter10-2/)

<br>

### Rect 2D Mask

- 이미지말고 그냥 이미지를 기반으로 한 사각형으로부터 튀어나온 자식 요소들을 잘라낸다.
- 그냥 Mask 컴포넌트와 다르게 <u>이미지 컴포넌트가 없어도, 즉 그래픽이 없어도 동작한다.</u>
- [참고 포스트](https://ansohxxn.github.io/unity%20lesson%201/chapter10-2/)

<br>

### Toggle Group

- 해당 오브젝트를 토글 그룹으로 설정한다.
  - 이 오브젝트에 토글 UI들을 자식으로 붙인 후 이 오브젝트를 토글 그룹 컴포넌트를 붙여 토글 그룹으로 설정하는 식으로 구현한다.
  - <u>토글 중 하나만 선택되게</u> 하며 <u>토글 하나가 선택되면 다른 토글들은 해제되도록 해준다.</u>
  - [참고 포스트](https://ansohxxn.github.io/unity%20lesson%201/chapter10-3/)

<br>
<br>

## 👩‍🦰 UnityEngine
C# 스크립트에서 `UnityEngine`이 제공하는 것들 정리. `using UnityEngine`을 해주어야만 사용할 수 있다.

<br>

### Vector2, Vector3
1. 위치 좌표
2. 벡터
- `magnitude` : 벡터의 길이 및 크기. float
- `sqrMagnitude` : 벡터의 길이 제곱. float
- `normalized` : 해당 벡터의 방향 벡터. (길이 1)
- `forward` : 해당 벡터의 ***앞쪽*** 을 나타내는 <u>방향 벡터</u> (길이가 1인)
- `SmoothDamp(Vector, Vector, ref Vector3, float)` 함수
  - 매개변수 첫번째 벡터가
  - 매개변수 두번째 벡터로 되기까지
  - 네번째 매개변수인 시간(float)동안 스무스하게 변화하는 값을 리턴한다.
  - 세번째 매개변수는 Call by reference인 ref 참조 변수로서 직전, 마지막 프레임에서의 속도를 나타낸다. 함수 안에서 계산되어 바깥으로 꺼내짐.
- `zero` : 원점. 
  - `Vector2.zero` 👉 (0, 0)
  - `Vector3.zero` 👉 (0, 0, 0)

<br>

### Debug
- 개발자가 쉽게 디버깅하기 위해 포함된 방법의 클래스
- *함수*
  - `Debug.Log` : 콘솔 출력
  - `Debug.DrawRay` : 레이캐스트 광선을 그려주어 개발자가 시각적으로 볼 수 있게끔 해준다.

<br>

### Mathf
- 수학과 관련된 함수들이 미리 들어있는 함수 집합
  - Mathf(a, b) : a, b 둘 중 더 큰 것을 리턴한다.
  - sqrt(a) : 루트 a 
  - `SmoothDamp(float, float, ref float, float)` 함수
    - 매개변수 첫번째 값이
    - 매개변수 두번째 값으로 되기까지
    - 네번째 매개변수인 시간(float)동안 스무스하게 변화하는 값을 리턴한다.
    - 세번째 매개변수는 Call by reference인 ref 참조 변수로서 직전, 마지막 프레임에서의 속도를 나타낸다. 함수 안에서 계산되어 바깥으로 꺼내짐.

<br>

### Physics
물리와 관련된 함수들이 미리 들어있는 함수 집합
- *함수*
  - `Physics.OverlapSphere(Vector3 pos, float radius)`
    - 구의 중심 위치(pos)와 반지름을 매개변수로 넘겨주면 구의 <u>반경 내에 있는 모든 * Collider * </u> 들을 <u>Collider 배열로 리턴</u>한다.
    - 추가 매개변수
      - layer도 같이 넘겨주면 해당 layer에 해당하는 Collider들만 들어있는 배열을 리턴한다. 
  - `Physics.Raycast`
    - 레이캐스트. 눈에 보이지 않는 광선을 쏴서 물리적 충돌을 감지한다.
    - `Physics.Raycast(발사위치 Vector3, 발사방향 Vector3, 광선길이 float, 감지할 레이어 Layer)`
      - 발사 위치와 발사 방향은 필수 매개변수다.
    - `Physics.Raycast(광선 Ray, 충돌 정보 RaycastHit, 광선길이 float, 감지할 레이어 Layer)`
      - `Ray`는 광선의 바랏 위치와 발사 방향을 담고 있는 광선이다.
      - 광선과 충돌한 Collider의 정보를 담는 역할을 하는 `RaycastHit` 타입의 변수도 매개변수로 넘겨준다. 이때 `out`을 함께 써서 바로 적용될 수 있게 해주기.
        - *if (Physics.Raycast(ray, `out hit`, distance, whatIsTarget))*
          - 광선에 충돌이 감지되는 순간, 이 if 조건문이 실행되자마자 RaycastHit 타입의 hit 변수에 충돌한 Collider의 정보가 들어간다. 

<br>

### Random
난수 생성과 관련된 함수들이 있는 집합
- *함수*
  - Random.`Range(int min, int max)`;
    - [min, max) 범위내에서 <u>int 타입의 랜덤한 정수</u>를 리턴한다.
      - max는 포함되지 않는다. 
  - Random.`ColorHSV()`
    - 랜덤한 컬러를 리턴한다.

<br>

### Input
- UnityEngine에서 제공하는 <u>입력</u>과 관련된 집합.
- 키보드, 모바일, 조이스틱의 입력을 받을 수 있는 여러 함수들의 집합.

- `Input.GetKey(int)`
  - 뫄뫄 키보드 입력이 들어오면 True를 리턴한다. 
  - 뫄뫄 키보드 입력이 들어오지 않는 상태면 False를 리턴한다.
  - *ex) Input.GetKey(KeyCode.W) : W키가 입력되면 True 리턴*
  - `KeyCode`
    - 키보드의 각 자판들은 정수와 매칭된다.
      - 그러나 자판에 매칭되는 정수를 모두 외우기는 힘듬. 119는 W.. 이런식으로 외울 수가 없음.
      - KeyCode 클래스에 각 키보드 자판들과 정수가 연결되어 있으니 예를 들어 `KeyCode.W` 이런식으로 갖다 쓰기만 하면 된다. 
  - `GetKey` : 키를 누르는 동안
  - `GetKeyUp` : 키를 떼는 순간
  - `GetKeyDown` : 키를 누르는 순간 
    - `GetMouseButtonDown(0)`
      - GetMouseButton<u>Down</u> : 마우스를 누르는 순간
      - `0` : 마우스 좌클
      - `1` : 마우스 우클
  - `Input.GetAxis("Horizontal");`
    - `GetKey`와 다르게 <u>문자열을 매개변수로 받으며</u> <u>float 값을 리턴한다.</u>
      - ex) "Horizontal"
      - <u>수평 방향</u>에 대해서 키보드나 조이스틱으로 입력을 했을때 <u>-1 ~ +1</u> 사이의 <u>float 값을 리턴한다.</u>
        - 즉 키보드 수평방향에 대응되는 키들이 "Horizontal"에 맵핑되어 있다.
        - A와 D가 맵핑 되어 있다.
      - "Fire", "Jump", "Crunch" 등등..
    - `GetKey`보다 좋은 점
      - 만약 점프 키를 Space bar가 아닌 윗쪽 방향키로 바꾸고 싶다면?
        - `GetKey`의 경우 직접 코드로 찾아가서 GetKey(KeyCode.SpaceBar)를 Getkey(KeyCode.Up)으로 바꾸는 수고를 감수해야 한다.
          - 그런데 `GetAxix`를 사용한다면 점프 동작에 대한 <u>유니티 상에서 "Jump"에 Space bar가 할당 되있는 것을 그냥 Up으로 바꿔주면 땡이다.
            - 직접 GetKey처럼 코드 찾아가서 `Input.GetAxis("Jump")` <u>코드를 수정하지 않아도 되는 것이다!</u>
            - 그냥 유니티 Project Settings 에서 바꾸기만 하면 된다.
    - 문자열에 맵핑된거 바꾸는 방법
      - 유니티 - 메뉴 - Edis - Project Settings - Input 에서 바꿀 수 있다.
      - 현재 Horizontal은 왼쪽 방향키(left), 오른쪽 방향키(right), 그리고 보조로 A키, D키가 설정 되어있다.
        - Negative : `left`, `A`
        - Positive : `Right`, `D`
    - float 값을 리턴하는 이유 : 조이스틱 때문
      - `Negative` 키는 `-1.0f`  *ex) "Horizontal"에서 A키*
      - `Positive` 키는 `1.0f`   *ex) "Horizontal"에서 D키*
      - 아무것도 안누를 땐 `0.0f`
      - 이렇게 -1 ~ 1 사이의 float 실수값을 반환하는데 이렇게 하는 이유는 <u>조이스틱 입력 때문!</u>
        - 조이스틱은 키보드와 다르게 딱 눌렀다 뗏다 같이 이분법적으로 생각할 수가 없기 때문.
        - 조이스틱은 아주 살짝만 밀거나 많이 밀거나 미는 정도에 따라 입력의 정도가 다른 것! 
          - 오른쪽으로 살짝 밀면 0.2 이런게 가능

<br>

### Time
시간과 관련된 함수나 변수의 집합

- `Time.deltaTime`
  - 변수.
  - 우리집 컴퓨터가 60프레임이면 `1/60`.
  - update같이 매 프레임마다 실행되는 함수 안에서 시간 간격을 고려하여 곱해준다.
    - 1초에 3m 가게 움직이고 싶은데 <u>1초에 60번 깜빡이는 60프레임 컴퓨터</u>라면 update함수 내에서 `3m * Time.deltaTime` 해주면 된다.
    - 매 프레임마다 `3m * Time.deltaTime`씩 움직여 최종적으로 1초에 3m 움직이게 되는 것.

<br>

### Color
색깔들이 미리 이름 붙어 구현되어 있다. `Color.red`, `Color.yellow` 이런 식으로 되있어서 그냥 갖다 쓰기만 하면 됨.

<br>

### Quaternion
쿼터니언과 관련된 함수들 집합. `3차원 회전`을 위한 함수.

- `Quaternion.Euler(Vector3)`
  - 매개변수로 받은 Vector3를 Quaternion타입으로 변환하여 리턴해주는 함수
- `Quaternion.LookRotation(Vector3)`
  - 벡터를 매개변수로 넣어주면 오브젝트가 그 벡터의 방향을 쳐다보게끔 자기 자신의 방향을 회전한다.
  - 현재 위치좌표에서 매개변수로 들어온 <u>벡터만큼 더한 목적지 위치 좌표를 바라보게</u> 된다. 
    - 따라서 `벡터의 뺄셈`으로 `목적지 위치좌표 - 출발지 위치좌표` 해주어 필요한 방향과 거리를 나타낼 벡터를 구해주는 것이 좋다. 그리고 매개변수로 넘기기.
- `Quaternion.Lerp(Vector3, Vector3, float)`
  - 두 개의 회전 값(Vector3)을 정하면 float 비율만큼의 적당한 회전값을 리턴
- `Quaternion.Lerp(aRotation, bRotation, 0.5f);`
  - `0.5f`면 딱 중간
  - `1.0f`면 bRotation을 그대로 따름
  - `0.0f`면 aRotation을 그대로 따름
  - `0.2f`면 aRotation에 좀 더 가깝게 회전
- `eulerAngles`
  - 변수다. `Quaternion.eulerAngles`이렇게 쓰는게 아니라 `Quaternion타입을 참조하는 변수.eulerAngles` 이렇게 쓴다.
  - 쿼터니언을 오일러각으로 변환시킨다. 즉 Vector3로 변환한다. 
    - Quaternion.Euler(Vector3)와 반대.
- `identity`
  - 변수다. 
  - 0도, 0도, 0도로 회전한 쿼터니언 값.

<br>

### UnityEngine.SceneManagement
`using UnityEngine.SceneManagement`을 해주어야만 사용할 수 있다. 

- scene과 scene을 넘나 드는 작업을 하고 싶을 때 사용.
- `SceneManagement`를 사용할 수 있다.
- ***함수***
  - `SceneManager.LoadScene("씬 이름" 혹은 씬 인덱스), 모드)`;
    - 해당 씬을 로드한다.
    - 첫번째 인수로 씬의 이름 문자열이나 씬의 인덱스를 넘긴다.
    - 모드는 `LoadSceneMode.Single`, `LoadSceneMode.Additive` 이렇게 2가지 있는 Single이 디폴트 값이다. 따라서 인수 한개만 넘기면 싱글 모드로 씬을 로드한다.
      - `LoadSceneMode.Single` : 현재 씬의 오브젝트들을 모두 Destroy하고 새롭게 씬을 로드
      - `LoadSceneMode.Additive` : 현재 씬에 새로운씬을 추가적으로 덧대어 로드. 말풍선을 덧붙이는 느낌.
  - `SceneManager.GetActiveScene()`
    - 현재 활성화 되있는 씬을 리턴한다.
    - SceneManager.GetActiveScene().`buildIndex` : 현재 활성화 되있는 씬의 인덱스 리턴

<br>

### PlayerPrefs
유니티에서 지원해주는 데이터 자료구조 타입이다. 문자열인 Key 값과 그에 따른 value를 묶어 로컬 파일로서 저장이 된다.(<u>로컬 파일로서 저장이 되기 때문에 껏다 켜도 그 값이 유지가 된다</u>) 이를 다루는 여러 함수들도 지원한다. Key 값만 알고 있다면 value를 찾을 수 있다. PlayerPref는 구조가 간단해서 해킹당하기 쉽다.
- PlayerPrefs.SetInt("BestScore", score);
  - "BestScore"라는 Key값에 int타입인 score를 파일로서 묶어 저장한다. 
- PlayerPrefs.GetInt("BestScore")
  - "BestScore"라는 Key값의 int 타입인 value값을 리턴한다.
  - "BestScore"라는 Key값이 없더라도 에러는 발생하지 않는다. Key값이 없으면 0을 리턴함.

<br>

### Ray
레이캐스트의 광선 그 자체. 발사 위치와 발사 방향을 담고 있는 구조체다.

<br>

### RaycastHit
- `Raycast` 레이캐스트로부터 정보를 얻기 위한 타입으로 <u>광선에 감지된 Collider의 정보를 담는 컨테이너</u>다.
- *변수*  RaycastHit hit 일때
  - hit.normal : 광선에 감지된 Collider의 노말 벡터 (충돌 표면의 방향벡터)
  - hit.distance : 광선 발사 위치로부터 광선에 감지된 Collider까지의 거리
  - hit.point : 광선에 감지된 Collider의 충돌 위치벡터
  - hit.collider : 광선에 감지된 Collider


<br>
<br>

## 👩‍🦰 이벤트 함수
- 실수로 이름 오타나면 절대 실행 안된다!! 
  - 오타여도 사용자 지정 함수인 줄 알고 잡아주지도 않음 ㅠ ㅠ OnTriggerEnter 인데 onTriggerEnter 라고 소문자로 써서 계속 실행되지 않았었는데 한참을 헤맸다...
- 이름 실수 하지 않도록 주의 하자!!

### void Start()
- <u>컴포넌트 초기화</u> 부분
  - 게임이 처음 활성화 되는 순간에 유니티가 Start 메세지를 브로드캐스팅 하여 뿌려 온 컴포넌트들을 각자 구현된 Start 내용대로 초기화 시킨다.
  - 유니티는 게임 시작할때 Start 메세지를 뿌린다.
- Awake() 후 + Update() 전에 1회 동작하는 이벤트 함수다.

### void Awake()
- Start()와 비슷한데 Start()보다도 먼저 실행되는 시작 함수다.
  - Start 함수의 이전 및 프리팹의 인스턴스화 직후에 호출
- 생성 하자마자 들어가는 1회 동작 함수
- Start 메세지보다 더 빨리 호출되므로 다른 스크립트들의 Start보다 더 빨리 실행되야 하는 내용이 있으면 Awake에 구현하면 된다.

### void Update()
- 1초에 수십번씩 자신의 상태를 갱신하고 주기적으로 계속 실행 
  - 외부에서 직접 찾아 실행할 필요 없음. 
- <u>스스로 매 프레임마다 실행 됨.</u>

### void FixedUpdate()
- Update()처럼 매 프레임 실행된다.
  - Update()와의 차이점
    - Update()는 화면 한번 깜빡일때마다 실행되서 렉걸리거나 환경이 안좋거나 하면 그만큼 Update()도 적게 실행되지만
    - Fixedupdate()는 환경에 상관없이 무조건 실행 횟수를 지킨다. 정해진 수만큼 무조건 실행 함

### void OnTriggerEnter(Collider other)
- On<u>Trigger</u>Enter : `Trigger`인 Collider와 충돌할 때 자동으로 실행된다. 
  - `Is Trigger`가 <u>체크된 Collider와 충돌하는 경우 발생되는 메세지</u>
  - 사실 충돌하는 두 오브젝트 중 하나만 Trigger 라도 두 오브젝트 모두 이 함수가 실행된다.
    - 즉 물리적 충돌은 일어나지 않고 뚫고 지나가지만 그래도 이벤트 발생은 시켜야 하는 경우.
- <u>오브젝트끼리 충돌하면 유니티에서 OnTriggerEnter 메세지를</u> ***<u>충돌한 오브젝트들</u>*** 에게 브로드캐스팅 한다.
  - 충돌한 두 오브젝트끼리는 서로 독립적이고 연관이 없으니 서로 충돌한 사실을 모르지만 두 오브젝트가 충돌하면 유니티에서 두 오브젝트에게 OnTriggerEnter 메세지를 뿌리므로 두 오브젝트는 <u>OnTriggerEnter 함수 안에 구현한 내용대로 충돌 처리를 한다.</u>
- 유니티는 충돌한 상대 오브젝트의 정보를`Collider`타입의 `other`가 참조하도록 넘겨준다.  
  - 충돌한 <u>상대 오브젝트에 붙어있는 Collider 컴포넌트</u> 
    - `Collider` 컴포넌트 👉🏻 <u>물리적 표면</u>

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

### void OnCollisionEnter(Collision other)
- On<u>Collision</u>Enter : Trigger가 체크되지 않은 <u>일반 Collider를 가진 오브젝트와 충돌한 경우</u> 자동으로 실행된다.
- 매개변수 `Collider`와 `Collision`의 차이
  - `Collision`이 좀 더 충돌에 대한 많은 정보를 담고 있다. 충돌 그 자체에 대한 물리적인 정보. 

<br>

### void OnEnable()
`컴포넌트`가 꺼져 있다가 켜지는 상태마다 발동된다. Start()와 비슷하지만 Start()는 게임 시작시 한번 발동되는 반면 OnEnable()은 컴포넌트가 꺼져있다가 켜질 때마다 1회씩 실행된다. 
- 게임 시작되자마자 `enabled = true` 상태인 컴포넌트들에게 발동
- 게임 도중에`enabled = false`이다가 `enabled = true`가 되는 컴포넌트들에게 발동
  - 오브젝트 끌 때는 `SetActive(false)`
  - 컴포넌트나 스크립트를 끌 때는 `enabled = false`

<br>

### void OnDestroy()
오브젝트가 Destroy 될 때 자동 호출되는 이벤트 함수다. 마치 소멸자 같은 것.

<br>

### void OnAnimatiorIK()

- 애니메이션의 관절 꺾는 IK 정보가 갱신될 때마다 발동되는 이벤트 함수.
- Animator와 관련된 이벤트로 애니메이터가 붙어있는 오브젝트만 이 이벤트 함수를 동작시킬 수 있다.

<br>

### 사용자 지정 이벤트
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

<br>
<br>

## 👩‍🦰 오브젝트 
- 오브젝트를 참조하고 있는 변수로 함수를 호출할 수 있다. *ex) winUI.SetActive(true);*
- 오브젝트들의 내장되어 있는 변수나 함수들.


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

### Instantiate(GameObject)
- 게임 플레이 도중에 매개변수에 들어온 <u>오브젝트를 복사</u>하여 게임 도중에 생성해낸다.
- Instantiate(GameObject, Vector3(position), Vector3(rotation))
  - 위치와 회전 벡터를 추가로 매개변수로 넣어주어 찍어낼 오브젝트의 위치와 회전값을 설정할 수도 있다.
  - 위치, 회전 지정 안해주면 랜덤 위치나 원점에서 생성됨.

### name
변수. 해당 오브젝트의 이름.

### FindObjectOfType<Object>();
- 씬 상의 모든 오브젝트들을 뒤져서 직접 `<>`안에 있는 이름과 일치하는 오브젝트를 찾아서 <u>리스트로</u>리턴해준다.
- 느리다. 이런 성능상에 문제 때문에
  - Awake() 함수 안에서만 구현되거나
  - 아주 적은 횟수로 호출되게끔 해야한다.

<br>

### DontDestroyOnLoad(GameObject) 함수
- 다른 Scene으로 변경되더라도 파괴되지 않고 유지할 오브젝트를 지정하는 함수다.

<br>
<br>


## 👩‍🦰 패키지

### Cinemachine
- 스마트 하게 추적하는 복잡한 카메라를 손쉽게 구현할 수 있도록 해주는 패키지
- *컴포넌트*
  - **Cinemachine Brain**
    - 해당 오브젝트를 Brain Camera로 설정해준다.
    - 여러개의 가상 카메라 중 하나를 선택하여 사용할 수 있는 기능을 가진다.

<br>

## 👩‍🦰 미분류

### LayerMask
`public LayerMask a;` 해주면 유니티에서 <u>Layer 를 선택할 수 있는</u> 드롭다운 슬롯이 열린다.

<br>

### TagMask
`public TagMask a;` 해주면 유니티에서 <u>Tag 를 선택할 수 있는</u> 드롭다운 슬롯이 열린다.

### Invoke()

MonoBehaviour 에서 지원하는 함수로, 함수 혹은 이벤트를 실행시킨다.
- 이벤트이름.Invoke() : 이벤트 발동
- Invoke(string) : 이름을 문자열로 넣으면 그 이름의 함수를 실행시킨다. 
- Invoke(string, floaT) : 매개 변수로 시간도 넣을 수 있다. *Invoke("Restart", 5f)* 해주면 <u>5초 뒤에 Restart() 함수를 실행시켜라</u>라는 의미다.

<br>
<br>

## 👩‍🦰 단축키 or 팁

- `Ctrl`키를 누른 상태에서 오브젝트를 옮기면 `1`단위로 평행이동 시킬 수 있다.
- `Ctrl + D` : 복사 붙여넣기 한번에.
- `shift` : shift 한 후 파일을 두개 선택하면 그 파일들을 포함하여 파일 사이에 있는 파일들까지도 전부 선택된다.
  - `Alt + shift` : 표면상 리스트 뿐만아니라 모든 자식의 자식들까지 다 선택된다. 
- 게임 창의 maximize on play 누르면 게임 실행화면을 큰 화면으로 볼 수 있다.

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}