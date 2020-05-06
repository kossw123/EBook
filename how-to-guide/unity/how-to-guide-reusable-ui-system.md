---
description: How-to-guide Reusable UI System
---

# How-to-guide Reusable UI System - 작성중

## 무엇을 하려고 하는가?

* Code에 대한 Review를 합니다. 
* 검토가 아닌 Review를 통해 Code의 동작원리를 설명합니다.
* 전체적인 Code에 대한 흐름이 아닌 Module 단위의 Code를 Review합니다.

## IP\_UI\_System.cs

{% tabs %}
{% tab title="Variable" %}
```csharp
#region Variables
[Header("Main Properties")]
public IP_UI_Screen m_StartScreen;
[Header("System Events")]
public UnityEvent onSwitchedScreen = new UnityEvent();

[Header("Fader Properties")]
public Image m_Fader;
public float m_FadeInDuration = 1f;
public float m_FadeOutDuration = 1f;

public Component[] screens = new Component[0];
private IP_UI_Screen previousScreen;
public IP_UI_Screen PreviousScreen { get { return previousScreen; } }
private IP_UI_Screen currentScreen;
public IP_UI_Screen CurrentScreen { get { return currentScreen; } }
```

* `m_StartScreen` : `IP_UI_Screen` Class 변수로써 최초의 Screen을 실행 할 때의 Data를 담습니다.
* `onSwitchedScreen` : UnityEvent System을 통해 Inspector에서 쉽게 Event를 추가할 수 있도록 합니다.
* `m_Fader` : Fade 효과를 위한 하나의 `Image` 변수로써, 이를 통해 Alpha값을 조정합니다.
  * `m_FadeInDuration`, `m_FadeOutDuration` : Fade 효과의 지속시간입니다.
* `screens` : Component 변수로써, 이를 배열변수로 선언하여 IP\_UI\_Screen가 존재하는 모든 Object를 가져옵니다.
* `previousScreen`, `currentScreen` : 이전 화면, 현재 화면을 담는 변수입니다. Property를 활용하여, 해당 변수를 가져오는 기능을 활성화 합니다.
{% endtab %}

{% tab title="Main Methods" %}
{% code title="IP\_UI\_System.cs" %}
```csharp
void Start() {
    screens = GetComponentsInChildren<IP_UI_Screen>(true);
    InitializeScreens();
    if(m_StartScreen) {
        SwitchScreen(m_StartScreen);
    }
    if(m_Fader) {
        m_Fader.gameObject.SetActive(true);
    }
    FadeIn();
}
```
{% endcode %}

* `Start()` : Unity Event Life Cycle 중에 `Start()` 함수를 사용하여 초기화에 이용합니다.
  * `screens` : `GetComponentsInChildren<type>(true)` 를 이용하여 &lt;type&gt; 붙어있는 모든 Object를 가져오고 true, false의 값에 따라 활성화, 비활성화된 Object의 Component도 포함할지 안할지 결정합니다.
  * `if(m_StartScreen)` : 분기문의 결과값에 따라 SwitchScreen을 하여 m\_StartScreen을 띄웁니다.
  * `if(m_Fader)` : Fade효과를 적용하는데 `Start()` 함수에서 적용함으로써 시작과 동시에 효과를 적용시킵니다.
{% endtab %}

{% tab title="Helper Methods" %}

{% endtab %}
{% endtabs %}





