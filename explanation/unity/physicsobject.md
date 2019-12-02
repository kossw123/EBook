---
description: How-to-guide PhysicsObject에 이은 Explanation
---

# Explanation PhysicsObject

## 무엇을 하려고 하는가?

* How-to-guide PhysicsObject에서는 코드의 흐름을 단계적으로 나눴다면 이 페이지 에서는 단계적으로 나눈 Code Block들에 대한 설명과 전반지식에 대한 설명을 합니다.
* 설명에 대한 함수 설명과 사용법들은 아래의 기술문서 링크를 타고 보실수 있습니다.

{% page-ref page="../../technical-reference/unity/tr-physicsobject.md" %}

## Scripting Gravity

PhysicsObject.cs는 물체에 중력을 적용함과 동시에 움직임을 부여하고, 충돌관련 기능들을                                    가지고 있습니다. 

우선적으로 중력을 적용시키기 위해 Object를 움직여야 하는데 RigidBody의 Position\(위치\)를 가지고 움직임을 부여 합니다.

움직임을 부여하기 위해서 Rigidbody2D Component에 접근을 해야하는데 이를 위해서는 GetComponent를 사용하여 해결할 수 있습니다.

{% hint style="info" %}
rb2d = GetComponent&lt;Rigidbody2D&gt;\(\);

위와 같이 해당 Script가 있는 Object의 Rigidbody2D에 접근하여 Position이나 다른 Component들을 조절하기 위해서 위와 같은 문법을 씁니다.

  
허나 이를 쓰기 위해서는 Start\(\), Awake\(\)나 Enable\(\)등 Update\(\)가 시작하기 전에 미리 쓴다고 선언을 해야합니다.
{% endhint %}

그리고 Rigidbody2D에 움직임을 부여할 Vector2변수를 생성하여 속도를 부여합니다. Velocity는 중력을 적용하기 위한 변수이고, Object의 움직임을 실질적으로 주기 위해서는 deltaPosition이라는 변수를 선언하여 Velocity라는 중력을 적용한 변수를 더한 새로운 Position값을 만들어서 Rigidbody에 부여합니다.

이 자료에서는 Movement라는 Method\(함수\)를 통해 따로 작성하고 그 함수를 FixedUpdate\(\)에서 돌아가게 끔 만들어 움직이고 있습니다.



## Detecting Overlaps

위에서 중력을 적용하고 움직임을 부여 했지만 그 움직임은 기본적으로 중력을 적용하기 위한 움직임이지 PlayerStart가 움직이기 위한 움직임은 아닙니다. ㅇ



