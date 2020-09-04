---
title:  "Unity C# > UnityEngine.AI 정리" 

categories:
  -  UnityDocs
tags:
  - [Game Engine, Unity]

toc: true
toc_sticky: true

date: 2020-09-04
last_modified_at: 2020-09-04
---

공부하면서 알게 된 **UnityEngine.AI**를 정리한 문서입니다.😀
{: .notice--warning}

- 유니티 공식 매뉴얼 <https://docs.unity3d.com/kr/current/Manual/UnityManual.html>
- Scripting Overview <http://www.devkorea.co.kr/reference/Documentation/ScriptReference/index.html>


# UnityEngine.AI

> `using UnityEngine.AI`을 해주어야만 사용할 수 있다. 

> AI와 관련된 `NavMesh` 등등에 관련된 작업을 하고 싶을 때 사용

## 👩‍🦰 NavMesh

> AI 에이전트가 걸어다닐 수 있는 표면. 네비게이션 경로를 계산할 수 있는 표면이 된다.

### 함수

#### SamplePosition

  - `SamplePosition((Vector3 sourcePosition, out NavMeshHit hit, float maxDistance, int areaMask)`
    - `areaMask` 에 해당하는 NavMesh 중에서  `maxDistance` 반경 내에서 `sourcePositio`에 <u>가장 가까운 위치를 찾아서</u> 그 결과를 `hit`에 담음

## 👩‍🦰 NavMeshHit

> NavMesh 샘플링의 결과를 담을 컨테이너. Raycast hit 과 비슷

### 변수/프로퍼티

  - `hit.normal` : 광선에 감지된 Collider의 노말 벡터 (충돌 표면의 방향벡터)
  - `hit.distance` : 광선 발사 위치로부터 광선에 감지된 Collider까지의 거리
  - `hit.position` : 광선에 감지된 Collider의 충돌 위치벡터


***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}