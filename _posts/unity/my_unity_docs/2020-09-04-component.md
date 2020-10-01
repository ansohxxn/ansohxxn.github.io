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

> 기존에 존재하는 컴포넌트와 그 설정들을 다른 오브젝트에 복사-붙여넣기 하는 것이 가능하다.

## 👩‍🦰 Transform

> 오브젝트의 위치, 회전, 크기를 나타내는 컴포넌트다.

- 오브젝트를 생성하면 기본으로 붙어있는 디폴트 컴포넌트다.
  - `Transform myTransform`하고 변수 선언을 해준 후 `myTransform.Rotate()` 이런식으로 쓰는 일반적인 방법도 있긴 하지만 Transform 컴포넌트는 모든 오브젝트들이 디폴트로 갖고 있는 컴포넌트기 때문에 그냥 변수 선언 없이 `transform` 소문자 transform으로 바로 사용하는게 가능하다. `transform.Rotate()` 이런 식으로.
  
    - 함수 `SetParent(부모 Transform)` 로 부모 오브젝트를 지정해줄 수 있다.
      ```c#
      effect.transform.SetParent(parent);
      ```

### 변수/프로퍼티

- 오브젝트의 `position`, `rotation`, `scale` 는 월드 좌표계 위치, 회전, 크기를 담당한다. 
  - `transform.position = Vector3(...)` 이런 식은 현재 위치와 상관 없이 오브젝트의 월드 좌표계 위치를 바꿔버리는 셈이 된다.
  - `localPosition`, `localRotation`, `localScale`은 Local좌표계에서의 위치.
- ✨ <u>부모 자식 관계</u> 또한 Transform이 관리한다.
  - `transform.parent` : 내 부모 오브젝트를 뜻한다.
- `eulerAngles`
  ```c#
  transform.eulerAngles = new Vector3(0.0f, _yAngle, 0.0f);  // Y 축으로 _yAngle 각도 만큼 회전한다.
  ```
  - 오일러 각도의 회전 값을 나타내며 Vector3 를 사용해 회전 값을 설정할 수 있다.
  - 📢 주의 사항
    - 밑에 코드와 같이 절대적인 회전 Vector3 값으로 설정하는 것이 아닌, 이만큼 더 회전해라! 하는 델타 값 의미로 `eulerAngles`에 Vector3를 더하고 빼주는건 안된다.
    - 오일러 각도는 360도를 넘어가면 값의 계산에 실패하기 때문에 `eulerAngles`를 얼만큼 더 회전할지의 델타 회전값으로 사용하는 것은 권장하지 않는다.
      ```c#
      transform.eulerAngles += new Vector3(0.0f, _yAngle, 0.0f);  // '+=' ❌❌❌
      ```
- `transform.forward` : 월드 기준에서 오브젝트 입장에서의 ***앞 쪽*** (월드 기준 z 축) 을 향하는 <u>방향 벡터</u> (길이가 1인)
  - 오브젝트의 로컬 z 축 기준에서의 양의 방향 벡터
- `transform.right` : 월드 기준에서 오브젝트의 입장에서의 ***오른 쪽*** (월드 기준 x 축) 을 향하는 <u>방향 벡터</u> (길이가 1인)

### 함수

#### Translate(Vector3)
- 매개변수로 들어온 현재 위치로부터 해당 벡터만큼 평행이동 시킨다.
- <u>상대적인 벡터의 방향으로</u> 평행 이동 한다.
  - 예를 들어 인수로 들어온 Vector3의 방향이 Vector3.forward 라면 절대적인 월드 좌표계 기준에서의 forward 방향이 아니라 자신을 기준으로 한 forward 방향힘을 나타낸다.
