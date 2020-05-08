# TR Reusable UI System - 작성중

## 무엇을 하려고 하는가?

* 해설문서\(Explanation\)에서의 설명이 부족했던 부분을 보완하는 문서입니다.
* 함수 및 의문점에 대한 문서가 중점입니다.
* 이 문서는 상업적으로 이용하지 않습니다.

## GameObject vs Component?

{% tabs %}
{% tab title="GameObject와 Component의 개념" %}
GameObject : 유니티 씬\(scene\)에서 전체 엔티티\(entity\)의 기본 클래스를 나타냅니다.

Component : GameObject에 첨부된 모든 Component의 기본 클래스를 나타냅니다.
{% endtab %}

{% tab title="왜 이렇게 비교하게 되었는가?" %}
**Unity는 Component pattern을 가지고 있습니다**. 정확히는 가장 핵심적인 GameObject Class는 Component pattern을 가지고 있습니다.

Inspector에서 어떤 기능을 추가할 때 Component를 추가한다고 하는데 이러한 pattern을 가지고 있기 때문에 유연성을 가지고 어떠한 Component라도 추가할 수 있습니다.

이러한 장점을 기반하여 Component를 가져올 수 있다면 어떤 것을 어떻게 가져오는게 좋은가?에 대한 의문점에 이 문서를 작성하게 되었습니다.



* Component Pattern을 가지고는 있는데, Pattern이 가진 장단점은 어떤것인가?
  * 장 : Class로 분리하여 가독성\(Readability\)을 늘리고, 결합도\(Coupling\)을 줄입니다.
  * 단 : 확장성이 좋진 않고, 각 Component간에 통신의 비용이 높습니다.



* 핵심적인 GameObject Class에 Component Pattern을 가졌다면, Script를 작성하는데 이에 대한 정보를 가져와야하는데, 어떠한 방식으로, 어떤 것을  가져오는가?
  * Unity 자체에서는 GetComponent를 가지고 Component에 Access 할수 있도록 합니다.
  * 혹은 Access할 변수를 선언 후 변수를 통해 Access합니다.
  * 왠만한 Component로 추가할 수 있는 모든 것들을 가져올 수 있습니다.



* GetComponent, 변수선얼을 통한 Access 활용방법은 어떤것이 있는가?
  * Unity Document를 살펴본다면 Public Function을 통해 확인 할 수 있습니다.
  * 대부분의 Component를 조작할 때 사용됩니다. 이에 대한 상황은 여러 프로젝트를 하면서 익히는 게 최선이지만 제가 느꼈던 필요한 상황을 아래에 기재 했습니다.
    * 1. 같은 Object의 Component에 접근할 때
         1. GetComponent를 통해 접근합니다.
            1. GetComponent, GetComponentsInChildren, GetComponentsInParent
      2. Hierarchy상의 현재 Object가 아닌 다른 Object에 접근하고 싶을 때
         1. Class 변수 선언을 통해 접근합니다.
            1. ClassType VariableName;
      3. Message를 통해 Console에 알리고 싶을 때
         1. BroadcastMessage, SendMessage를 통해 접근합니다.

{% embed url="https://docs.unity3d.com/kr/530/ScriptReference/Component.html" %}
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



## Coroutine을 사용한다는 것

{% tabs %}
{% tab title="Coroutine과 Update의 간단한 개념" %}
Coroutine : 특정 함수\(IEnumerator type Function\)를 호출하면 수행할 내용을 완수하고 반환\(yield\) 시킨다음 함수가 호출된 시점으로 돌아가서 다른 Code를 실행시킵니다.

{% embed url="https://docs.unity3d.com/kr/530/Manual/Coroutines.html" %}

Update : 매 프레임마다 호출되는 함수입니다. 

{% embed url="https://docs.unity3d.com/kr/530/ScriptReference/MonoBehaviour.Update.html" %}
{% endtab %}

{% tab title="왜 이렇게 비교하게 되었는가?" %}
보통 빈번하게 사용하 Update문은 Message System을 가졌기 때문에 Unity Event Life Cycle에서는 보통 Message System을 통해 함수간 통신을 하기 때문에 굉장히 많은 비용이 듭니다.

{% embed url="https://blogs.unity3d.com/kr/2015/12/23/1k-update-calls/" %}

{% embed url="https://hrmrzizon.github.io/2017/04/07/unity-message-system/" %}

그리고 이러한 Message System을
{% endtab %}

{% tab title="Coroutine vs Update" %}
C
{% endtab %}
{% endtabs %}





