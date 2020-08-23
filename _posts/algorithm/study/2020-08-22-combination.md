---
title:  "(C++) 조합(Combination) 구현하기" 

categories:
  - Algorithm
tags:
  - [Algorithm, Coding Test, Cpp, Recursion, STL]

toc: true
toc_sticky: true

date: 2020-08-22
last_modified_at: 2020-08-22
---

## 조합이란

- 순서를 따지지 않는다.
  - `abc`와 `acb`는 같은 존재다.
- 중복을 허용하지 않는다.
- `nCr`
  - 5C3 = 5P3 / 3! = (5 X 4 X 3) / (3 X 2 X 1)

<br>

### 조합 경우의 수 구하기 nCr

`nCr = n-1Cr-1 + n-1Cr` 공식을 사용하여 재귀적으로 구하면 된다.

```cpp
int combination(int n, int r)
{
    if(n == r || r == 0) 
        return 1; 
    else 
        return combination(n - 1, r - 1) + combination(n - 1, r);
}
```
<br>

## 재귀로 구현한 코드 1

```cpp
#include <iostream>
#include <vector>

using namespace std;

void Combination(vector<char> arr, vector<char> comb, int r, int index, int depth)
{
    if (r == 0)
    {
        for(int i = 0; i < index; i++)
            cout << comb[i] << " ";
        cout << endl;
    }
    else if (depth == arr.size())  // depth == n
    {
        return;
    }
    else
    {
        comb[index] = arr[depth];
        Combination(arr, comb, r - 1, index + 1, depth + 1);
        
        Combination(arr, comb, r, index, depth + 1);
    }
}

int main()
{
    vector<char> vec = {'a', 'b', 'c', 'd', 'e'};  // n = 5
    
    int r = 3;
    vector<char> comb(r);
    
    Combination(vec, comb, r, 0, 0);  // {'a', 'b', 'c', 'd', 'e'}의 '5C3' 구하기 

    return 0;
}
```
```
💎출력💎

a b c
a b d
a b e
a c d
a c e
a d e
b c d
b c e
b d e
c d e
```

> 예시) `{'a', 'b', 'c', 'd', 'e'}` 벡터에서 `r = 3` 자릿수의 **조합**들 출력하기 👉 경우의 수 5C3 =10 

### 이 풀이의 원리

> **nCr = n-1Cr-1 + n-1Cr**

> <u>뽑은 경우</u>와 <u>뽑지 않은 경우로 나뉜다.

- `arr`에서 어떤 원소를 뽑은 경우
  ```cpp
  comb[index] = arr[depth];
  Combination(arr, comb, r - 1, index + 1, depth + 1);
  ```
- `arr`에서 어떤 원소를 뽑지 않기로 결정한 경우 
  ```cpp
  Combination(arr, comb, r, index, depth + 1);
  ```
- 매개 변수 소개
  - `arr`
    - `r`개를 뽑을 대상이 될 원소들이 모인 `n`크기의 벡터
    - `n`은 `arr.size()`로 구할 수 있고 재귀 할 때마다 변화를 겪는건 아니기 때문에 굳이 인수로 넘기지 않았다.
  - `comb` 
    - 하나의 조합이 완성되면 `r`크기를 가지는 벡터가 될 것이다. 처음엔 빈 벡터로 시작한다.
  - `r`
    - 재귀의 매 단계마다 그 단계 시점에서 `arr`중에서 <u>몇 개를 더 뽑아야 하는지</u>를 나타낼 수. 
      - 5C3 의 경우들을 구하는데 이미 원소 2개가 뽑여있는 상태라면 현재의 r 값은 1이 된다. 하나만 더 뽑으면 되니까.
    - `arr[depth]` 원소를 뽑는 경우에는 뽑아야 하는 수는 1 줄어드므로 `r - 1`을 다음 재귀에 인수로 넘긴다.
    - `arr[depth]` 원소를 뽑지 않은 경우에는 뽑아야 하는 수는 그대로 이므로 `r`을 그대로 다음 재귀에 인수로 넘긴다.
  - `index`
    - `arr`로부터 원소를 뽑을 때 뽑은 원소를 `comb`에 넣을 위치가 된다.
      - `comb`의 인덱스가 됨.
    - `arr[depth]` 원소를 뽑은 경우에는 `comb`의 `[index]` 인덱스에 이미 `arr[depth]`원소를 뽑아 넣었으므로 이젠 `comb`의 `[index + 1]` 인덱스에 뽑아 넣어야 한다. 따라서 다음 재귀에 `index + 1`을 넘긴다.
    - `arr[depth]` 원소를 뽑지 않은 경우에는 `comb`의 `[index]` 인덱스에 넣을 원소를 다음 재귀에서 결정해야 한다. 따라서 다음 재귀에 그대로 `index`을 넘긴다.
    - 중복을 허용하지 않으므로 순열과 달리 <u>이미 지나간 원소는 다시 쳐다도 보면 안된다.</u> ✨✨
      - <u>따라서 순열과 달리 매 재귀 단계마다 arr의 첫번째 원소(인덱스 0)부터 따지는 것이 아닌 매 단계마다 1씩 증가시키는 depth 자리를 검사한다.</u> 
  - `depth`
    - 뽑을 원소를 결정하기 위해 `arr`을 순회하는 인덱스.
    - ✨재귀의 깊이는 `arr`을 끝까지 순회하는 곳 까지이기 때문에 `arr`을 순회할 인덱스를 `depth`라고 명명했다.✨
      - ✨`arr`의 원소들을 하나하나씩 차례대로 살펴볼 때 재귀를 사용하여 살펴봄 ✨
    - 뽑는 경우든 뽑지 않는 경우든 어쨋든 `comb`에 결정해서 넣을 `arr`의 다음 원소를 살펴보아야 하기 때문에 언제나 `depth + 1`을 다음 재귀에 인수로 넘긴다.
