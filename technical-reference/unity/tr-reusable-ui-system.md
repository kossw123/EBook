# TR Reusable UI System - 작성중

## 무엇을 하려고 하는가?

* 해설문서\(Explanation\)에서의 설명이 부족했던 부분을 보완하는 문서입니다.
* 함수 및 의문점에 대한 문서가 중점입니다.

## GameObject vs Component?

{% tabs %}
{% tab title="GameObject와 Component의 개념" %}
GameObject : 유니티 씬\(scene\)에서 전체 엔티티\(entity\)의 기본 클래스를 나타냅니다.

Component : GameObject에 첨부된 모든 Component의 기본 클래스를 나타냅니다.
{% endtab %}

{% tab title="두 개체 간 차이점" %}
이에 대한 차이는 아래와 같습니다.

* GameObjects are all the same type.
* Components come in many types.
* GameObjects contain \(instances of\) components.
* Every GameObject contains a Transform component.
* Every component can access its gameObject.
* Every component also get convenience accessors \(properties\) that retrieve the corresponding properties of its gameObject, including .transform, as you have observed.



* GameObject는 모두 같은 타입을 가집니다.
* Components는 많은 타입을 제공합니다.
* GameObject는 Component를 포함하고 있습니다.
  * 모든 GameObject는 Transform Component를 포함하고 있습니다.
* 모든 Component 또한 Transform을 포함한 GameObject에 접근하는 편의성 접근자\(convenience accessors\) Properties가 존재합니다. 

{% embed url="https://answers.unity.com/questions/42472/whats-the-difference-between-the-component-and-gam.html" caption="\\" %}

결론적으로 GameObject Class로 접근하는 방법과, GetComponent를 사용하여 Component에 접근하는 방법이 같습니다. 어떤것을 쓰냐에 따라 Shortcut code를 작성할 수도 있고, Component 내부의 Component를 생성하여 return 받을 수 있습니다.

즉, GameObject를 하나의 박스라고 생각하고, Component는 박스 내용물의 하나의 구성품이라고 생각하면 됩니다.
{% endtab %}

{% tab title="해당 프로젝트에서는 왜" %}
해당 프로젝트에서는 왜 Component 배열 변수를 사용하여 IP\_UI\_Screen type을 return 받는가?

* IP\_UI\_Screen Script Component가 공통적으로 들어가 있기 때문입니다.
* GameObject를 받기에는 같은 타입을 가지기 때문에 Shortcut으로 작성 할 수 없기 때문입니다.

```text
GameObject[] screen = new GameObject[0];

screen = gameObject.GetComponentsInChildren<IP_UI_System>(true);
```
{% endtab %}

{% tab title="결론" %}
Component, GameObject 둘중 하나만 받아도 상관이 없습니다. 해당 Project에서도 이것에 대한 중요성을 강조 하지 않고, 실제로도 중요하지 않을 수 있습니다.

하지만 메모리 관리 측면에 있어서 Instance를 생성하는데 Heap영역에 저장되기 때문에 GC가 어떠한 작용을 할 수 없기 때문에 GameObject가 아닌 Component type으로 접근하여 알수없는 Side Effect를 유발하는 경우 이러한 접근법이 좀 더 논리적이라고 생각되기 때문에 해당 항목을 작성하게 되었습니다.
{% endtab %}
{% endtabs %}







