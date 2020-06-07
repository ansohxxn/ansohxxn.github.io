---
title:  "C++ Chapter 7.2 : 인수 전달 (값에 의한, 참조에 의한, 주소에 의한)" 

categories:
  - C++
tags:
  - [Programming, C++]

toc: true
toc_sticky: true

date: 2020-06-07
last_modified_at: 2020-06-07
---

인프런의 **<u>홍정모의 따라 하며 배우는 C++</u>** 강의를 듣고 정리한 필기입니다. 😀
{: .notice--warning}

# chapter 7. 함수

## 값에 의한 인수 전달 

```cpp
void func(int x) 
{
    ...
}
int main()
{
    int a = 10;
    func(a); 
}
```
내부적으로 `int x = a` 가 실행된다. 함수의 지역변수이자 매개변수인 int x 에 a의 값이 <u>복사된다.</u>
- 인수의 값이 복사되어 매개변수에게 넘겨지는 것을 <u>값에 의한 인수 전달 (call by value) 라고 한다.</u>

<br>

## 참조에 의한 인수 전달



***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}