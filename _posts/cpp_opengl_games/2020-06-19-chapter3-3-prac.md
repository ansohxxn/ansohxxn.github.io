---
title:  "[C++] 3.3 연습 문제 풀이" 

categories:
  - C++ games
tags:
  - [Programming, Cpp, OpenGL, Graphics, Design Pattern]

toc: true
toc_sticky: true

date: 2020-06-19
last_modified_at: 2020-06-19
---

인프런에 있는 홍정모 교수님의 **홍정모의 게임 만들기 연습 문제 패키지** 강의를 듣고 정리한 필기입니다.😀   
[🌜 공부에 사용된 홍정모 교수님의 코드들 보러가기](https://github.com/jmhong-simulation/GameDevPracticePackage)   
[🌜 [홍정모의 게임 만들기 연습 문제 패키지] 강의 들으러 가기!](https://www.inflearn.com/course/c-2)
{: .notice--warning}

<br>

# 3.2 연습문제

**연습 문제**는 스스로 풀이했습니다. 😀       
해당 챕터 보러가기 🖐 [3.3 질량-용수철 시스템](https://ansohxxn.github.io/c++%20games/chapter3-3/)   
연습 문제 출처 : [홍정모 교수님 블로그](https://blog.naver.com/atelierjpro/221413483005)
{: .notice--warning}

<br>

## 🙋 Q1. 스프링과 물체 하나 더 추가해보기

> 🟡*rb0*은 고정이며 🔵*rb1*,🔵*rb1* 질점이 2개 있는 형태

![image](https://user-images.githubusercontent.com/42318591/85251341-1218dc00-b494-11ea-9243-84b01896a85b.png){: width="30%" height="40%"}{: .align-center}

### 전체 코드

```cpp
#include "Game2D.h"
#include "Examples/PrimitivesGallery.h"
#include "RandomNumberGenerator.h"
#include "RigidCircle.h"
#include <vector>
#include <memory>

namespace jm
{
	class Example : public Game2D
	{
	public:
		RigidCircle rb0, rb1, rb2;

		Example()
			: Game2D()
		{
			reset();
		}

		void reset()
		{
			// Initial position and velocity
			rb0.pos = vec2(0.0f, 0.5f);
			rb0.vel = vec2(0.0f, 0.0f);
			rb0.color = Colors::hotpink;
			rb0.radius = 0.03f;
			rb0.mass = 1.0f;

			rb1.pos = vec2(0.5f, 0.5f);
			rb1.vel = vec2(0.0f, 0.0f);
			rb1.color = Colors::yellow;
			rb1.radius = 0.03f;
			rb1.mass = rb0.mass * std::pow(rb1.radius / rb0.radius, 2.0f);

			rb2.pos = vec2(0.8f, 0.5f);
			rb2.vel = vec2(0.0f, 0.0f);
			rb2.color = Colors::green;
			rb2.radius = 0.03f;
			rb2.mass = rb0.mass * std::pow(rb1.radius / rb0.radius, 2.0f);
		}

		void drawWall()
		{
			setLineWidth(5.0f);
			drawLine(Colors::blue, { -1.0f, -1.0f }, Colors::blue, { 1.0f, -1.0f });
			drawLine(Colors::blue, { 1.0f, -1.0f }, Colors::blue, { 1.0f, 1.0f });
			drawLine(Colors::blue, { -1.0f, -1.0f }, Colors::blue, { -1.0f, 1.0f });
		}

		void update() override
		{
			const float dt = getTimeStep() * 0.4f;
			const float epsilon = 0.5f;

			// physics update (Temporarily disabled)
			//rb0.update(dt);
			//rb1.update(dt);

			// coefficients
			const vec2 gravity(0.0f, -9.8f);
			const float coeff_k = 50.0f;
			const float coeff_d = 10.0f;

			// update rb1 (Note: rb0 is fixed)
			{
				const float l0 = 0.3f;

				// rb0-rb1 spring
				const auto distance = (rb1.pos - rb0.pos).getMagnitude();
				const auto direction = (rb1.pos - rb0.pos) / distance;// unit vector

				// compute stiffness force
				const auto spring_force = direction * -(distance - l0) * coeff_k +
					direction * -(rb1.vel - rb0.vel).getDotProduct(direction) * coeff_d;

				// compute damping force

				const auto accel = gravity + spring_force / rb1.mass;

				rb1.vel += accel * dt;

				// to 'reaction' to rb0 because rb0 is fixed.
			}

			// update rb2
			{
				const float l0 = 0.2f;

				// rb1-rb2 spring
				const auto distance = (rb2.pos - rb1.pos).getMagnitude();
				const auto direction = (rb2.pos - rb1.pos) / distance; // unit vector

				// compute stiffness force
				const auto spring_force = direction * -(distance - l0) * coeff_k +
					direction * -(rb2.vel - rb1.vel).getDotProduct(direction) * coeff_d;

				// compute damping force

				const auto accel = gravity + spring_force / rb2.mass;

				rb2.vel += accel * dt;
				rb1.vel -= spring_force / rb1.mass * dt; // reaction
			}

			// update positions
			rb1.pos += rb1.vel * dt;
			rb2.pos += rb2.vel * dt;

			// draw
			drawWall();

			// spring
			drawLine(Colors::red, rb0.pos, Colors::red, rb1.pos);
			drawLine(Colors::red, rb1.pos, Colors::red, rb2.pos);

			// mass points
			rb0.draw();
			rb1.draw();
			rb2.draw();

			// reset button
			if (isKeyPressedAndReleased(GLFW_KEY_R)) reset();
		}

	};
}

int main(void)
{
	jm::Example().run();

	return 0;
}
```

### 🟡*rb0* 👉🏻 🔵*rb1* 속도 업데이트 (가속도)

```cpp
{
	const float l0 = 0.3f;

	// rb0-rb1 spring
	const auto distance = (rb1.pos - rb0.pos).getMagnitude();
	const auto direction = (rb1.pos - rb0.pos) / distance;// unit vector

	// compute stiffness force
	const auto spring_force = direction * -(distance - l0) * coeff_k + direction * -(rb1.vel - rb0.vel).getDotProduct(direction) * coeff_d;

	// compute damping force

	const auto accel = gravity + spring_force / rb1.mass;

	rb1.vel += accel * dt;

	// to 'reaction' to rb0 because rb0 is fixed.
}
```

- 🟡*rb0*은 고정이므로 속도를 업데이트 하지 않는다. 
- 가속도 = 중력가속도 + 스프링의총힘/질량
- *const float l0 = 0.3f;*
  - 🟡*rb0* 👉🏻 🔵*rb1* 의 고정 길이
- distance :  l rb1.pos - rb0.pos l
- direction : ***rb0 → rb1***
- `rb1의 가속도`
  - `accel` = gravity + spring_force / rb1.mass;
    1. 중력가속도
    2. spring_force / rb1의 질량
      - rb1이 spring_force로부터 받을 가속도

### 🔵*rb1* 👉🏻 🔵*rb2* 속도 업데이트 (가속도)

```cpp
{
	const float l0 = 0.2f; // 원래 길이

	// rb1-rb2 spring
	const auto distance = (rb2.pos - rb1.pos).getMagnitude();
	const auto direction = (rb2.pos - rb1.pos) / distance; // unit vector

	// compute stiffness force
	const auto spring_force = direction * -(distance - l0) * coeff_k + direction * -(rb2.vel - rb1.vel).getDotProduct(direction) * coeff_d;

	// compute damping force

	const auto accel = gravity + spring_force / rb2.mass;

	rb2.vel += accel * dt;
	rb1.vel -= spring_force / rb1.mass * dt; // reaction
}
```

- 🔵*rb2* 의 `가속도`
  ```cpp
  const auto accel = gravity + spring_force / rb2.mass;
  rb2.vel += accel * dt;
  ```
  - distance : l rb2.pos - rb1.pos l
  - direction : ***rb1 → rb2***
  - `accel` = gravity + spring_force / rb2.mass*dt;
    1. 중력가속도
    2. rb2이 spring_force로부터 받을 가속도
      - spring_force / rb2의 질량
      - rb1 → rb2
      - rb1 입장에서의 rb2의 상대속도 : ***rb1.vel - rb0.vel***
        
- 🔵*rb1* 의 `가속도`
  ```cpp
  rb1.vel -= spring_force / rb1.mass * dt; // reaction
  ```
  - rb1.vel의 중력 가속도는 🟡*rb0*👉🏻🔵*rb1* 에서 반영 했으니까 언급 X
  - 🔵*rb1*👉🏻🔵*rb2* 의 스프링이 원래대로 돌아가는 힘은 🟡*rb0*👉🏻🔵*rb1*의 스프링이 원래대로 돌아가는 힘에 영향을 받은 🔵*rb1*의 <u>가속도를 줄인다.</u>
      - rb1.vel `-=` spring_force / rb1.mass * dt; 
      - <u>그래서 이렇게 빼주어야 한다.</u>
    - 이말은 즉 rb1의 속도 변화 폭이 점점 줄어든다는 것.
      - ~~이 부분 이해가 빠삭하게 되는건 아님.. 아리까리 😥😭~~

***
<br>

    🌜 개인 공부 기록용 블로그입니다. 오류나 틀린 부분이 있을 경우 
    언제든지 댓글 혹은 메일로 지적해주시면 감사하겠습니다! 😄

[맨 위로 이동하기](#){: .btn .btn--primary }{: .align-right}