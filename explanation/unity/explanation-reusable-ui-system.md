---
description: Explanation Reusable UI System
---

# Explanation Reusable UI System - 작성중

## 무엇을 하려고 하는가?

* Reusable UI System Project의 해설문서를 통해 Code의 흐름을 기재합니다.
* 프로그램을 실행하여 조작하는데 있어서 크게 2가지로 나눴습니다.
  * Click
  * Switch
* Click Event는 Button Script에 등록하기만 하면 작동하기 때문에, Switch라는 기능이 어느 부분까지 적용되는가에 대한 설명문서를 작성하겠습니다.
* 영상 강의에서 Script의 작성 순서에 따라 해설문서에 기재하도록 하겠습니다.



{% tabs %}
{% tab title="Screen을 가져오기" %}
## IP\_UI\_System\(Screen을 가져오\)

* 화면의 전환\(SwitchScreen\)에 있어서 핵심 Script입니다. 
* 해당 Script에서 Screen Class 변수를 받아서 받은 변수타입을 가진 모든 Object를 조사한 다음 배열에 저장하도록 합니다.

{% code title="IP\_UI\_System.cs" %}
```csharp
public Component[] screens = new Component[0];

screens = GetComponentsInChildren<IP_UI_Screen>(true);
```
{% endcode %}

* 이 부분에서 몇가지 의문점이 들 수 있습니다.
  * 왜 GameObject가 아닌 Component type인가?
    * 공통적으로 들어가는 Component중에 어떤 것이던지 상관없지만 Shortcut code를 위한 방법입니다.
  * 왜 배열로 받는것 인가?
    * 각 Screen은 개별적으로 존재하고, Screen이 여러개 존재하기 때문입니다. 
  * 선언한 type을 가져오기 위한 GetComponentsInChildren은 무엇인가?
    * GetComponentsInChildren\(\) 함수는 해당 Object의 Child Object의 모든 Component를 가져오는 함수입니다.
{% endtab %}

{% tab title="SwitchScreen\(\) 함수로 Screen 전환하기" %}
* 해당 프로젝트에는 Screen Object가 총 3개의 IP\_UI\_Screen 
{% endtab %}
{% endtabs %}

