---
title:  "UE4 blueprint 정리" 

categories:
  -  UE4Docs
tags:
  - [Game Engine, UE4]

toc: true
toc_sticky: true

date: 2020-09-05
last_modified_at: 2020-09-05
---

공부하면서 알게된 <u>Blueprint 정리</u>  
{: .notice--warning}

***

> 언리얼 공식 매뉴얼 <https://docs.unrealengine.com/ko/index.html>

## 👩‍🦰 레퍼런스

- C++ 프로그래밍
  - 언리얼 오브젝트를 참조하는 안전한 C++ 포인터
- 블루프린트
  - <u>물체에 명령을 전달할 수 있는 핀을 가진 노드</u>

## 👩‍🦰 블루프린트 종류

- 레벨 블루프린트 
  - <u>다른 레벨에선 동작하지 않고 해당 레벨에서만 동작하게 된다</u>
  - 추가 방법 👉 툴바 - 블루프린트 - 레벨 블루프린트 열기
- 클래스 블루프린트
  - <u>오브젝트별로 동작</u>하게 할 수 있으며 레벨 프린트와 다르게 여러개 생성할 수 있고 <u>다른 레벨에서도 쓸 수 있다.</u>
  - 추가 방법 👉 콘텐츠 브라우저에서 우클 - 블루프린트 클래스 - 부모 클래스를 선택한다.
    - 이렇게 만든 클래스를 더블클릭하면 블루프린트 창이 뜬다.

<br>

## 👩‍🦰 클래스 블루프린트

> 마치 유니티에서의 `Prefab`과 같다.

프리팹처럼 클래스 블루 프린트를 작성하여 미리 에셋으로 생성해둔 후, 뷰포트에 드래그 앤 드롭하여 오브젝트화 할 수 있다.

- 뷰포트에 현재 존재하고 있는 액터에 컴포넌트 추가하듯이(유니티에서 C# 스크립트 추가하듯이) 블루프린트를 추가할 수도 있고
- 프리팹을 미리 만들어 두듯이 콘텐츠 브라우저에 블루프린트 클래스를 미리 생성해둘 수도 있다.

### 컴포넌트

- `큐브`
  - 큐브 모양을 입힘
  - 꼭 큐브 아니라도 디테일 창의 Static Mesh 에서 여러가지 모양의 컴포넌트를 선택할 수 있다.
    - ![image](https://user-images.githubusercontent.com/42318591/92304027-5c026000-efb5-11ea-96ab-b0440194b834.png){: width="60%" height="60%"}{: .align-center}

### 이벤트

- **클래스 블루프린트 같은 경우에는 다음과 같은 이벤트들이 미리 생성이 되어 있음**
  - ![image](https://user-images.githubusercontent.com/42318591/92304203-cc5db100-efb6-11ea-9f67-713175196bc0.png){: width="60%" height="60%"}{: .align-center}
    - `BeginPlay` 👉 게임이 시작되자마자 발생하는 이벤트 
    - `ActorBeginOverlap` 👉 오브젝트끼리 겹쳤을 때, 충돌했을 때 발생하는 이벤트
    - `Event Tick` 👉 계속 매 프레임 호출이 된다. 마치 유니티의 **Update** 함수


<br>

## 👩‍🦰 노드 종류

### Event

- `BeginPlay`
  - 게임이 실행되자마자 가장 먼저 실행됨
  - 1회 실행

<br>

### Input

- 키보드 이벤트
  - 예를들어 `A` 노드를 추가하면 `A` 키보드 키에 대한 레퍼런스 노드가 생성됨. 
- `Enable Input`
  - 기본적으로 언리얼은 액터가 Input을 받아들이는게 비활성화 되어있는 상태라 키보드 입력을 받으려면 이 노드를 추가해 주어야 한다.
    - 컨트롤러로 `Get Player Controller` 리턴값을 연결해주어야 함
- 사용자 지정 입력 축 (Axis Mapping)
  -  Axis Mapping 추가하기
  - 블루프린트창 - 편집 - 프로젝트 세팅 - 입력 - Axis Mappings + 추가 버튼 누르기. 
    - 다음과 같이 추가 (이름은 "LeftRight")
      - ![image](https://user-images.githubusercontent.com/42318591/92319631-7ee45100-f055-11ea-9a60-32ad75ee6beb.png){: width="60%" height="60%"}{: .align-center}
    - A 키를 누르면 Axis Value 로 -1.0 을 리턴하고
    - D 키를 누르면 Axis Value 로 1.0 을 리턴하고
    - **아무 키도 누르지 않으면 Axis Value 는 0.0을 리턴하는**
    - "LeftRight" 이벤트를 이제 노드로 추가할 수 있다!
  - 이런 <u>Axix Mapping 이벤트 노드는 매 프레임마다 실행이 된다</u>
    - 따라서 '누르는 동안' 이런거 구현하기 좋다.
- `AddMovementInput`
  - 캐릭터 같은 `Pawn`의 경우 `SetActorLocation`처럼 트랜스폼 벡터 위치값을 변경해주는 방식이 아닌 이 노드를 사용하여 이동해야 액터끼리의 충돌 처리가 된다.
    - 타겟은 `Pawn`만 연결 가능하다.
  - 입력 값을 리턴하는 핀과 `Scale Value` 핀과 연결해주면 입력에 따라 움직일 수 있게 된다.
  - `Scale Value`의 크기만큼 `World Direction` 방향으로 현재 위치로부터 이동하게 된다.

<br>

### String

- `PrintString`
  - 노드에 적힌 메세지를 화면에 출력한다.

<br>

### Transform

- `SetActorLocation`
  - 액터의 트랜스폼 위치를 지정한다
    - 타겟의 위치를 New Location 위치로 지정한다.
- `GetActorLocation`
  - 액터의 현재 위치를 리턴한다.
- `AddActorWorldRotation`
  - 해당 값만큼 더하여 회전한다.

<br>

### Utility

- `Delay`
  - 지연 시간을 둔다

<br>

### Physics

- `Set Simulate Physics`
  - 유니티의 Rigidbody처럼 중력, 마찰력 같은 물리 효과를 적용한다.
  - *Static Mesh Component* 에만 적용할 수 있음

<br>

### Math

- `Vector + Vector`
  - A + B
    - 어떤 Vector 값을 리턴하는 노드의 실행핀과 연결하면 그게 A 가 되고 `Vector + Vector` 노드에 입력하는 벡터값은 B가 된다.
    - 최종적으로 A + B 두 벡터의 값을 리턴한다.
- `Vector + float`
  - A + B
    - 벡터가 되는 핀과 float이 되는 핀을 각각 연결해와서 결과를 도출해도 되고
    - 연결 되지 않은 피연산자 핀은 입력하면 된다.

<br>

### Game

- `GetPlayerController`
  - 플레이어 컨트롤러를 리턴하는데 플레이어 컨트롤러는 플레이어에게서 받은 입력을 어떤 동작으로 변환하는 함수성을 구현하는, 사람 플레이어의 의지를 나타낸다

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}