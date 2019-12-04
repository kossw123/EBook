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

위에서 중력을 적용하고 움직임을 부여 했지만 그 움직임은 기본적으로 중력을 적용하기 위한 움직임이지 PlayerStart가 움직이기 위한 움직임은 아닙니다. PlayerStart가 움직이기 위해서는  여러 작업이 필요합니다. 그 중에 이 단락에서는 충돌체가 겹치는지 안겹치는지 확인하기 위한 작업을 합니다.

핵심적으로는 Rigidbody2D.cast라는 함수가 존재합니다. 이 함수를 이용하여 충돌체를 반환하고 결과를     int형 배열에 저장 하는 기능을 가진 함수인데 이것을 가지고 충돌을 감지하는 이 단락에서 핵심 기능입니다.

{% hint style="info" %}
int count = rb2d.Cast\(move, contactFilter, hitBuffer, distance + shellRadius\);

위와 같은 parameter를 가지고 그 결과를 int형 배열로 반환하기 때문에 int형 변수에                   저장합니다.
{% endhint %}

 이 기능을 사용하기 위해서는 RayCastHit2D, ContactFilter2D Component가 필요한데 각각 아래의 기능을 가지고 있습니다.

* RayCastHit2D : 2D physics에서 RayCast\(광선을 따라 놓여 있는 물체를 감지하기 위해 사용\)에 의해           감지된 물체에 대한 정보를 반환합니다.
* ContactFilter2D : 접촉한 물체에 대한 정보를 반환합니다. ContactFilter를 사용한다면 접촉 결과를     나중에 필터링 할 필요없이 더 빠르고 편리하게 사용 가능합니다.

 대충 흐름을 보자면 충돌을 감지하기 위해 RayCastHit2D로 PlayerStart에서 시작되는 광선을 따라 놓여있는 물체를 감지하고 ContactFilter2D로 결과를 미리 필터링하여 닿는 물체를 확인한다는 의미 인듯합니다.

## Scripting Collision

 위에서는 충돌을 감지하는 기능이 대부분 이였다면 여기서는 직접 충돌에 관한 처리를 Scripting을 합니다. RayCastHit2D List를 가지고 광선에 따라 놓여진 물체들을 List형식으로 관리합니다.

 그 후 move.magnitude를 반환하는 distance 변수를 가지고 움직인 Vector2의 길이를 알아낼 수 있습니다. 이 변수는 곧 움직인 거리가 되며 최소 minMoveDistance변수와 비교하여 조금이라도 움직였을 시 hitBufferList, 즉 놓여진 물체를 조사하는 List에 넣습니다.

{% hint style="info" %}
List&lt;RayCastHit2D&gt; hitBufferList = new List&lt;RayCastHit2D&gt;\(\);

위의 List는 RayCastHit2D가 반환하는 결과물들을 List형식으로 정리하여 탐색하기 위한 List
{% endhint %}

그 후 List에 들어간 결과물을 가지고 반복문을 돌려서 normal Vector를 뽑아냅니다. 이 normal Vector는    캐릭터를 표면에 세우거나 발사체의 반사, 도탄을 정렬하는 방법으로 유용하게 사용할 수 있습니다.               즉, normal Vector\(방향만 가지고 있는 벡터\)는 표면에 수직이면 양수값을 반환하기 때문에 표면에 수직인지 아닌지를 판별 할 수 있습니다.

표면인지 아닌지를 판별했다면 groundNormal = currentNormal를 하여 ground에 있다고 체크 후 currentNoraml.x = 0으로 함으로써 초기화 하는 과정을 거칩니다.   //이부분확

float projection = Vector2.Dot함수를 통해 Velocity와 currentNoraml의 내적을 구합니다.                               양수를 가리킨다면 정확히 같은 방향을 가지고 있기 때문에 따로 처리를 하진 않지만 다른 방향을 가리킨다면 예외 처리를 해줘야 합니다. 그렇기 때문에 velocity에 projection\(음수\) \* currentNormal\(음수\)를 하여 velocity에 더하여 \(x + a, y + a\)와 같은 과정을 통해 땅 위에 고정시켜 놓습니다.

////// 이 부분 상세설명

원점에서 충돌지점 까지의 거리구하여 shellRadius보다 가까운 지점에 충돌시  기존의 distance\(이동 길이\)를 수정합니다.

////// 이 부분 상세설명

## Horizontal Movement



## Player Controller Script