- **if(r == 0)**
  - `depth`가 `arr.size()`(= n)에 도달하기 전에 `r`개만큼 다 뽑은 경우다. 즉, `comb`가 결정되어 조합의 한 경우가 결정되어 이제 출력해야 할 때.
  - 이때의 `index`는 `comb.size()`(원래의 시작 r값)과 같은 값을 가진다. `comb`를 다 채웠기 때문에! 
- **else if(depth == arr.size())**
  - `depth`가 `arr.size()`(= n)에 도달한 경우다. 
    - 즉, `arr`의 모든 원소를 순회했는데도 `r`이 0 이 되지 않은 경우이므로 조합의 경우들 중 하나가 될 수 없는 케이스라는 것을 뜻한다. 
      - 예를 들어 {a, b, c, d, e} 중에서 5C3을 구해야 하는데 a 는 뽑고 b c d e 는 뽑지 않기로 했다면, 즉 r = 2 두개를 더 뽑아야함에도 불구하고 이미 순회를 다 해버렸으니 a 는 뽑고 b c d e 는 뽑지 않는 경우는 철회해야 한다.
  - 정답 중 하나가 될 수 없으므로 되돌아가기 위해 <u>return 한다.</u>
    - base case
- **else**
  - 아직 더 뽑아야 하고 (r != 0), `arr` 中 아직 뽑을 후보들도 남아 있다면 (depth != arr.size())  
  - 현재 단계 (depth)에서 아래와 같이 두 케이스로 나눈다. 
    - `arr[depth]` 원소를 ✨뽑는 경우
    ```cpp
      comb[index] = arr[depth];  // 뽑음.
      Combination(arr, comb, r - 1, index + 1, depth + 1); // arr의 다음 원소를 comb[index + 1]자리에 따져보기 위해 출발
    ```
    - `arr[depth]`를 ✨뽑지 않는 경우 
    ```cpp
    Combination(arr, comb, r, index, depth + 1);  //  arr의 다음 원소를 뽑지 않았으니 그대로 comb[index]자리에 따져보기 위해 출발
    ```


<br>

## 재귀로 구현한 코드 2

```cpp
#include <iostream>
#include <vector>

using namespace std;

void Combination(vector<char> arr, vector<char> comb, int index, int depth)
{
    if (depth == comb.size())
    {
        for(int i = 0; i < index; i++)
            cout << comb[i] << " ";
        cout << endl;
        
        return;
    }
    else
    {
        for(int i = index; i < arr.size(); i++)
        {
            comb[depth] = arr[i];
            Combination(arr, comb, i + 1, depth + 1);
        }
    }
}

int main()
{
    vector<char> vec = {'a', 'b', 'c', 'd', 'e'};  // n = 5
    
    int r = 3;
    vector<char> comb(r);
    
    Combination(vec, comb, 0, 0);  // {'a', 'b', 'c', 'd', 'e'}의 '5C3' 구하기 

    return 0;
}
```
```
💎출력💎

a b c
a b d
a b e
a c d
a c e
a d e
b c d
b c e
b d e
c d e
```

> 예시) `{'a', 'b', 'c', 'd', 'e'}` 벡터에서 `r = 3` 자릿수의 **조합**들 출력하기 👉 경우의 수 5C3 =10 


### 이 풀이의 원리

