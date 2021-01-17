---
title:  "Unity C# > 컴포넌트 : Camera 와 프로퍼티/함수 모음" 

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

# 👩‍🦰 Camera

## 🚀 변수/프로퍼티

### ✈ Camera.main

"Main Camera" 태그가 붙은 현재 활성화 되어 있는 카메라.

<br>

### ✈ nearClipPlane

- 해당 카메라의 위치부터 렌더링을 시작할 최소 위치까지의 <u>거리</u>
  - 카메라 컴포넌트에서 `near` 값 리턴


<br>

## 🚀 함수

화면 상의 좌표는 `Screen` 과 `Viewport` 가 있다. 이 둘을 구분해야 함.

### ✈ ViewportToWorldPoint

> public Vector3 ViewportToWorldPoint(Vector3 position);

> 뷰포트상 좌표를 월드 좌표계로 변환하여 리턴한다.

- 호출한 카메라가 찍고 있는 `Viewport` 상에서의 위치(x, y)와 카메라로부터의 거리(z)를 Vector3로 묶어 넘기면 그 위치를 실제 게임 월드의 위치로 리턴해준다.
  - 화면 좌표계
    - `Screen` 👉 좌하단(0, 0) ~ 우하단(1280, 960) 같이 해상도 픽셀 단위의 화면 좌표계.
      - (0, 0) ~ (Screen.width, Screen.height) 의 범위를 가짐
    - `Viewport` 👉 좌하단(0, 0) ~ 우하단(1, 1) 같이 해상도를 0 ~ 1 사이의 비율 단위로 나타낸 화면 좌표계
- 인수로 넘기는 Vector3의 `z` 값은 카메라 오브젝트로부터 실제 카메라가 찍고 있는 화면까지의 실제 게임 월드 거리, 즉 카메라의 깊이를 의미한다. 카메라로부터의 거리.
  - (0.5f, 0.5f, 0.0f)를 넘겨준다는 것은 카메라 화면의 정중앙을 월드 좌표로 변환하되 `z` 값은 카메라의 위치와 일치. (카메라로부터 거리 0 이므로)

<br>

### ✈ ViewportPointToRay

> public Ray ViewportPointToRay(Vector3 position);

- 카메라가 찍고 있는 화면 `Viewport` 상의 위치를 Vector3로 넘기면 그 화면 상 위치를 가로지르는 `Ray`를 리턴한다. 
  - 과정
    - 카메라 위치로부터, `Viewport` 상에서의 위치(인수의 x, y)를 카메라와의 거리(인수의 z)를 고려하여 월드 기준의 좌표로 변환한 좌표를 향하는 Ray 광선을 리턴한다.

<br>

### ✈ ScreenPointToRay

> public Ray ScreenPointToRay(Vector3 pos);

> public Ray ScreenPointToRay(Vector3 pos, Camera.MonoOrStereoscopicEye eye);

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

<br>

### ✈ WorldToScreenPoint

> public Vector3 WorldToScreenPoint(Vector3 position);

- 월드 좌표계 위치(Vector3)를 인수로 받아서 이를 카메라 화면 상의(`Screen`) 위치로 변환하여 Vector2 를 리턴한다.
  - `Screen` 👉 좌하단(0, 0) ~ 우하단(1280, 960) 같이 해상도 픽셀 단위의 화면 좌표계
  - `Viewport` 👉 좌하단(0, 0) ~ 우하단(1, 1) 같이 해상도를 0 ~ 1 사이의 비율 단위로 나타낸 화면 좌표계
- 카메라가 투영 되면 `z` 값은 무시된채 (x, y) 월드 좌표가 `Screen` 화면 좌표계로 변환될 것이다.

<br>

### ✈ ScreenToWorldPoint

> public Vector3 ScreenToWorldPoint(Vector3 position);

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


<br>

### ✈ ScreenToViewportPoint

> public Vector3 ScreenToViewportPoint(Vector3 position);

- (0, 0) ~ (Screen.width, Screen.height)로 나타낼 수 있는 픽셀 좌표(`Screen`)를 (0, 0) ~ (1, 1)로 나타내는 `Viewport`, 즉 비율 좌표로 변환하여 리턴한다.


***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}