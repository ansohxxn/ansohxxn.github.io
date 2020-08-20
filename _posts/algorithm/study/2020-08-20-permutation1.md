---
title:  "(C++) 순열(Permutation) 구현하기" 

categories:
  - Algorithm
tags:
  - [Algorithm, Coding Test, Cpp, Recursion, STL]

toc: true
toc_sticky: true

date: 2020-08-20
last_modified_at: 2020-08-20
---

> 순열은 완전 탐색(브루트 포스) 문제에서 많이 등장하므로 잘 익혀놓자.

<br>

## 순열이란

- 순서를 따진다.
  - `abc` 와 `abc`는 서로 다른 존재이다.
- 중복을 허용하지 않는다.
- `nPr`
  - 5P3 = 5 X 4 X 3
  - 4P1 = 4
  - 4P4 = 4! = 4 X 3 X 2 X 1

<br>

## 재귀로 구현한 코드

> 예시) `{'a', 'b', 'c', 'd'}` 배열의 `4P3`의 순열 출력하기

```cpp
#include <iostream>

using namespace std;

void swap(char & a, char & b)
{
    char temp = a;
    a = b;
    b = temp;
}

void permutation(char data [], int depth, int n, int r)
{
    if (depth == r)
    {
        for(int i = 0; i < r; i++)
            cout << data[i] << " ";
        cout << endl;
        
        return;
    }
    
    for(int i = depth; i < n; i++)
    {
        swap(data[depth], data[i]);   // 스왑
        permutation(data, depth + 1, n, r);  // ⭐재귀
        swap(data[depth], data[i]);  // 다시 원래 위치로 되돌리기
    }
}
```
```cpp

int main()
{
    char arr [] = {'a', 'b', 'c', 'd'};
    
    permutation(arr, 0, 4, 3); // 4P3

    return 0;
}

```
```
💎출력💎

a b c
a b d
a c b
a c d
a d c
a d b
b a c
b a d
b c a
b c d
b d c
b d a
c b d
c a b
c a d
c d a
c d b
d b c
d b a
d c b
d c a
d a c
d a b
```

### 설명

- <u>{a, b, c, d}</u> 배열의 `4P3` 순열을 예시로 들어 보겠다.

- **재귀**
  - {a, b, c, d} 의 모든 `4P3`짜리 순열
    - **depth = 0**
    - 1️⃣ 첫 원소가 `a`로 시작하는 {b, c, d} 의 모든 `3P2`짜리 순열
      - 👉 {a, b, c, d} 에서 `a`와 `a`끼리 스왑 👉🏻 {`a`, b, c, d}
        - **depth = 1**
        - 1️⃣ 첫 원소가 `ab`로 시작하는 {c, d} 의 모든 `2P1`짜리 순열
          - 👉 {a, b, c, d} 에서 `b`와 `b`끼리 스왑 👉🏻 {`a`, `b`, c, d}
            - **depth = 2**
            - 1️⃣ 첫 원소가 `abc`로 시작하는 {d} 의 모든 `1P0`짜리 순열
              - 👉 {a, b, c, d} 에서 `c`와 `c`끼리 스왑 👉🏻 {`a`, `b`, `c`, d}
                - **depth = 3**
                  - <u>depth = 3 가 되었으므로 인덱스 2 까지 a b c 를 출력 한다.</u>
                  - 다시 한단계 이전 정렬로 되돌리기 {a, b, c, d}
            - 2️⃣ 첫 원소가 `abd`로 시작하는 {c} 의 모든 `1P0`짜리 순열
              - {a, b, c, d} 에서 `c`와 `d`끼리 스왑 👉🏻 {`a`, `b`, `d`, c}
                - **depth = 3**
                  - <u>depth = 3 가 되었으므로 인덱스 2 까지 a b d 를 출력 한다.</u>
                  - 다시 한단계 이전 정렬로 되돌리기 {a, b, c, d}
            - 다시 한단계 이전 정렬로 되돌리기 {a, b, c, d}
        - 2️⃣ 첫 원소가 `ac`로 시작하는 {b, d} 의 모든 `2P1`짜리 순열
          - 👉 {a, b, c, d} 에서 `b`와 `c`끼리 스왑 👉🏻 {`a`, `c`, b, d}
            - **depth = 2**
            - 1️⃣ 첫 원소가 `acb`로 시작하는 {d} 의 모든 `1P0`짜리 순열
              - 👉{a, c, b, d} 에서 `b`와 `b`끼리 스왑 👉🏻 {`a`, `c`, `b`, d}
                - **depth = 3**
                  - <u>depth = 3 가 되었으므로 인덱스 2 까지 a c b 를 출력 한다.</u>
                  - 다시 한단계 이전 정렬로 되돌리기 {a, c, b, d}
            - 2️⃣ 첫 원소가 `acd`로 시작하는 {b} 의 모든 `1P0`짜리 순열
              - {a, c, b, d} 에서 `b`와 `d`끼리 스왑 👉🏻 {`a`, `c`, `d`, b}
                - **depth = 3**
                  - <u>depth = 3 가 되었으므로 인덱스 2 까지 a c d 를 출력 한다.</u>
                  - 다시 한단계 이전 정렬로 되돌리기 {a, c, b, d}
            - 다시 한단계 이전 정렬로 되돌리기 {a, b, c, d}
        - 3️⃣ 첫 원소가 `ad`로 시작하는 {b, c} 의 모든 `2P1`짜리 순열
          - 👉 {a, b, c, d} 에서 `a`와 `d`끼리 스왑 👉🏻 {`a`, `d`, b, d}
            - **depth = 2**
            - 1️⃣ 첫 원소가 `acb`로 시작하는 {d} 의 모든 `1P0`짜리 순열
              - 👉{a, c, b, d} 에서 `b`와 `b`끼리 스왑 👉🏻 {`a`, `c`, `b`, d}
                - **depth = 3**
                  - <u>depth = 3 가 되었으므로 인덱스 2 까지 a c b 를 출력 한다.</u>
                  - 다시 한단계 이전 정렬로 되돌리기 {a, c, b, d}
            - 2️⃣ 첫 원소가 `acd`로 시작하는 {b} 의 모든 `1P0`짜리 순열
              - {a, c, b, d} 에서 `b`와 `d`끼리 스왑 👉🏻 {`a`, `c`, `d`, b}
                - **depth = 3**
                  - <u>depth = 3 가 되었으므로 인덱스 2 까지 a c d 를 출력 한다.</u>
                  - 다시 한단계 이전 정렬로 되돌리기 {a, c, b, d}
            - 다시 한단계 이전 정렬로 되돌리기 {a, b, c, d}
    - 2️⃣ 첫 원소가 `b`로 시작하는 {a, c, d} 의 모든 `3P2`짜리 순열
      - 👉 {a, b, c, d} 에서 `a`와 `b`끼리 스왑 👉🏻 {`b`, a, c, d}
    - 3️⃣ 첫 원소가 `c`로 시작하는 {b, a, d} 의 모든 `3P2`짜리 순열
      - 👉 {a, b, c, d} 에서 `a`와 `c`끼리 스왑 👉🏻 {`c`, b, a, d}
    - 4️⃣ 첫 원소가 `d`로 시작하는 {b, c, a} 의 모든 `3P2`짜리 순열
      - 👉 {a, b, c, d} 에서 `a`와 `d`끼리 스왑 👉🏻 {`d`, b, c, a}

<br>

## STL : next_permutation

<br>

## 참고 문제

- [프로그래머스 (소수 찾기)](https://ansohxxn.github.io/programmers/kit19/)

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}