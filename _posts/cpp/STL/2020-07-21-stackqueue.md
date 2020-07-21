---
title:  "[STL 컨테이너] stack & queue & priority_queue" 

categories:
  - STL
tags:
  - [Data Structure, Coding Test, Cpp, STL]

toc: true
toc_sticky: true

date: 2020-07-21
last_modified_at: 2020-07-21
---

# 스택 & 큐 & 우선순위 큐

> `Container Adaptor` 👉 <u>다른 컨테이너 클래스</u>들을 상속 받아서 다른 컨테이터 클래스의 객체에 <u>특정한 인터페이스</u>를 제공해준다.

- 순서 인덱스로 접근하는 `Sequence Container`, Key로 접근하는 `Associative Container`과는 다르게 `pop()` 연산을 통해 삭제시킬 때만 접근할 수 있다.

## 🔔 스택

> #include \<stack>

- 삽입되고 삭제되는 쪽이 한쪽 밖에 없다.
  - 삽입, 삭제가 모두 `top`에서 이루어짐.

1. `LIFO` 방식으로 구현할 때 사용된다.
  - 가장 먼저 삽입된 것이 가장 나중에 삭제되고 
  - 가장 나중에 삽입된 것이 가장 먼저 삭제된다.
2. `DFS` 깊이 우선 탐색에 사용 된다.

- `vector`, `deque`, `list` 를 기반으로 사용 가능하다.
  - 내부적으로는 `vecotr`, `deque`, `list` 구조로 구현이 되어 있되 `statck`과 같이 작동하도록 멤버 함수를 지원한다.
  - 디폴트로 `deque` 기반으로 작동한다.

### 함수 

- `size()` : 스택의 size. 원소 개수 리턴
- `empty()` : 스택이 빈 스택인지, 즉 원소 개수가 0 인지에 대한 true or false 리턴
- `top()` : <u>스택에서 가장 나중에 들어간 원소 리턴</u> 삽입 삭제시 무조건 여기서 이루어진다.
- `push(n)` : 스택에 원소를 삽입
- `pop()` : `top()`을 삭제한다. 가장 나중에 들어간 원소 삭제

> 다른 컨테이너들과 다르게 `clear()`같은 함수가 없기 때문에 일일이 `pop()` 해주어야 한다. 

```cpp
stack<int> s;
while(!s.empty())
{
    s.pop();
}
```

<br>

## 🔔 큐

> #include \<queue>

- `back`에서 삽입이 이루어지고 `front`에서 삭제가 이루어진다.

1. `FIFO` 방식으로 구현할 때 사용된다.
  - 가장 먼저 삽입된 것이 가장 먼저 삭제되고 
  - 가장 나중에 삽입된 것이 가장 나중에 삭제된다.
2. `BFS` 너비 우선 탐색에 사용 된다.

### 함수 

- 스택의 `pop`과 다르게
  - `front()` : <u>스택에서 가장 먼저 들어간 원소 리턴</u>
  - `back()` : <u>스택에서 가장 나중에 들어간 원소 리턴</u>
- 다른건 다 스택과 동일하다!

- `deque`, `list` 를 기반으로 사용 가능하다.
  - 내부적으로는 `deque`, `list` 구조로 구현이 되어 있되 `queue`과 같이 작동하도록 멤버 함수를 지원한다.
  - 디폴트로 `deque` 기반으로 작동한다.
- `vector`는 불가능하다.
  - 큐 특성상 뒤에서 삽입하고 앞에서 빠져야 하는데 `vector`는 앞에서 삭제되는 동작은 지원하지 않기 때문.

<br>

## 🔔 우선순위 큐

> #include \<priority_queue>

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}