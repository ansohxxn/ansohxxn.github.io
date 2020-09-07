---
title:  "Unity C# > UnityEngine의 클래스들 정리" 

categories:
  -  UnityDocs
tags:
  - [Game Engine, Unity]

toc: true
toc_sticky: true

date: 2020-09-04
last_modified_at: 2020-09-04
---

공부하면서 알게 된 **UnityEngine의 클래스**들을 정리한 문서입니다.😀
{: .notice--warning}

- 유니티 공식 매뉴얼 <https://docs.unity3d.com/kr/current/Manual/UnityManual.html>
- Scripting Overview <http://www.devkorea.co.kr/reference/Documentation/ScriptReference/index.html>


# UnityEngine

> `using UnityEngine`을 해주어야만 사용할 수 있다.

## 👩‍🦰 Vector2, Vector3

> 위치 좌표, 벡터

### 변수/프로퍼티

  - `magnitude` : 벡터의 길이 및 크기. float
  - `sqrMagnitude` : 벡터의 길이 제곱. float
  - `normalized` : 해당 벡터의 방향 벡터. (길이 1)
  - `forward` : 해당 벡터의 ***앞 쪽*** (z 축) 을 나타내는 <u>방향 벡터</u> (길이가 1인)
  - `right` : 해당 벡터의 ***오른 쪽*** (x 축) 을 나타내는 <u>방향 벡터</u> (길이가 1인)
  - `up` : 해당 벡터의 ***위 쪽*** (y 축) 을 나타내는 <u>방향 벡터</u> (길이가 1인)
  - `zero` : 원점. 
    - `Vector2.zero` 👉 (0, 0)
    - `Vector3.zero` 👉 (0, 0, 0)

### 함수

#### `SmoothDamp(Vector, Vector, ref Vector3, float)` 함수
- 매개변수 첫번째 벡터가
- 매개변수 두번째 벡터로 되기까지
- 네번째 매개변수인 시간(float)동안 스무스하게 변화하는 값을 리턴한다.
- 세번째 매개변수는 Call by reference인 ref 참조 변수로서 직전, 마지막 프레임에서의 속도를 나타낸다. 함수 안에서 계산되어 바깥으로 꺼내짐.

#### `Distance(Vector a, Vector b)`
- a 와 b 사이의 거리를 리턴한다. float 리턴.

#### `Angle(Vector a, Vector b)`
- a 와 b 사이의 각도를 리턴한다. float 리턴.


<br>

## 👩‍🦰 Debug
> 개발자가 쉽게 디버깅하기 위해 포함된 방법의 클래스

### 함수

#### `Debug.Log` 

- 콘솔 출력

#### `Debug.DrawRay` 

- 레이캐스트 광선을 그려주어 개발자가 시각적으로 볼 수 있게끔 해준다.

#### `Debug.LogFormat` 

- 출력 포맷 설정

<br>

## 👩‍🦰 Mathf

> 수학과 관련된 함수들이 미리 들어있는 함수 집합

### `Max(a, b)` 
a, b 둘 중 더 큰 것을 리턴한다.

### `sqrt(a)` 
루트 a 

### `Floor(float)`
내림. 소수점 버림

### `SmoothDamp(float, float, ref float, float)` 
- 매개변수 첫번째 값이
- 매개변수 두번째 값으로 되기까지
- 네번째 매개변수인 시간(float)동안 스무스하게 변화하는 값을 리턴한다.
- 세번째 매개변수는 Call by reference인 ref 참조 변수로서 직전, 마지막 프레임에서의 속도를 나타낸다. 함수 안에서 계산되어 바깥으로 꺼내짐.

### `SmoothDampAngle(float, float, ref float, float)`
- 👉 SmoothDamp 함수와 기능은 동일하나 <u>각도를 고려해서 스무스하게 변경시킨다.</u>
  - 360도 체계에서는 예를들어 -270도와 90도는 같은 회전값임을 의미하니까 사실 -270도면 90도만 돌면 되는건데 일반 SmoothDamp 함수를 사용하면 270도씩 돌아버리니까 SmoothDampAngle을 사용하면 <u>의도와 달리 더 많이 회전하는 경우를 막아 줌</u> !

### `Clamp(float target, float a, float b);`
- target 이 a ~ b 범위를 벗어나지 않도록 한다. 

### `RoundToInt(float)`
- 인수를 정수로 반올림

### Lerp(float a, float b, float t)

> a, b 사이의 t (0~1)만큼 위치한 값을 리턴한다.

- t 가 0.5라면 리턴되는 값은 a 와 b 의 중간값!

<br>

## 👩‍🦰 Physics

> 물리와 관련된 함수들이 미리 들어있는 함수 집합

### 변수/프로퍼티

