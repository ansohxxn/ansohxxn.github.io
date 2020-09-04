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


<br>
<br>

## 👩‍🦰 UnityEngine
C# 스크립트에서 `UnityEngine`이 제공하는 것들 정리. `using UnityEngine`을 해주어야만 사용할 수 있다.

<br>

### Vector2, Vector3

> 위치 좌표, 벡터

- ***변수***
  - `magnitude` : 벡터의 길이 및 크기. float
  - `sqrMagnitude` : 벡터의 길이 제곱. float
  - `normalized` : 해당 벡터의 방향 벡터. (길이 1)
  - `forward` : 해당 벡터의 ***앞 쪽*** (z 축) 을 나타내는 <u>방향 벡터</u> (길이가 1인)
  - `right` : 해당 벡터의 ***오른 쪽*** (x 축) 을 나타내는 <u>방향 벡터</u> (길이가 1인)
  - `up` : 해당 벡터의 ***위 쪽*** (y 축) 을 나타내는 <u>방향 벡터</u> (길이가 1인)
  - `zero` : 원점. 
    - `Vector2.zero` 👉 (0, 0)
    - `Vector3.zero` 👉 (0, 0, 0)
- ***함수***
  - `SmoothDamp(Vector, Vector, ref Vector3, float)` 함수
    - 매개변수 첫번째 벡터가
    - 매개변수 두번째 벡터로 되기까지
    - 네번째 매개변수인 시간(float)동안 스무스하게 변화하는 값을 리턴한다.
    - 세번째 매개변수는 Call by reference인 ref 참조 변수로서 직전, 마지막 프레임에서의 속도를 나타낸다. 함수 안에서 계산되어 바깥으로 꺼내짐.
  - `Distance(Vector a, Vector b)`
    - a 와 b 사이의 거리를 리턴한다. float 리턴.
  - `Angle(Vector a, Vector b)`
    - a 와 b 사이의 각도를 리턴한다. float 리턴.


<br>

### Debug
- 개발자가 쉽게 디버깅하기 위해 포함된 방법의 클래스
- *함수*
  - `Debug.Log` : 콘솔 출력
  - `Debug.DrawRay` : 레이캐스트 광선을 그려주어 개발자가 시각적으로 볼 수 있게끔 해준다.
  - `Debug.LogFormat` : 출력 포맷 설정

<br>

### Mathf
- 수학과 관련된 함수들이 미리 들어있는 함수 집합
  - `Max(a, b)` : a, b 둘 중 더 큰 것을 리턴한다.
  - `sqrt(a)` : 루트 a 
  - `Floor(float)` : 내림. 소수점 버림
  - `SmoothDamp(float, float, ref float, float)` 함수
    - 매개변수 첫번째 값이
    - 매개변수 두번째 값으로 되기까지
    - 네번째 매개변수인 시간(float)동안 스무스하게 변화하는 값을 리턴한다.
    - 세번째 매개변수는 Call by reference인 ref 참조 변수로서 직전, 마지막 프레임에서의 속도를 나타낸다. 함수 안에서 계산되어 바깥으로 꺼내짐.
  - `SmoothDampAngle(float, float, ref float, float)`
    - 👉 SmoothDamp 함수와 기능은 동일하나 <u>각도를 고려해서 스무스하게 변경시킨다.</u>
      - 360도 체계에서는 예를들어 -270도와 90도는 같은 회전값임을 의미하니까 사실 -270도면 90도만 돌면 되는건데 일반 SmoothDamp 함수를 사용하면 270도씩 돌아버리니까 SmoothDampAngle을 사용하면 <u>의도와 달리 더 많이 회전하는 경우를 막아 줌</u> !
  - `Clamp(float target, float a, float b);`
    - target 이 a ~ b 범위를 벗어나지 않도록 한다. 

<br>

### Physics
물리와 관련된 함수들이 미리 들어있는 함수 집합
- *변수,상수*
  - `Physics.gravity`
    - 중력 가속도 상수 벡터
    - Physics.gravity.y 는 y 방향의 중력 가속도를 뜻함.