- 기본적으로 Local 좌표계를 기준으로 평행이동한다. (Space.Self가 디폴트)
- `Translate(Vector3), Space.World)`
  - Translate 함수 매개변수로 Space.World를 넘기면 Global 좌표계를 기준으로 평행이동한다.
  - 매개변수 디폴트 값은 Space.self (Local)
    - Translate, Rotate함수는 `Local`인 반면 그냥 바로 변수로 접근하는 `position`, `rotation`같은 것들은 `Global`좌표계 기준이다.
      - 다만 부모가 있는 자식 오브젝트라면 `Local`임

#### Rotate(Vector3)
- <u>현재 회전에서</u> 매개변수로 들어온 상대적인 Vector3만큼 <u>더</u> x, y, z 방향으로 각각 a, b, c도 만큼 <u>회전</u>을 시킨다.
- Translate과 마찬가지로 Space.Self가 디폴트라 로컬 좌표계를 기준으로 회전한다.

#### SetParent(object)
- 함수 `SetParent(부모 Transform)` 로 부모 오브젝트를 지정해줄 수 있다.
  ```c#
  effect.transform.SetParent(parent);
  ```

#### TransformDirection(Vector3)

```c#

Vector3 pos = transform.TransformDirection(Vector3.forward); // 내 로컬 좌표로서의 (0, 0, 1) 로컬 좌표 위치를 월드 좌표계 기준으로 변환하여 리턴해준다.
```
  
  - 인수로 받은 좌표 Vector3 를 *로컬 좌표계 기준에서 월드 좌표계 기준으로*  방향만 변환하여 이를 리턴해준다.(벡터 길이는 변하지 않음)
  - `transform.TransformDirection(Vector3.forward)`와 `transform.forward` 는 같다.

#### InverseTransformDirection(Vector3)
  
  - 인수로 받은 좌표 Vector3 를 *월드 좌표계 기준에서 로컬 좌표계 기준으로*  방향만 변환하여 이를 리턴해준다.(벡터 길이는 변하지 않음)

#### LookAt(Vector3)

```c#
transform.LookAt(_player.transform);
```

