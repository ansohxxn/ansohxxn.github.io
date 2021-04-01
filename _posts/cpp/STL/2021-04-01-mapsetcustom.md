---
title:  "[STL 컨테이너] set, unordered_set (+ map)에 사용자 정의 구조체 혹은 객체 담기" 

categories:
  - STL
tags:
  - [Data Structure, Coding Test, Cpp, STL]

toc: true
toc_sticky: true

date: 2021-04-01
last_modified_at: 2021-04-01
---

## 🚀 map, set, unordered_map, unordered_set 자세한 설명 

- [[STL 컨테이너] map & unordered_map & multimap](https://ansohxxn.github.io/stl/map/)
- [[STL 컨테이너] set & unordered_set & multiset](https://ansohxxn.github.io/stl/set/)
- [[STL 컨테이너] map 과 set의 정렬](https://ansohxxn.github.io/stl/sortmapset/)

<br>

## 🚀 `map`, `set` 에서 커스텀 구조체 or 객체 저장하기 (👉 정렬된 순서를 유지)

`map`과 `set`은 정렬된 상태를 유지하기 때문에 `int` 같은 기본 자료형이 아닌, 우리가 직접 만든 구조체나 클래스와 같은 **'사용자 지정 자료형'을 저장하려할 때는 <u>operator < 연산자를 const 함수로 오버로딩 해주어야 한다.</u>**(뒤에 `const` 붙여주어야 한다.) 즉, 같은 타입의 인스턴스끼리 크기를 비교할 수 있는 어떤 비교 기준을 마련해주어야 하는 것이다.

- `set` 👉 원소의 타입에 대한 비교기준
- `map` 👉 Key 의 타입에 대한 비교 기준 (map은 Key 를 기준으로 정렬한다. 즉, Key 가 사용자 지정 자료형일 때)

<br>

### 🔥 첫 번째 방법 : 멤버 함수로서 < 연산자 오버로딩 정의 

구조체 혹은 클래스 내부에서 멤버 함수로서 `<` 연산자를 오버로딩하여 구조체 혹은 클래스 자체에 비교 기준을 부여한다. 

#### ✈ `set`

```cpp
struct Student {
    string name;
    int grade;

    // Student 구조체들은 name을 비교 기준으로 정렬하고자 한다. name 이 같다면 grade 로 비교하기로 약속함!
    // 여기서 name 은 Student 구조체 인스턴스 입장에서 나의 name
    // 여기서 other.name 은 비교 대상이 되는 다른 Student 구조체 인스턴스의 name (other은 비교 대상이 되는 다른 Student 구조체)
    bool operator < (const Student& other) const {
        if (name == other.name)
            return grade < other.grade;
        return name < other.name; 
    }
};

int main()
{
    set<Student> students;
    
    students.insert({ "ryan", 5 });
    students.insert({ "muzi", 3 });
    students.insert({ "muzi", 1 });
    students.insert({ "apeach", 1 });
    students.insert({ "apeach", 1 }); // set은 중복을 허용하지 않으므로 이건 삽입되지 않는다.

    // 출력을 통해 name 을 기준으로(name이 같다면 grade로) 정렬된 모습 확인 가능!
    for (auto& stu : students)
        cout << stu.name << " : " << stu.grade << endl; 
}
```
```
💎출력💎

apeach : 1
muzi : 1
muzi : 3
ryan : 5
```


<br>

#### ✈ `map`

```cpp
struct Student {
    string name;
    int grade;

    bool operator < (const Student& other) const {
        if (name == other.name)
            return grade < other.grade;
        return name < other.name;
    }
};

int main()
{
    map<Student, string> students2;
    students2[{"prodo", 2}] = "착함";
    students2[{"con", 4}] = "노래를 잘함";
    for (auto& stu : students2)
        cout << stu.second << endl;
}
```
```
💎출력💎

노래를 잘함
착함
```


<br>

### 🔥 두 번째 방법 : 비교 함수 만들기 () 연산자

따로 비교 전용 구조체를 만들어 `()` 연산자를 오버로딩한 후 그 안에 <u>"두 인스턴스"의 비교 기준</u> 을 정의해준다. 그리고 `set`은 구체화시 두 번째 파라미터로, `map`은 세 번째 파라미터로 해당 비교 전용 구조체를 넘겨주면 된다. **이렇게 비교 함수를 두 파라미터를 받는 전역 함수 느낌으로 만들 경우, 구조체를 따로 만들어 이 안에다가 비교함수를 정의해준 후 STL 함수들에 이 구조체를 넘기는 식인듯 하다!** *sort* 함수도 그랬고..! 

#### ✈ set

```cpp
struct Student {
    string name;
    int grade;
};

// 구조체를 따로 만들어서 () 연산자를 오버로딩한 후 그 안에 비교 기준을 정의한다.
// 멤버 함수로서 내부에 저장한 것이 아니기 때문에 파라미터가 2 개가 필요하다. 같은 타입의 두 인스턴스 비교!
// stu1 가 참조하는 인스턴스와 stu2 가 참조하는 인스턴스의 비교
struct cmp {
    bool operator () (const Student& stu1, const Student& stu2) const {
        if (stu1.name == stu2.name)
            return stu1.grade < stu2.grade;
        return stu1.name < stu2.name;
    }
};

int main()
{
    set<Student, cmp> students; // 비교함수를 안에 정의해준 cmp 구조체를 함께 넘긴다.

    students.insert({ "ryan", 5 });
    students.insert({ "muzi", 3 });
    students.insert({ "muzi", 1 });
    students.insert({ "apeach", 1 });
    students.insert({ "apeach", 1 });

    for (auto& stu : students)
        cout << stu.name << " : " << stu.grade << endl;
}
```
```
💎출력💎

apeach : 1
muzi : 1
muzi : 3
ryan : 5
```

<br>

#### ✈ map

```cpp
struct Student {
    string name;
    int grade;
};

struct cmp {
    bool operator () (const Student& stu1, const Student& stu2) const {
        if (stu1.name == stu2.name)
            return stu1.grade < stu2.grade;
        return stu1.name < stu2.name;
    }
};

int main()
{
    map<Student, string, cmp> students2; // 비교함수를 안에 정의해준 cmp 구조체를 함께 넘긴다.
    students2[{"prodo", 2}] = "착함";
    students2[{"con", 4}] = "노래를 잘함";
    for (auto& stu : students2)
        cout << stu.second << endl;
}
```
```
💎출력💎

노래를 잘함
착함
```


## 🚀 `unordered_map`, `unordered_set` 에서 커스텀 구조체 or 객체 저장하기 (👉 정렬 안됨, 해시 함수 사용)

- 1️⃣ 해시 함수 만들기 
  - 해당 구조체 혹은 클래스 타입의 전용 해시 함수를 직접 만들어주어야 하는데
    - *C++ 에서 제공하는 기본 자료형에 대한 해시 함수*를 사용하여 해시값을 만들어내도 되고
    - *XOR 연산자* 를 사용하여 만드는 방법도 있다.
- 2️⃣ `==` 연산자 오버로딩이 필요하다. 
  - 해시 충돌(해시값 동일) 발생시 원소 비교가 가능해야하기 때문이다.
    - 두 인스턴스가 같다고 판단할 수 있는 그 기준이 필요하다.
  - 해시 함수 값으로 저장된 데이터를 찾는 `unordered_map`, `unordered_set`는 정렬이 필요 없으므로 `<` 연산자는 필요 없다.

<br>

### 1️⃣ 해시함수

```cpp
namespace std {
    template <>
    struct hash<Student> {
        size_t operator()(const Student& stu) const {
            hash<string> hash_func; // string 을 파라미터로 받아 해시 함수값을 리턴해주는 C++ 제공 해시 객체

            return hash_func(stu.name) ^ stu.grade; // 임의로 string 인 name 의 해시함수값과 int인 grade 값을 XOR 연산한 것을 해시 함수 값으로 하기로 정의!
        }
    };
}
```

- `namespace std` 안에 정의해야 한다.
- 구조체의 이름은 `hash<사용자정의타입이름>`
- `()` 연산자를 오버로딩한다.
  - 이 안에 해시 함수를 정의한다.
  - 해시 함수 값을 리턴하기에 리턴 타입은 `size_t` (unsigend_int)
- C++은 `int`, `string` 등등 기본 타입의 인스턴스를 파라미터로 받아 어떤 해시 함수 값을 생성해주는 그런 객체를 제공하고 있다. *ex) hash\<string> hash_func 👉 string 을 파라미터로 받아 해시 함수값을 리턴해주는 C++ 제공 해시 객체*
  - 이 해시 객체를 이용하여 구조체 혹은 클래스의 멤버들과 *XOR* 연산도 좀 섞어보고.. 잘 짬뽕하여.. 해시 함수를 만들어주면 된다.  

<br>

### 2️⃣ == 연산자 오버로딩

```cpp
struct Student {
    string name;
    int grade;

    bool operator == (const Student& other) const {
        if (name == other.name && grade == other.grade)
            return true;
        return false;
    }
};
```

<br>

### 3️⃣ 사용

```cpp
unordered_set<Student> students1;
unordered_map<Student, string> students2;
```

해시를 정의한 구조체 `hash`를 넘겨줄 필요는 없는듯 하다. 그냥 위와 같이 평소처럼 사용하면 된다!


<br>

## 🚀 출처 및 참고

윗 내용 모두 [모두의 코드](https://modoocode.com/224) 를 참고했습니다. :)

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}