- *함수*
  - `Physics.OverlapSphere(Vector3 pos, float radius)`
    - 구의 중심 위치(pos)와 반지름을 매개변수로 넘겨주면 구의 <u>반경 내에 있는 모든 * Collider * </u> 들을 <u>Collider 배열로 리턴</u>한다.
    - 추가 매개변수
      - layer도 같이 넘겨주면 해당 layer에 해당하는 Collider들만 들어있는 배열을 리턴한다. 
  - `Physics.SphereCastNonAlloc`
    - > 이 함수는 인수로 방향과 거리를 넘겨주면 구가 해당 방향과 거리로 <u>이동한 ✨궤적✨에 겹치는 Collider가 있는지를 검사한다.</u> 이게 바로 <u>Cast계열 함수들의 특성</u>
    - ![image](https://user-images.githubusercontent.com/42318591/92210583-f5544800-eec9-11ea-9da2-403c5f25d02e.png){: width="70%" height="70%"}{: .align-center}
    - 유니티에서 Collider를 찾아내는 방법은 크게 4 가지가 있다. `Ray`, `Box`, `Sphere`, `Capsule` 등등 이런 형태의 Collider 컴포넌트를 오브젝트에 달아서 OnTrigger나 OnCollision 같은 이벤트를 사용하여 해당 형태에 겹치는 Collider를 찾아내기도 한다.
    - 그러나 이런 방법의 경우 매 프레임 실행되는 것이기 때문에 순간적으로 딱 한번만 Collider를 찾아내려고 하는 경우에는 성능상 부적절할 수 있다. 매 프레임 실행되므로 적당히 모양을 유지하는 경우에는 사용하기 괜찮지만 그 순간의 겹치는 Collider를 잡아내야 하는 경우에는 힘들기 때문!   
    - **Physics의 Cast계열 함수** 들은 움직이려는 궤적에 충돌하는지를 검사하기 때문에 위와같이 프레임간 사이에서 빠르게 변화하여 감지되지 못할 수 있는 것들도 감지할 수 있도록 해준다.
      - 종류
      - `Cast` 
        - 👉 찾아낸 충돌체 하나만을 구조체로 반환한다. 가장 처음에 충돌한 물체만 반환한다.
      - `CastAll` 
        - 👉 찾아낸 충돌체 전부를 리턴한다. `RaycastHit []` 배열로 리턴한다.
      - `CastNonAlloc` 
        - 👉 충돌체들을 리턴이 아닌 인수로 넘긴 `RaycastHit` 배열에 담아준다. 따라서 `CastAll`보다는 성능이 더 좋을 수 있다. 다만 찾아낸 충돌체들의 수가 인수로 넘긴 배열의 사이즈보다 적을 수도 많을 수도 있다는 것에 주의하여 사용해야 한다.
        - 직전 프레임까지 어떤 `RaycastHit` 정보가 있었는지를 알 수 있다.
      - 리턴은 `int`로 인수로 넘긴 배열이 채워진 사이즈, 즉 충돌체들의 개수를 리턴한다.
    - <u>움직이려고 하자마자, 즉 궤적을 그리기도 전에 바로 Collider가 걸린 상태라면 첫번째로 감지된 이 Collider의 point는 제로 포인트다.</u>
      - 가상의 구 *(SphereCastNonAlloc을 예로 들자면)* 가 움직이기도 전에 처음부터 겹쳐있었던 것(`hits[0]`)이 있다면 그 Collider의 `point`(충돌 위치)는 제로 포인트가 된다.
        - `hits[0].point`는 (0, 0, 0) 
        - 따라서 이런 경우엔 distance도 0 으로 나오게 된다.
      - `Collider`가 들어있는 것이 아닌 `Raycast`에 걸린 원소들이 들어 있는 `hit` 배열의 크기를 리턴한다.
        ```c#
        var size = Physics.SphereCastNonAlloc(attackRoot.position, attackRadius, direction, hits, deltaDistance, whatIsTarget);
        ```
      - `attackRadius` 반경을 가진 구가 `attackRoot.position`에 위치로부터 `direction` 방향으로 `deltaDistance` 거리만큼 이동하면서 생긴 궤적(연속선상)에 겹치는 Collider들 중에서 `whatIsTarget` LayerMask를 가진 Collider가 있다면 그것을 `hits` 배열에 담고 그 배열의 크기를 리턴한다.
      - `out hits` 혹은 `ref hits` 이렇게 레퍼런스로 넘기지 않은 이유는 `hits` 배열 이름이 그 자체로 레퍼런스가 되기 때문이다. 따라서 그냥 `hits`로 인수 넘기면 됨.  
      - 배열이 아니라 그냥 `RaycastHit` 자체를 넘기는 것이였으면 value이므로 `out hit` 이런식으로 넘겨야 한다.
  - `Physics.Raycast`
    - 레이캐스트. 눈에 보이지 않는 광선을 쏴서 물리적 충돌을 감지한다.
    - `Physics.Raycast(발사위치 Vector3, 발사방향 Vector3, 광선길이 float, 감지할 레이어 Layer)`
      - 발사 위치와 발사 방향은 필수 매개변수다.
    - `Physics.Raycast(광선 Ray, 충돌 정보 RaycastHit, 광선길이 float, 감지할 레이어 Layer)`
      - Boolean 타입을 리턴한다. `Raycast`에 걸린게 있다면 true 리턴.
      - `Ray`는 광선의 바랏 위치와 발사 방향을 담고 있는 광선이다.
      - 광선과 충돌한 Collider의 정보를 담는 역할을 하는 `RaycastHit` 타입의 변수도 매개변수로 넘겨준다. 이때 `out`을 함께 써서 바로 적용될 수 있게 해주기.
        - *if (Physics.Raycast(ray, `out hit`, distance, whatIsTarget))*
          - 광선에 충돌이 감지되는 순간, 이 if 조건문이 실행되자마자 RaycastHit 타입의 hit 변수에 충돌한 Collider의 정보가 들어간다. 
      - 감지할 레이어 마스크인 다섯번째 인수에 `~`을 붙여주면 그 레이어 마스크가 붙은 오브젝트들은 레이 캐스트 처리 하지 말고 무시하라는 뜻.
  - `Physics.Linecast`
    - `Physics.Raycast(발사시작위치 Vector3, Linecast 종료 지점 Vector3, 충돌 정보 RaycastHit, 감지할 레이어 Layer)`
    - 라인캐스트. 눈에 보이지 않는 광선을 쏴서 물리적 충돌을 감지한다.
    - Raycast 와의 차이점 
      - 👉 Linecast는 정확한 종료 지점을 알 고 어떤 범위 내에서의 충돌을 감지하려 할 때, Raycast는 방향만 알 때 사용! 
    - start과 **end** <u>사이의 범위</u>내에서 교차하는 충돌이 있을 경우 true를 리턴
    - 충돌 정보는 `Raycast hit`에 저장된다.
  
      

<br>

### Random
난수 생성과 관련된 함수들이 있는 집합
- *변수/프로퍼티*
  - Random.`insideUnitSphere`
    - 반지름 1 을 갖는 구 안의 랜덤한 위치(Vector3)를 반환하는 프로퍼티. 
      ```c#
      var randomPos = Random.insideUnitSphere * distance + center;  // center를 중점으로 하여 반지름(반경) distance 내에 랜덤한 위치 리턴
      ```
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
- `Time.time`
  - 게임 시작 후 현재까지의 경과 시간(float 초 단위)
- `Time.timeScale`
  - `Time.timeScale`을 통해 시간이 흘러가는 속도를 변경한다면 `Time.fixedDeltaTime`도 그에 맞게 변경해줄 것을 권장한다.
  - `Time.fixedDeltaTime = 0.02f * Time.timeScale`
  - `Time.timeScale`이 2.0f이면 디폴트에 비해 두배로 시간이 빨리 흘러간다는 의미고 0.0f면 시간이 아예 흐르지 않고 정지 상태라는 것을 의미한다.
- `Time.fixedDeltaTime`
  - **FixedUpdate()** 함수가 실행되는 그 사이의 시간. 다음 **FixedUpdate()** 함수가 실행되기까지의 시간. 
  - **FixedUpdate()** 함수는 디폴트로 1/50초인 0.02초를 주기로 실행되기 때문에 디폴트론 `Time.fixedDeltaTime` 값은 `0.02`이다.
  - <u>FixedUpdate() 함수 안에서 사용할 때 Time.fixedDeltaTime가 아닌 그냥 Time.deltaTime 를 사용할 것을 권장한다.</u>
    - 자동으로 `Time.deltaTime`이 **FixedUpdate()** 함수 안에서 쓰이면 알아서 `Time.fixedDeltaTime` 으로 리턴하기 때문이다. 
      - `Time.deltaTime`은 알아서 **Update()** 함수에서는 프레임간의 사잇 시간으로서 동작하고  **FixedUpdate()** 함수 안에서 쓰이면 알아서 고정된 프레임인 `Time.fixedDeltaTime` (0.02초) 으로 쓰인다.

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
- `Quaternion.AngleAxis(float angle, Vector3 axis);`
  - 축 axis 주위를 angle 만큼 회전한 rotation을 생성하고 리턴한다.
    - 예를 들어 중심 축이 되는 `axis`가 y 축이라면 회전 값이 y 값은 변하지않고 x, z 값만 변한다.
      - ![image](https://user-images.githubusercontent.com/42318591/91792755-40175b00-ec51-11ea-84b5-5537a296cdc3.png){: width="70%" height="70%"}{: .align-center}
- `eulerAngles`
  - 변수다. `Quaternion.eulerAngles`이렇게 쓰는게 아니라 `Quaternion타입을 참조하는 변수.eulerAngles` 이렇게 쓴다.
  - 쿼터니언을 오일러각으로 변환시킨다. 즉 Vector3로 변환한다. 
    - Quaternion.Euler(Vector3)와 반대.
- `identity`
  - 변수다. 
  - 0도, 0도, 0도로 회전한 쿼터니언 값.

<br>

### Gizmos

> `OnDrawGizmos()`, `OnDrawGizmosSelected()` 같은 이벤트 함수 내에서 사용되며 개발자의 편의를 위해 Scene상에서만 보여지는 기즈모를 그리는 함수들이 모여이는 클래스다.

- ***변수***
  - `color` 기즈모 컬러
- ***함수***
  - `DrawSphere(Vector3 center, float radius` 기즈모가 될 구를 그린다. 

<br>

### 👩🏻‍🦰 UnityEngine.SceneManagement
`using UnityEngine.SceneManagement`을 해주어야만 사용할 수 있다. 

- scene과 scene을 넘나 드는 작업을 하고 싶을 때 사용.
- `SceneManagement`를 사용할 수 있다.
- ***함수***
  - `SceneManager.LoadScene("씬 이름" 혹은 "씬 인덱스"), 모드)`;
    - 해당 씬을 로드한다.
    - 첫번째 인수로 씬의 이름 문자열이나 씬의 인덱스를 넘긴다.
    - 모드는 `LoadSceneMode.Single`, `LoadSceneMode.Additive` 이렇게 2가지 있는 Single이 디폴트 값이다. 따라서 인수 한개만 넘기면 싱글 모드로 씬을 로드한다.
      - `LoadSceneMode.Single` : 현재 씬의 오브젝트들을 모두 Destroy하고 새롭게 씬을 로드
      - `LoadSceneMode.Additive` : 현재 씬에 새로운씬을 추가적으로 덧대어 로드. 말풍선을 덧붙이는 느낌.
  - `SceneManager.GetActiveScene()`
    - 현재 활성화 되있는 씬을 리턴한다.
    - SceneManager.GetActiveScene().`buildIndex`
      - 현재 활성화 되있는 씬의 인덱스 리턴
    - SceneManager.GetActiveScene().`name`
      - 현재 활성화 되있는 씬의 이름 리턴
  ```c#
  SceneManager.LoadScene(SceneManager.GetActiveScene().name);  // 현재 활성화 되어있는 씬을 재시작
  ```

<br>

### 👩🏻‍🦰 UnityEngine.AI
`using UniyEngine.AI`를 해주어야만 사용할 수 있다.

#### NavMesh

> AI 에이전트가 걸어다닐 수 있는 표면. 네비게이션 경로를 계산할 수 있는 표면이 된다.

- ***함수***
  - `SamplePosition((Vector3 sourcePosition, out NavMeshHit hit, float maxDistance, int areaMask)`
    - `areaMask` 에 해당하는 NavMesh 중에서  `maxDistance` 반경 내에서 `sourcePositio`에 <u>가장 가까운 위치를 찾아서</u> 그 결과를 `hit`에 담음

#### NavMeshHit

> NavMesh 샘플링의 결과를 담을 컨테이너. Raycast hit 과 비슷

  - `hit.normal` : 광선에 감지된 Collider의 노말 벡터 (충돌 표면의 방향벡터)
  - `hit.distance` : 광선 발사 위치로부터 광선에 감지된 Collider까지의 거리
  - `hit.position` : 광선에 감지된 Collider의 충돌 위치벡터

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
  - `hit.normal` : 광선에 감지된 Collider의 노말 벡터 (충돌 표면의 방향벡터)
  - `hit.distance` : 광선 발사 위치로부터 광선에 감지된 Collider까지의 거리
  - `hit.point` : 광선에 감지된 Collider의 충돌 위치벡터
  - `hit.collider` : 광선에 감지된 Collider


<br>
<br>

## 👩‍🦰 UnityEditor
`using UnityEditor` 를 해주어야만 사용이 가능하다. 

### Handles

<u>그림을 그리는 여러가지 함수</u>가 내장되어 있는 클래스다. 3D 기즈모와 같은 것을 그릴 수 있고 GUI도 그릴 수 있다.

- ***변수***
  - `color` 핸들하는 색상
- ***함수***
  - `DrawSolidArc` 색깔이 꽉 채워져 칠해져 있는 부채꼴(Arc)를 그린다.
    - 인수
      - *Handles.DrawSolidArc(eyeTransform.position, Vector3.up, leftRayDirection, fieldOfView, viewDistance);*
        - 시야 각이 그려지는 위치인 `eyeTransform.position` 에서 그려지며 (Vector3 center)
        - `Vector3.up` 축을 기준으로 회전한 부채꼴 (Vector3 normal)
        - `leftRayDirection` 을 부채꼴 시작점으로 (from Vector3)
        - 오른쪽으로 `fieldOfview` 각도만큼 회전한 (float angle)
        - 중심으로부터 호가 그려지는 길이는, 즉 반지름이 `viewDistance` (좀비가 볼 수 있는 거리)인 Arc를 그린다. (float radius) 

<br>
<br>

## 👩‍🦰 이벤트 함수

> 실수로 이름 오타나면 절대 실행 안된다!! (당연한 얘기지만.. 😂)

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
- <u>프레임 속도는 환경마다 다르기 때문에 물리 처리를 update() 함수에서 해주면 환경에 따라 물리 처리 오차가 발생할 수 있어 비추천</u>

### void FixedUpdate()

> 디폴트로 0.02초 (초당 50회)마다 실행된다.

- <u>Update()처럼 매번 반복 실행</u>되나 프레임에 기반하지 않고 어떤 고정적이고 동일한 시간에 기반하여 실행된다.
  - Update()와의 차이점
    - Update()는 화면 한번 깜빡일때마다 실행되서 렉걸리거나 환경이 안좋거나 하면 그만큼 Update()도 적게 실행되지만
    - Fixedupdate()는 환경에 상관없이 무조건 실행 횟수를 지킨다. 정해진 수만큼 무조건 실행 함
- <u>프레임과 상관없이 고정적인 시간마다 실행되기 때문에 환경에 상관없이 물리처리를 오차 없이 실행시킬 수 있다.</u>
  - 물리처리는 **FixedUpdate()** 안에서 해주기.
- `Time.fixedDeltaTime` 의 시간 간격으로 실행이 된다. 디폴트로 `Time.fixedDeltaTime` 값은 0.02f 이다.

### void LateUpate()
  - 유니티 함수로, <u>매 프레임마다 실행되지만 Update()함수 보다 늦게 실행된다.</u>
    - <u>Update() 함수 실행이 다 끝난 후에, Update()함수의 종료 시점에 맞춰서 LateUpdate() 가 실행</u>된다.
  - 예를 들어 캐릭터의 이동 방향 계산은 Update() 에서 끝내준 후,Update()에서 계산 끝낸 캐릭터의 이동 방향에 따라 LateUpdate()에서 카메라가 캐릭터를 따라가도록 하는 식으로 구현한다.

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

> 해당 스크립트가 <u>활성화 될 때마다 자동으로 실행된다.</u>

`컴포넌트`가 꺼져 있다가 켜지는 상태마다 발동된다. Start()와 비슷하지만 Start()는 게임 시작시 한번 발동되는 반면 OnEnable()은 컴포넌트가 꺼져있다가 켜질 때마다 1회씩 실행된다. 
- 게임 시작되자마자 `enabled = true` 상태인 컴포넌트들에게 발동
- 게임 도중에`enabled = false`이다가 `enabled = true`가 되는 컴포넌트들에게 발동
  - 오브젝트 끌 때는 `SetActive(false)`
  - 컴포넌트나 스크립트를 끌 때는 `enabled = false`

<br>

### void OnDisable()

> 해당 스크립트가 <u>비활성화 될 때마다 자동으로 실행된다.</u>

`컴포넌트`가 켜져 있다가 꺼지는 상태마다 발동된다. 

<br>

### void OnDestroy()
오브젝트가 Destroy 될 때 자동 호출되는 이벤트 함수다. 마치 소멸자 같은 것.

<br>

### void OnAnimatiorIK()

- 애니메이션의 관절 꺾는 IK 정보가 갱신될 때마다 발동되는 이벤트 함수.
- Animator와 관련된 이벤트로 애니메이터가 붙어있는 오브젝트만 이 이벤트 함수를 동작시킬 수 있다.
- 씬상에 존재하기는 하나 아직 보이지는 않는 그런 오브젝트들을 기즈모를 통해 보여줌으로서 위치 조정을 쉽게 해주는 등등 개발자 편의를 돕는다. 

<br>

### void OnDrawGizmos()

- 유디터 에디터 내에서만, 씬 상에서만 <u>매 프레임 기즈모를 그리는 역할</u>을 한다.
- 이 함수가 소속된 스크립트가 컴포넌트로서 붙은 오브젝트가 Scene 화면 상에서 항상 보이도록 하는 이벤트 함수.
- 씬상에 존재하기는 하나 아직 보이지는 않는 그런 오브젝트들을 기즈모를 통해 보여줌으로서 위치 조정을 쉽게 해주는 등등 개발자 편의를 돕는다. 

<br>

### void OnDrawGizmosSelected()

- 유디터 에디터 내에서만, 씬 상에서만 <u>Hierarchy 창에서 선택된 오브젝트에 한해서만 매 프레임 기즈모를 그리는 역할</u>을 한다.
- 이 함수가 소속된 스크립트가 컴포넌트로서 붙은 오브젝트가 선택됐을때만 보이도록 하는 이벤트 함수.

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
<br>

## 👩‍🦰 Attributes

### [SerializeField]

```c#
[SerializeField] private GameObject gameoverUI;
```

- private 이지만 `[SerializeField]`이 붙으면 <u>private 하더라도 유니티 엔진의 Inspector 창에서 슬롯이 열려</u> 이를 수정할 수 있다.

<br>

### [Range]

```c#
[Range(0.0f, 12.0f)] public float Size = 12.0f;
```

- 유니티 엔진의 Inspector 창에서 해당 최소값 ~ 최대값을 범위로 가지는 <u>슬라이더 슬롯</u>이 열린다.

<br>

### [HideInInspector]

- public 이어도 유니티 엔진의 Inspector 창에선 해당 멤버 변수가 보이지 않게 된다. 슬롯이 열리지 않음.

<br>
<br>

## 👩‍🦰 전처리기

> **전처리기** 👉 특정 상황에 따라 스크립트를 컴파일할지 말지 결정할 수 있다.

- 유니티는 여러 플랫폼을 빌드할 수 있다. iOS, 안드로이드, 윈도우, 맥OS 등등..
  - 플랫폼에 따른 각각의 코드들을 만들 때 <u>특정 플랫폼에만 컴파일 되는 전처리기를 만들 수 있다.</u>
  - 예를들어 iOS 전처리기 부분은 게임 개발이 완성된 후 iOS로 빌드될 때만 포함된다. 안드로이드로 빌드 될 때는 이 부분의 코드가 빌드에 포함되지 않는다.

 ```c#
    #if UNITY_EDITOR 
      Debug.Log("Unity Editor");  // 유니티 에디터에서만 나오는 로그
    #endif
    
    #if UNITY_IOS
      Debug.Log("Iphone");   // iOS 에서만 나오는 로그. iOS로 빌드할 때만 빌드 된다.
    #endif

    #if UNITY_STANDALONE_OSX
    Debug.Log("Stand Alone OSX");   // 맥 OS 에서만 나오는 로그. 맥 OS로 빌드할 때만 빌드 된다.
    #endif

    #if UNITY_STANDALONE_WIN
      Debug.Log("Stand Alone Windows");  // 윈도우에서만 나오는 로그. 윈도우로 빌드할 때만 빌드 된다.
    #endif
```

<br>
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
- `Alt + 🔻` : `Alt` 누른 채로  Hierarchy창의 오브젝트의 자식 목록들을 볼 수 있는 🔻를 누르면 모~~든 자식의 자식들까지 모든 리스트가 펼쳐진다.

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}