인수로 들어온 해당 Vector3 위치값을 바라보게끔 회전시킨다. `Quaternion.LookRotation` 함수는 회전시키는 것이 아닌, 해당 방향을 바라보게끔 회전하려면 얼만큼 회전해야하는지 그 회전값을 리턴해주는 함수고 `Transform`의 `LookAt`은 <u>직접 인수로 들어온 그 '위치'를 바라보게끔 회전시킨다.</u>

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
- `bounds`
  - 구조체다. Collider를 바운딩 박스로 감싸 나타낸 것에 대한 정보를 담는 구조체.
  - `center` : 박스의 중심 벡터
  - `size` : 박스의 크기(길이) 벡터 (스칼라 아님)
  - `extends` : 박스 `size`의 **절반** 크기에 해당하는 벡터 (스칼라 아님)
  - `min` : 박스의 최고점 벡터. 늘 `center` + `extends` 이다.
  - `min` : 박스의 최하점 벡터. 늘 `center` - `extends` 이다.
  - ![image](https://user-images.githubusercontent.com/42318591/93081119-b5f7d980-f6c9-11ea-9e7d-dcb0cd151623.png){: width="60%" height="60%"}{: .align-center}

<br>

## 👩‍🦰 Capsule Collider

> 캡슐 모양의 Collider로 주로 인체, 나무, 가로등과 같은 긴 형태의 모델의 Collider로 사용 된다. 

- Collider는 <u>물리적으로 오브젝트끼리 표면에 충돌이 일어났을 때를 감지</u>하고 이를 처리하는 컴포넌트다. 
- Edit Collider 버튼을 눌러 캡슐의 모양을 알맞게 손볼 수 있다.
- **Direction**으로 축을 변경할 수 있다. 크기의 기준이 되는 축.
  - Z-Axis 앞뒤 크기 
  - X-Axis 좌우 크기
  - Y-Axis 위아래 크기






<br>

## 👩‍🦰 Mesh Collider

와이어가 복잡하게 이루어져 있는 Mesh 의 경우, Mesh 를 바탕으로 Collider를 따는 Mesh Collider를 콜라이더로 사용하면, 그리고 이런 Mesh Collider가 게임 월드 상에 많다면 연산이 어마어마하게 소요 될 것이다. 이처럼 좀 세밀하게 Mesh 모양 그대로를 충돌 처리를 받아야 하는 경우엔 Mesh Collider를 쓰기도 하지만 충돌 전용 Mesh를 따로 만들기도 한다. 

- `Convex`를 체크하면 Mesh 자체를 단순화 해서 Collider로 만들어 부하를 줄여준다.

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
- `nearClipPlane`
  - 해당 카메라의 위치부터 렌더링을 시작할 최소 위치까지의 <u>거리</u>
    - 카메라 컴포넌트에서 `near` 값 리턴

### 함수

화면 상의 좌표는 `Screen` 과 `Viewport` 가 있다. 이 둘을 구분해야 함.

#### `ViewportToWorldPoint(Vector3)`

> 뷰포트상 좌표를 월드 좌표계로 변환하여 리턴한다.

- 호출한 카메라가 찍고 있는 `Viewport` 상에서의 위치(x, y)와 카메라로부터의 거리(z)를 Vector3로 묶어 넘기면 그 위치를 실제 게임 월드의 위치로 리턴해준다.
  - 화면 좌표계
    - `Screen` 👉 좌하단(0, 0) ~ 우하단(1280, 960) 같이 해상도 픽셀 단위의 화면 좌표계.
      - (0, 0) ~ (Screen.width, Screen.height) 의 범위를 가짐
    - `Viewport` 👉 좌하단(0, 0) ~ 우하단(1, 1) 같이 해상도를 0 ~ 1 사이의 비율 단위로 나타낸 화면 좌표계
- 인수로 넘기는 Vector3의 `z` 값은 카메라 오브젝트로부터 실제 카메라가 찍고 있는 화면까지의 실제 게임 월드 거리, 즉 카메라의 깊이를 의미한다. 카메라로부터의 거리.
  - (0.5f, 0.5f, 0.0f)를 넘겨준다는 것은 카메라 화면의 정중앙을 월드 좌표로 변환하되 `z` 값은 카메라의 위치와 일치. (카메라로부터 거리 0 이므로)

#### `ViewportPointToRay(Vector3)`
- 카메라가 찍고 있는 화면 `Viewport` 상의 위치를 Vector3로 넘기면 그 화면 상 위치를 가로지르는 `Ray`를 리턴한다. 
  - 과정
    - 카메라 위치로부터, `Viewport` 상에서의 위치(인수의 x, y)를 카메라와의 거리(인수의 z)를 고려하여 월드 기준의 좌표로 변환한 좌표를 향하는 Ray 광선을 리턴한다.

#### `ScreenPointToRay(Vector3)`

  ```c#
  Ray ray = Camera.main.ScreenPointToRay(Input.mousePosition); 
  ```
  - 호출한 카메라 위치로부터 픽셀 단위의 화면상 좌표의 실제 월드 좌표로 향하는 방향의 `Ray` (광선)을 리턴한다.
    - 아래 과정은 위 함수 한 줄과 비슷하다. 아래 과정에다가 추가로 해당 `dir`로 향하는 `Ray`를 직접 리턴한다.
      ```c#
            Vector3 mousePos = Camera.main.ScreenToWorldPoint(new Vector3(Input.mousePosition.x, Input.mousePosition.y, Camera.main.nearClipPlane));
            Vector3 dir = mousePos - Camera.main.transform.position;
            dir = dir.normalized;
      ```
  - 마찬가지로 화면상 좌표(X, Y)와 함께 월드 좌표의 Z 값을 계산할 때 고려할 카메라와의 거리 또한 Z 로 함께 Vector3로 묶어 넘겨 준다.
- *Raycast* 에 시작 위치와 방향을 넘기는 대신 광선 자체인 `ray`를 넘겨준다.

#### `WorldToScreenPoint(Vector3)`
- 월드 좌표계 위치(Vector3)를 인수로 받아서 이를 카메라 화면 상의(`Screen`) 위치로 변환하여 Vector2 를 리턴한다.
  - `Screen` 👉 좌하단(0, 0) ~ 우하단(1280, 960) 같이 해상도 픽셀 단위의 화면 좌표계
  - `Viewport` 👉 좌하단(0, 0) ~ 우하단(1, 1) 같이 해상도를 0 ~ 1 사이의 비율 단위로 나타낸 화면 좌표계
- 카메라가 투영 되면 `z` 값은 무시된채 (x, y) 월드 좌표가 `Screen` 화면 좌표계로 변환될 것이다.

#### `ScreenToWorldPoint(Vector3)`
  - 픽셀 단위의 화면 좌표계 위치(x, y)와 월드 좌표 z 를 구하는데 사용할 카메라로부터의 원하는 거리(z) 를 Vector3로 묶어 넘기면 월드 좌표계로 변환해주고 이를 리턴해준다.

```c#
 Vector3 mousePos = Camera.main.ScreenToWorldPoint(new Vector3(Input.mousePosition.x, Input.mousePosition.y, Camera.main.nearClipPlane)); 
```

- *ScreenToWorldPoint* 함수 👉 화면 좌표계를 월드 좌표계로 변환하고 리턴함
  - *ScreenToWorldPoint* 에다가
    - 마우스를 클릭한 화면상의 좌표(X, Y)와 
      - (0, 0) ~ (Screen.width, Screen.height) 범위를 가진다.
    - 화면 상 좌표는 Z 값이 없다. 따라서 월드 좌표로서의 Z 값을 정할 때 고려할 <u>카메라와의 거리</u> 를 함께 Vector3로 묶어 넘겨 주어야 한다.
      - 현재 카메라의 위치인 `camera.main.transform.position.z`값으로부터 Vector3로 함께 묶어서 넘긴 `z` 값(카메라부터의 거리)을 더해준 값을 화면 좌표계를 월드 좌표계로 변환했을 때의 `z`값으로 삼는다. 
        - 단지 인수로 넘겨주는 Vector3 의 `z`값은 이 값으로 월드 `z` 좌표로 삼겠다는게 아니라 이걸 카메라와의 거리로 받아들이고 이걸 고려해서 월드 좌표계 기준 `z`로 만들겠다는 것이다. 착각 노노!
        - `z`값 없이 화면 좌표계(X, Y)를 가지던 마우스 클릭한 곳을 월드 기준으로 전환한 좌표의 `z`값은 위의 예로는 *camera.main.transform.position.z + Camera.main.nearClipPlane* 가 된다.
          - 카메라의 `nearClipPlane` 은 카메라 컴포넌트의 `near`. 카메라가 카메라로부터 렌더링을 시작하는 곳 까지의 z 축 기준의 거리
  - 이렇게 세 가지 요소를 Vector3 로 묶어서 인수로 넘겨주면 적절한 월드 좌표계로 변환해준다. 

![image](https://user-images.githubusercontent.com/42318591/94434628-0fd1c680-01d5-11eb-8194-04f1fb61517a.png)

예를 들어 게임 화면 상에서 화면상 픽셀 좌표 기준으로 (160, 180)에 해당하는 픽셀에 마우스 클릭을 했다고 가정해보자. 이 좌표는 Vector3 로 보면 (160, 180, 0) 상태일 것이다. 화면상 좌표는 z 값 없이 2D 니까.. 클릭했던 이 (160, 180) 픽셀 좌표를 게임 월드 기준으로 변환하고자 *ScreenToWorldPoint(Vector3(160, 180, 3))* 로 실행시켰다고 가정해보자. `z`값으로 넘긴 3 을 카메라와의 '거리'를 3 이라고 고려하여 화면상 좌표인 (160, 180)를 월드 기준 좌표로 변환했더니 (3.7, 4.8, -0.6)이 되었다. 현재 카메라 위치의 `z` 값 + 3 👉 - 0.6.

#### `ScreenToViewportPoint(Vector3)`
- (0, 0) ~ (Screen.width, Screen.height)로 나타낼 수 있는 픽셀 좌표(`Screen`)를 (0, 0) ~ (1, 1)로 나타내는 `Viewport`, 즉 비율 좌표로 변환하여 리턴한다.


<br>

## 👩‍🦰 Rigidbody

> 오브젝트에 <u>현실적이고 사실적인 물리 기능</u>을 추가해준다.
 
- <u>오브젝트가 중력의 영향을 받게 됨</u>
  - `Use Gravity` 체크 해제하면 중력 영향 안받게 됨
- 중력 말고도 질량, 마찰력 등등 현실 세계에 존재하는 물리적인 힘들 설정 가능. 
  -  `Is Kinematic`가 체크가 되어 있다면!! 중력은 물론이고 모든 물리적인 힘들을 받지 않게 된다.
- <u>Collider에 물리학을 입힌다.</u>
- `Constraints`로 특정 축으로는 이동, 회전 안되게 제한할 수 있다.
- `Mass` 질량
  - 질량이 클수록 무거워서 물리적인 힘을 덜 받는다. 
- `Drag` 저항
  - 0 이면 저항이 없어 무중력 상태와 같고
  - 1 이면 지구와 비슷한 환경의 저항
  - 10 정도면 달과 같은 환경
- `Angular drag` 밀리는 정도
  - 크면 클 수록 물리적인 힘에 의해 밀려나는 정도가 덜 하다

### 변수/프로퍼티

- `velocity`
  - 현재 속도. Vector3 벡터이다.
    - 이 `velocity`를 사용해 속도 벡터를 직접 인위적으로 설정해주면 `velocity`만큼 움직이게 된다. 
  - cf) 점프를 구현할 땐 AddForce 함수보단 `velocity`를 사용하는게 낫다.
    - `AddForce` 함수로 Vector.up 방향으로 힘을 주어 점프를 하게 한다면 위의 힘을 서서히 받아 지수 그래프 모양으로 움직이기 때문에 점프가 순간적으로 되지 않는다. 서서히 위로 올라가는 그림으로 연출된다. 
    - 반면에 `velocity`값을 수정 해주면 인위적으로 수정한 `velocity` 벡터 값 만큼 순간적으로 이동하게 되므로 점프 같이 순간적으로 바로 이동해야 하는 것을 구현하기에 딱이다.
      - 순간적으로 `velocity` 만큼 이동(점프)한 후에는, Rigidbody 답게 중력에 의하여 서서히 떨어지게 된다. `velocity` 벡터 값도 이제는 중력 같은 물리적인 현상 아래에 있게 되므로 점프를 하고 난 다음에는 `velocity` 벡터 값이 중력 가속도 등의 영향을 받아 알아서 물리적인 현상에 의해 바뀌게 된다.
- `useGravity`
  - 중력을 쓸지 말지 체크, 체크 해제 할 수 있다. Boolean 타입.

### 함수

#### `Addforce(x, y, z)`
  - 오브젝트가 x, y, z 축 방향으로 물리적인 힘을 받도록 한다. 
  - 힘의 정도를 매개변수로 받아서 관성, 마찰력, 중력 등등 여러가지를 고려해서 내부적으로 계산한 힘을 처리한다.

#### `AddExplosionForce(float, Vector3, float)`
  - 매개변수로 넘겨받은 정보를 가지고 폭발에 대한 물리처리를 해준다. 폭발 시키기!
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
  - `GetBool("파라미터 이름")`, `GetFloat("파라미터 이름")` 등등 : 인수에 해당하는 파라미터 값을 가져온다.

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