- `Physics.gravity`
    - 중력 가속도 상수 벡터
    - Physics.gravity.y 는 y 방향의 중력 가속도를 뜻함.

### 함수
  
#### `Physics.OverlapSphere`

> 구의 중심 위치(pos)와 반지름을 매개변수로 넘겨주면 구의 <u>반경 내에 있는 모든 * Collider * </u> 들을 <u>Collider 배열로 리턴</u>한다.

- `Physics.OverlapSphere(Vector3 pos, float radius)`
  - 추가 매개변수
    - layer도 같이 넘겨주면 해당 layer에 해당하는 Collider들만 들어있는 배열을 리턴한다. 

#### `Physics.SphereCastNonAlloc`

 > 이 함수는 인수로 방향과 거리를 넘겨주면 구가 해당 방향과 거리로 <u>이동한 ✨궤적✨에 겹치는 Collider가 있는지를 검사한다.</u> 이게 바로 <u>Cast계열 함수들의 특성</u>
    
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

#### `Physics.Raycast`

>  레이캐스트. 눈에 보이지 않는 광선을 쏴서 물리적 충돌을 감지한다.

- `Physics.Raycast(발사위치 Vector3, 발사방향 Vector3, 광선길이 float, 감지할 레이어 Layer)`
    - 발사 위치와 발사 방향은 필수 매개변수다.
- `Physics.Raycast(광선 Ray, 충돌 정보 RaycastHit, 광선길이 float, 감지할 레이어 Layer)`
    - Boolean 타입을 리턴한다. `Raycast`에 걸린게 있다면 true 리턴.
    - `Ray`는 광선의 바랏 위치와 발사 방향을 담고 있는 광선이다.
    - 광선과 충돌한 Collider의 정보를 담는 역할을 하는 `RaycastHit` 타입의 변수도 매개변수로 넘겨준다. 이때 `out`을 함께 써서 바로 적용될 수 있게 해주기.
      - *if (Physics.Raycast(ray, `out hit`, distance, whatIsTarget))*
        - 광선에 충돌이 감지되는 순간, 이 if 조건문이 실행되자마자 RaycastHit 타입의 hit 변수에 충돌한 Collider의 정보가 들어간다. 
    - 감지할 레이어 마스크인 다섯번째 인수에 `~`을 붙여주면 그 레이어 마스크가 붙은 오브젝트들은 레이 캐스트 처리 하지 말고 무시하라는 뜻.

#### `Physics.Linecast`

> 라인캐스트. 눈에 보이지 않는 광선을 쏴서 물리적 충돌을 감지한다.

- `Physics.Raycast(발사시작위치 Vector3, Linecast 종료 지점 Vector3, 충돌 정보 RaycastHit, 감지할 레이어 Layer)`
- Raycast 와의 차이점 
    - 👉 Linecast는 정확한 종료 지점을 알 고 어떤 범위 내에서의 충돌을 감지하려 할 때, Raycast는 방향만 알 때 사용! 
  - start과 **end** <u>사이의 범위</u>내에서 교차하는 충돌이 있을 경우 true를 리턴
  - 충돌 정보는 `Raycast hit`에 저장된다.

<br>

## 👩‍🦰 Random

> 난수 생성과 관련된 함수들이 있는 집합

### 변수/프로퍼티
  - Random.`insideUnitSphere`
    - 반지름 1 을 갖는 구 안의 랜덤한 위치(Vector3)를 반환하는 프로퍼티. 
      ```c#
      var randomPos = Random.insideUnitSphere * distance + center;  // center를 중점으로 하여 반지름(반경) distance 내에 랜덤한 위치 리턴
      ```
    
### 함수

#### Random.`Range(int min, int max)`
- [min, max) 범위내에서 <u>int 타입의 랜덤한 정수</u>를 리턴한다.
    - max는 포함되지 않는다. 

#### Random.`ColorHSV()`
- 랜덤한 컬러를 리턴한다.
  

<br>

## 👩‍🦰 Input

>  UnityEngine에서 제공하는 <u>입력</u>과 관련된 집합. 키보드, 모바일, 조이스틱의 입력을 받을 수 있는 여러 함수들의 집합.

### 함수

#### `Input.GetKey(int)` `UP` `Down` 

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

  #### `Input.GetAxis("Horizontal")`

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

## 👩‍🦰 Time

> 시간과 관련된 함수나 변수의 집합

### 변수/프로퍼티

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

## 👩‍🦰 Color

색깔들이 미리 이름 붙어 구현되어 있다. `Color.red`, `Color.yellow` 이런 식으로 되있어서 그냥 갖다 쓰기만 하면 됨.

### 함수

#### Lerp(Color a, Color b, float t)

> a, b 색깔 사이의 t (0~1) 비율에 위치한 색을 리턴한다.

- t 가 0.5라면 리턴되는 값은 a 와 b 의 중간값!
- 

