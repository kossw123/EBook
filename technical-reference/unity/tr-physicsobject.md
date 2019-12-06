---
description: Explanation PhysicsObject에 사용된 함수의 기능과 사용방법 등 함수에 관련된 기반들을 설명합니다.
---

# TR PhysicsObject

## 시작하기에 앞서 몇가지 주의해야 할 점들

* 해당 글에 대한 기술적인 문서들은 Document 혹은 타 기술Blog에서 가져온 것이며 이것을 가지고 어떠한 수입창출을 내지 않습니다.
* 일부 Document에 대한 글은 정보의 부족 혹은 원서 번역을 통해 나름대로 번역한것 입니다. 일부 곡해된 단어나 뜻이 들어갈 수 있기 때문에 이는 아래의 e-mail을 통해 오류를 범한 부분을 보내주시면 확인 후 수정작업에 들어가도록 하겠습니다.

## Scripting Gravity

{% embed url="https://docs.unity3d.com/ScriptReference/Rigidbody2D.Cast.html" caption="Rigidbody2D.Cast Document" %}

두 종류의 Rigidbody2D.cast 함수가 존재합니다. 각각 아래의 parameter를 가지고 있습니다.

1. public int Cast\([Vector2](https://docs.unity3d.com/ScriptReference/Vector2.html) direction, RaycastHit2D\[\] results, float distance = Mathf.Infinity\);
2. public int Cast\([Vector2](https://docs.unity3d.com/ScriptReference/Vector2.html) direction, [ContactFilter2D](https://docs.unity3d.com/ScriptReference/ContactFilter2D.html) contactFilter, List&lt;RaycastHit2D&gt; results, float distance = Mathf.Infinity\);

{% tabs %}
{% tab title="first Tab" %}
All the [Collider2D](https://docs.unity3d.com/ScriptReference/Collider2D.html) shapes attached to the [Rigidbody2D](https://docs.unity3d.com/ScriptReference/Rigidbody2D.html) are cast into the Scene starting at each Collider position ignoring the Colliders attached to the same [Rigidbody2D](https://docs.unity3d.com/ScriptReference/Rigidbody2D.html).

**Rigidbody2D에 부착된 모든 Collider2D 형상은 Scene이 시작될 때 동일한 Rigidbody2D에 부착된 충돌체를 무시하고 각 충돌체 위치에서 만들어집니다.**

This function will take all the [Collider2D](https://docs.unity3d.com/ScriptReference/Collider2D.html) shapes attached to the [Rigidbody2D](https://docs.unity3d.com/ScriptReference/Rigidbody2D.html) and cast them into the Scene starting at the Collider position in the specified `direction` for an optional `distance` and return the results in the provided `results` array.

**이 함수의 기능은 Rigidbody2D에 부착된 모든 Collider2D의 모양을 가져와서 선택적인 distance변수, 지정된 direction변수 방향으로 Collider위치에서 시작하여 Scene에 Cast하고 제공된 결과배열로 result를 반환합니다.**  
  
The integer return value is the number of results written into the `results` array. The results array will not be resized if it doesn't contain enough elements to report all the results. The significance of this is that no memory is allocated for the results and so garbage collection performance is improved when casts are performed frequently.

**정수 반환 값은 result array에 기록된 결과의 수입니다. 모든 결과를 보고하기에 충분한 요소가 포함되어 있지 않으면 result array의 크기가 조정되지 않습니다. 그 중요성은 결과에 메모리가 할당되지 않기 때문에 자주 cast를 수행할 때 가비지 수집 성능이 향상된다는 점에 있습니다.**

  
Additionally, this will also detect other Collider\(s\) overlapping the collider start position. In this case the cast shape will be starting inside the Collider and may not intersect the Collider surface. This means that the collision normal cannot be calculated in which case the collision normal returned is set to the inverse of the `direction` vector being tested.

**또한, 이것은 Collider 시작 위치와 겹치는 다른 Collider도 탐지합니다. 이 경우 cast shape은 충돌기 내부에서 시작되고 Collider 표면을 교차하지 않을 수 있다. 즉, 충돌 정상값이 시험 중인 방향 벡터의 역방향으로 설정된 경우 충돌 정상값을 계산할 수 없다.**
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

**즉, Object의 Rigidbody에 붙은 Collider의 모양을 가져와서 설정한 방향으로 Collider 위치에서 일정한 거리만큼의 RayCast, 광선을 쏴서 걸리는 물체들을 배열로 반환한다는 의미인듯 합니다. 충돌을 감지하거나, 충돌에 대한 어떤 처리를 위해 필요한 함수 인듯합니다.**



## Detecting Overlaps

 ContactFilter2D, RayCastHit2D, 

## Scripting Collision

Vector에 관한 전반적인 내용, normalVector, Vector2.Dot

## Horizontal Movement

moveAlongGround을 왜쓰는가? 2D 평면상에서의 x,y축 Swap에 대한 의미와 사용법