> 이 코드는 [중복 순열](https://ansohxxn.github.io/algorithm/repeated-permutation/) 구현 코드와 비슷하다!

- 중복 순열
  - 매개변수
    ```cpp
    void repeatPermutation(vector<char> arr, vector<char> perm, int depth)
    ```
  - 재귀 부분
    ```cpp
    for(int i = 0; i < arr.size(); i++)
    {
        perm[depth] = arr[i];
        repeatPermutation(arr, perm, depth + 1);
    }
    ```
- 조합
  - 매개변수
    ```cpp
    void Combination(vector<char> arr, vector<char> comb, int index, int depth)
    ```
  - 재귀 부분
    ```cpp
    for(int i = index; i < arr.size(); i++)
    {
        comb[depth] = arr[i];
        Combination(arr, comb, i + 1, depth + 1);
    }
    ```

- 1 번 코드에서 `arr`을 순회하는 포인터를 `depth`로 명명했던 반면에 이 2번 코드에서의 `depth`는 `comb`의 자릿수만큼 재귀한다는 점에서 `comb`를 순회하는 포인터를 `depth`라고 명명했다.
  - 1 번 코드는 `arr` 순회를 재귀 기준으로 삼았었고
  - 2 번 코드는 `comb` 순회를 재귀 기준으로 삼는다.
    - 그리고 `comb`의 매 자리마다(매 재귀마다) for문으로 `arr` 원소들을 순회한다.
- 각 재귀 단계 (`arr` 원소가 `r`개 뽑혀 들어갈 `comb`의 각 자리. 인덱스)마다 <u>for문을 돌며 arr의 원소들을 대입해본다</u>
  - for문은 `arr`의 원소들 중 `comb`에 들어갈 수 있는 원소들을 순회한다.
    - **중복 순열**에서는 중복이 가능하므로 `comb`의 매 자리를 결정할 때마다(매 재귀마다) `arr`의 모든 원소가 후보가 될 수 있었다. 따라서 *for(<u>int i = 0</u>; i < arr.size(); i++)* 이렇게 0 인덱스부터 순회
    - **조합**에서는 중복이 가능하지 않고 순서도 따지지 않으므로 `arr` 원소들 중에서 <u>이미 한번 뽑았었거나 뽑지 않기로 결정하고 지나갔었던 원소라면 다시 comb에 넣는 경우와 넣지 않는 경우를 따질 필요가 없다.</u> 따라서 조합을 구하는 경우에는 `comb`의 매 자리를 결정할 때마다(매 재귀마다) `arr`에서 `comb`에 넣고 안 넣고를 <u>따졌었던 가장 최근의 원소의 그 다음 원소부터가 후보가 된다.</u> 따라서 *for(<u>int i = index</u>; i < arr.size(); i++)* 후보는 `index`부터이며 재귀할 때 `index + 1`을 인수로 넘겨주어야 한다. *Combination(arr, comb, <u>i + 1</u>, depth + 1);* 이 때문에 중복순열과 달리 `arr` 중에서 아직 안지나간 원소들 중 첫 번째 원소의 위치를 나타내는 `index`라는 매개 변수가 추가로 필요하다. 후보가 될 수 있는 원소의 범위가 매 재귀마다 바뀌므로 `index`로 범위의 시작 위치를 매번 갱신해 표시해주어야 하기 때문이다.

<br>

### 재귀로 구현한 코드 3  👉 2 응용


```cpp
#include <iostream>
#include <vector>

using namespace std;

void Combination(vector<pair<char, bool>> arr, vector<char> comb, int index, int depth)
{
    if (depth == comb.size())
    {
        for(int i = 0; i < arr.size(); i++)
            if (arr[i].second)
                cout << arr[i].first << " ";
        cout << endl;
        
        return;
    }
    else
    {
        for(int i = index; i < arr.size(); i++)
        {
            if(arr[i].second == true)
                continue;
            else
            {
                arr[i].second = true;
                comb[depth] = arr[i].first;
                Combination(arr, comb, i + 1, depth + 1);
                arr[i].second = false;
            }
        }
    }
}

int main()
{
    vector<char> vec = {'a', 'b', 'c', 'd', 'e'};  // n = 5
    vector<pair<char, bool>> check(vec.size());
    for(int i = 0; i < check.size(); i++)  // check는 vec의 원소들이 이미 comb 원소로 결정된 적이 있는지를 함께 나타내주는 컨테이너가 될 것이다.
        check[i] = make_pair(vec[i], false);  // false로 초기화
    
    int r = 3;
    vector<char> comb(r);
    
    Combination(check, comb, 0, 0);  // {'a', 'b', 'c', 'd', 'e'}의 '5C3' 구하기 

    return 0;
}
```
```
💎출력💎

a b c
a b d
a b e
a c d
a c e
a d e
b c d
b c e
b d e
c d e
```

> 예시) `{'a', 'b', 'c', 'd', 'e'}` 벡터에서 `r = 3` 자릿수의 **조합**들 출력하기 👉 경우의 수 5C3 =10 

