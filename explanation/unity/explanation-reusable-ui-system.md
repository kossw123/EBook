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

## IP\_UI\_System

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
    * GameObject는 Scene에서 전체 Entity의  Base Class이고, Component
  * 왜 배열로 받는것 인가?
  * 선언한 type을 가져오기 위한 