<br>

## 👩‍🦰 Quaternion

> 쿼터니언과 관련된 함수들 집합. `3차원 회전`을 위한 함수.

### 변수/프로퍼티 

- `eulerAngles`
  - 변수다. `Quaternion.eulerAngles`이렇게 쓰는게 아니라 `Quaternion타입을 참조하는 변수.eulerAngles` 이렇게 쓴다.
  - 쿼터니언을 오일러각으로 변환시킨다. 즉 Vector3로 변환한다. 
    - Quaternion.Euler(Vector3)와 반대.
- `identity`
  - 변수다. 
  - 0도, 0도, 0도로 회전한 쿼터니언 값.

### 함수

#### `Quaternion.Euler(Vector3)`
  - 매개변수로 받은 Vector3를 Quaternion타입으로 변환하여 리턴해주는 함수

#### `Quaternion.LookRotation(Vector3)`
  - 벡터를 매개변수로 넣어주면 오브젝트가 그 벡터의 방향을 쳐다보게끔 자기 자신의 방향을 회전한다.
  - 현재 위치좌표에서 매개변수로 들어온 <u>벡터만큼 더한 목적지 위치 좌표를 바라보게</u> 된다. 
    - 따라서 `벡터의 뺄셈`으로 `목적지 위치좌표 - 출발지 위치좌표` 해주어 필요한 방향과 거리를 나타낼 벡터를 구해주는 것이 좋다. 그리고 매개변수로 넘기기.

#### `Quaternion.Lerp(Vector3, Vector3, float)`
  - 두 개의 회전 값(Vector3)을 정하면 float 비율만큼의 적당한 회전값을 리턴

#### `Quaternion.Lerp(aRotation, bRotation, 0.5f);`
  - `0.5f`면 딱 중간
  - `1.0f`면 bRotation을 그대로 따름
  - `0.0f`면 aRotation을 그대로 따름
  - `0.2f`면 aRotation에 좀 더 가깝게 회전

#### `Quaternion.AngleAxis(float angle, Vector3 axis);`
  - 축 axis 주위를 angle 만큼 회전한 rotation을 생성하고 리턴한다.
    - 예를 들어 중심 축이 되는 `axis`가 y 축이라면 회전 값이 y 값은 변하지않고 x, z 값만 변한다.
      - ![image](https://user-images.githubusercontent.com/42318591/91792755-40175b00-ec51-11ea-84b5-5537a296cdc3.png){: width="70%" height="70%"}{: .align-center}

<br>

## 👩‍🦰 Gizmos

> `OnDrawGizmos()`, `OnDrawGizmosSelected()` 같은 이벤트 함수 내에서 사용되며 개발자의 편의를 위해 Scene상에서만 보여지는 기즈모를 그리는 함수들이 모여이는 클래스다.

### 변수/프로퍼티 

  - `color` 기즈모 컬러

### 함수

#### `DrawSphere(Vector3 center, float radius` 

- 기즈모가 될 구를 그린다. 

<br>

## 👩‍🦰 PlayerPrefs

> 유니티에서 지원해주는 데이터 자료구조 타입이다. 문자열인 Key 값과 그에 따른 value를 묶어 로컬 파일로서 저장이 된다.(<u>로컬 파일로서 저장이 되기 때문에 껏다 켜도 그 값이 유지가 된다</u>) 

- 이를 다루는 여러 함수들도 지원한다. Key 값만 알고 있다면 value를 찾을 수 있다. PlayerPref는 구조가 간단해서 해킹당하기 쉽다.

### 함수

#### PlayerPrefs.SetInt("BestScore", score);
  - "BestScore"라는 Key값에 int타입인 score를 파일로서 묶어 저장한다. 

#### PlayerPrefs.GetInt("BestScore")
  - "BestScore"라는 Key값의 int 타입인 value값을 리턴한다.
  - "BestScore"라는 Key값이 없더라도 에러는 발생하지 않는다. Key값이 없으면 0을 리턴함.

<br>

## 👩‍🦰 Ray
레이캐스트의 광선 그 자체. 발사 위치와 발사 방향을 담고 있는 구조체다.

<br>

## 👩‍🦰 RaycastHit

> `Raycast` 레이캐스트로부터 정보를 얻기 위한 타입으로 <u>광선에 감지된 Collider의 정보를 담는 컨테이너</u>다.

### 변수/프로퍼티
- RaycastHit hit 일때
  - `hit.normal` : 광선에 감지된 Collider의 노말 벡터 (충돌 표면의 방향벡터)
  - `hit.distance` : 광선 발사 위치로부터 광선에 감지된 Collider까지의 거리
  - `hit.point` : 광선에 감지된 Collider의 충돌 위치벡터
  - `hit.collider` : 광선에 감지된 Collider


***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}