- 2 번 코드와 사실 비슷하다. `comb`의 각 자리를 결정 하는 것을 재귀의 한 단계로 삼으며 매 단계마다 `comb`의 각 자리에 들어갈 후보는 중복 순열처럼 `arr`의 0 인덱스부터 시작한 모든 원소가 아닌`arr`중 이전에 `comb`에 넣을지 안넣을지 결정 한번 했었던 원소부터 끝까지를 후보로 삼는 것 또한 똑같다. (*for(<u>int i = index</u>; i < arr.size(); i++)*) 
- [순열 구현 2 번 코드](https://ansohxxn.github.io/algorithm/permutation/#%EC%9E%AC%EA%B7%80%EB%A1%9C-%EA%B5%AC%ED%98%84%ED%95%9C-%EC%BD%94%EB%93%9C-2)처럼 `comb`에 넣을지 안넣을지 결정 한번 했었던 `arr`원소인지를 bool 타입으로 저장할 수도 있지만 <u>사실 for문을 `comb`에 넣을지 안넣을지 결정 한적이 없었던 원소들 중 첫번째 위치를 나타내는 `index`부터를 후보로 정한다면 그 자체로 결정 안했었던 원소들만이 후보가 된다는 것을 의미하니 bool타입으로 체크하는 것은 사실 필요없는 과정이기는 하다.</u>
- 그래도 한번 구현해 보았다. 출력할 때 `arr`을 순회하며 출력하되 `comb`에 넣기로 결정했었다는 것을 의미하는 bool타입의 second 값이 true 값을 가지는 `arr` 원소만 출력하는데에 써먹었다.
- 재밌는 점은 *for(<u>int i = 0</u>; i < arr.size(); i++)* `comb`의 각 자리를 매번 결정할때마다 `arr`의 모든 원소로 한다는 것을 의미하므로 이렇게 `i = 0`으로만 바꿔주어도 순열을 구하는 것과 같아진다!
  - 매번 모든 원소가 후보가 된다는 것은 순서를 고려한다는 말과도 같다. 순열은 `arr`원소가 `comb`의 원소로 결정만 안됐었으면 될 뿐, 안뽑힌 것이라도 다음 후보가 될 수 있기 때문이다.

<br>

## STL: prev_permutation으로 조합 구현하기

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
 
int main() {
    const int r = 2;
    
    vector<char> arr{'b', 'a', 'd', 'c'};
    vector<bool> temp(arr.size());
    for(int i = 0; i < r; i ++)
        temp[i] = true;
 
    do {
        for (int i = 0; i < arr.size(); ++i) {
            if (temp[i])
                cout << arr[i] << ' ';
        }
        cout << endl;
    } while (prev_permutation(temp.begin(), temp.end()));
}

```
```
💎출력💎

b a
b d
b c
a d
a c
d c
```

- `prev_permutation`의 특징 
  - 모든 순열을 구하려면 <u>내림 차순 정렬이 되어 있어야 한다.</u>
  -  <u>중복이 있는 원소들은 제외하고 순열을 만들어준다.</u>
  - 자세한 설명은 [순열 포스트](https://ansohxxn.github.io/algorithm/permutation/#prev_permutation) 참고

예를 들어 {'b', 'a', 'd', 'c'}의 `4C2` 조합들을 출력하려면 `r = 2` 개수 만큼의 `true` 값 원소를 가지고 {'b', 'a', 'd', 'c'}와 사이즈가 같은 bool타입의 `temp` 벡터를 선언한다. `temp`의 초기 값은 `r = 2`개의 `true` 원소를 맨 앞으로 보낸 것에서 시작한다. 이렇게 {true, true, false, false} 가 되는데 이는 `temp`를 내림 차순 정렬할 때 가장 처음으로 올 값이다. 즉 반대로 말하면 오름 차순 정렬시 가장 큰 값. 여기서 시작하여 <u>temp 를 대상으로 prev_permutation 실행한다.</u>

```
{true, true, false, false}
{true, false, true, false}
{true, false, false, true}
{false, true, true, false}
{false, true, false, true}
{false, false, true, true}
```

위와 같이 중복을 제외하고 내림 차순으로 정렬이 된다. 매번 `temp`의 이전 순열을 구한 후 `temp`의 `true`인 자리에 일치하는 인덱스를 가진 `arr`의 원소만을 뽑는다! 현재 순열이 `{true, false, false, true}` 이라면 `arr[0]`, `arr[3]`인 'b'와 'c'를 출력한다.

<br>

## 참고한 블로그

- <https://mjmjmj98.tistory.com/38>
- <https://gorakgarak.tistory.com/523>
- <https://codemcd.github.io/algorithm/Algorithm-%EC%88%9C%EC%97%B4%EA%B3%BC-%EC%A1%B0%ED%95%A9/#%EA%B5%AC%ED%98%84-%EC%BD%94%EB%93%9C>
- <https://yabmoons.tistory.com/99?category=838490>

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}