---
description: How-to-guide Reusable UI System
---

# How-to-guide Reusable UI System

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
{% code title="IP\_UI\_System.cs" %}
```csharp
public void SwitchScreen(IP_UI_Screen screen) {
    if(screen) {
        if(currentScreen) {
            currentScreen.CloseScreen();
            previousScreen = currentScreen;
        }

        currentScreen = screen;
        currentScreen.gameObject.SetActive(true);
        currentScreen.StartScreen();

        if(onSwitchedScreen != null) {
            onSwitchedScreen.Invoke();
        }
    }
}
```
{% endcode %}

* `SwitchScreen(IP_UI_System screen)` : 화면 전환 함수입니다. 기본적으로 현재화면을 이전화면에 저장하고 다음 현재화면을 띄우는 방식입니다.
  * `if(screen)` : 기본적으로 screen parameter가 존재해야 하기 때문에 모든 Code를 if문으로 묶어서 Condition을 생성하고, Screen을 활성화 하고 `StartScreen()` 함수를 실행합니다.
    * `if(currentScreen)` : 최초로 프로그램 실행시 다음화면으로 넘어가기 위해 현재화면을 닫고 이전 화면을 저장합니다.
  * `if(onSwitchedScreen)` : 화면전환하면 실행되는 Event입니다. 즉, 화면 실행과 동시에 활성화 됩니다.

{% code title="" %}
```csharp
public void GoToPreviousScreen() {
    if(previousScreen) {
        SwitchScreen(previousScreen);
    }
}
```
{% endcode %}

* `GoToPreviousScreen()` : 이전화면으로 가기 위한 함수입니다.
  * `if(previousScreen)` : previousScreen이 존재하면 이전화면으로 돌아갑니다.

{% code title="IP\_UI\_System.cs" %}
```csharp
public void LoadScene(int sceneIndex) {
    StartCoroutine(WaitToLoadScene(sceneIndex));
}

IEnumerator WaitToLoadScene(int sceneIndex) {
    yield return null;
}
```
{% endcode %}

* `LoadScene(int sceneIndex)` : Coroutine을 이용하여 Scene을 Load할 때 쓰입니다.
  * 하지만 해당 Project에는 활용하지 않지만 Reusable이라는 취지를 위해 작성한 것으로 보입니다.
* `WaitToLoadScene(int sceneIndex)` : Coroutine에 필요한 IEnumerator입니다.

{% code title="IP\_UI\_System.cs" %}
```csharp
public void FadeIn() {
    if(m_Fader) {
        m_Fader.CrossFadeAlpha(0f, m_FadeInDuration, false);
    }
}
public void FadeOut() {
    if(m_Fader) {
        m_Fader.CrossFadeAlpha(1f, m_FadeOutDuration, false);
    }
}
```
{% endcode %}

* `FadeIn()` , `FadeOut()` : Fade 효과를 위해 Image Component의 CrossFadeAlpha를 이용하여 Alpha값을 조정합니다.

{% code title="IP\_UI\_System.cs" %}
```csharp
void InitializeScreens() {
    foreach(var screen in screens) {
        screen.gameObject.SetActive(true);
    }
}
```
{% endcode %}

* `InitializeScreens()` : 초기에 Start\(\) 함수에서 실행되는 함수입니다. 이를 통해 모든 Screen들을 활성화 시킵니다. 이를 통해 혹시나 비활성화 된 Screen이 없도록 합니다.
{% endtab %}
{% endtabs %}

## IP\_UI\_Screen.cs

{% tabs %}
{% tab title="Attribute" %}
```csharp
[RequireComponent(typeof(Animator))]
[RequireComponent(typeof(CanvasGroup))]
```

RequireComponent Attribute를 사용하여 해당 Script가 붙어있는 Object는 Animator, CanvasGroup을 필수적으로 붙입니다.
{% endtab %}

{% tab title="Variable" %}
{% code title="IP\_UI\_Screen.cs" %}
```csharp
[Header("Main Properties")]
public Selectable m_StartSelectable;

[Header("Screen Events")]
public UnityEvent onScreenStart = new UnityEvent();
public UnityEvent onScreenClose = new UnityEvent();

private Animator animator;
```
{% endcode %}

* `m_StartSelectable` : Selectable Class 변수입니다. namespace UI에 속한 Class이며, 이를 통해 UI관련된 Object들을 선택할 수 있습니다.
* `onScreenStart` : Screen이 시작할 때 작동되는 Event입니다.
* `onScreenClose` : Screen이 끝날 때 작동되는 Event입니다.
* `animator` : Animator Class 변수입니다. 해당 변수를 통해 Animation parameter의 값을 입력하여 StateTransition합니다.
{% endtab %}

{% tab title="Main Methods" %}
{% code title="IP\_UI\_Screen.cs" %}
```csharp
void Start() {
    animator = GetComponent<Animator>();
    if(m_StartSelectable) {
        EventSystem.current.SetSelectedGameObject(m_StartSelectable.gameObject);
    }
}
```
{% endcode %}

* Start\(\) : animator에 Access하여 Animator를 해당 Script에서 조정하기 위해 선언됩니다.
* `if(m_StartSelectable)` : Selectable Class 변수가 존재한다면 실행되는 조건문 입니다.
  * 1. `EventSystem.` : EventSystem Class에 접근합니다.
    2. `current.` : 현재 EventSystem Class를 반환합니다.
    3. `SetSelectedGameObject()` : SetSelectedGameObject\(\) 함수를 통해 m\_StartSelectable의 GameObject를 선택하여 반환합니다.
{% endtab %}

{% tab title="Helper Methods" %}
{% code title="IP\_UI\_Screen.cs" %}
```csharp
public virtual void StartScreen() {
    if (onScreenStart != null) {
        onScreenStart.Invoke();
    
    HandleAnimator("show");
}
```
{% endcode %}

* `StartScreen()` : Screen이 시작하자마자 실행되는 함수입니다.
  * `if(onScreenStart != null)` : onScreenStart Event가 존재한다면 Event를 Invoke\(\) 함수로 실행합니다.
  * `HandleAnimator("show")` : Animator Trigger Setting 함수입니다. parameter의 값에 따라 StateTransition을 실행시킵니다.

{% code title="IP\_UI\_Screen.cs" %}
```csharp
public virtual void CloseScreen() {
    if(onScreenClose != null) {
        onScreenClose.Invoke();
    }

    HandleAnimator("hide");
}
```
{% endcode %}

* `CloseScreen()` : `StartScreen()` 함수와 동일하게 작동하지만 HandleAnimator의 parameter만 변경됩니다.

{% code title="IP\_UI\_Screen.cs" %}
```csharp
void HandleAnimator(string Trigger) {
    if(animator) {
        animator.SetTrigger(Trigger);
    }
}
```
{% endcode %}

* `HandleAnimator(string Trigger)` : string parameter를 통해 Animator의 parameter로 설정하여 StateTransition합니다.
{% endtab %}
{% endtabs %}

## IP\_TimedUI\_Screen

{% tabs %}
{% tab title="Variable" %}
```csharp
[Header("Timed Screen Properties")]
public float m_ScreenTime = 2f;
private float startTime;
public UnityEvent onTimeCompleted = new UnityEvent();
```

* `m_ScreenTime` : Screen이 전환되기 전에 멈추는 시간을 설정하는 변수입니다.
* `startTime` : Time.time을 통해 이번 프레임이 시작되는 시간을 저장하는 변수입니다.
* `onTimeCompleted` : Event\(\) 변수입니다. 이 변수를 통해 해당 Script에 EventSystem을 넣어서 m\_ScreenTime 보다 큰 Time이라면, 해당 Event를 실행시킵니다.
{% endtab %}

{% tab title="Helper Methods" %}
{% code title="IP\_TimedUI\_Screen.cs" %}
```csharp
public override void StartScreen() {
    base.StartScreen();
    startTime = Time.time;
    StartCoroutine(WaitForTime());
}

IEnumerator WaitForTime() {
    yield return new WaitForSeconds(m_ScreenTime);
    if(onTimeCompleted != null) {
        onTimeCompleted.Invoke();
    }
}
```
{% endcode %}

* `StartScreen()` : IP\_UI\_Screen의 StartScreen이 Override된 함수입니다.
  * `base.StartScreen()` : base Class의 StartScreen을 실행시킨 후 Coroutine을 실행시킵니다.
* `WaitForTime()` : IEnumerator를 통해 yield return new WaitForSeconds\(\) 함수를 통해 delay 시킨 후 아래의 Code를 실행시킵니다.
{% endtab %}
{% endtabs %}

## IP\_UI\_Menus

{% tabs %}
{% tab title="Create Methods" %}
Menu를 Attribute를 사용하여 Editor상의 항목을 제작합니다.

{% code title="IP\_UI\_Menus.cs" %}
```csharp
[MenuItem("ReusableUISystem/UI Tools/Create UI Group")]
public static void CreateUIGroup() {
    GameObject uiGroup = AssetDatabase.LoadAssetAtPath<GameObject>("Assets/ReusableUISystem/Arts/Prefabs/UIBonus.prefab");
    CreateGameObject(uiGroup, "UIBouns");
}

[MenuItem("ReusableUISystem/UI Tools/Create InputField Group")]
public static void CreateUIInputField() {
    GameObject inputFieldGroup = AssetDatabase.LoadAssetAtPath<GameObject>("Assets/ReusableUISystem/Arts/Prefabs/InputField.prefab");
    CreateGameObject(inputFieldGroup, "UI_InputField");
}
```
{% endcode %}

* uiGroup : AssetDataBase inferface를 사용하여 Editor상에서 Menu를 생성합니다.
  * LoadAssetAtPath를 이용하여 해당 type의 Prefabs을 Hierarchy에 생성합니다.
* CreateGameObject\(\) : 따로 작성자가 Refactoring 과정을 통해 뽑아낸 Method입니다.
  * 해당 Method는 Instantiate를 통해 GameObject를 생성합니다.

{% embed url="https://docs.unity3d.com/kr/530/Manual/AssetDatabase.html" %}
{% endtab %}

{% tab title="Refactoring Methods" %}
Create Method에서 생성하는 항목을 Refactoring을 한 Method 입니다. 단순하게 Instantiate를 통해 GameObject를 생성하고, 생성을 못하면 Console에 오류 메시지를 표시하는 구조입니다.

{% code title="IP\_UI\_Menus.cs" %}
```csharp
public static GameObject CreateGameObject(GameObject obj, string name) {
    if(obj) {
        GameObject createGroup = (GameObject)Instantiate(obj);
        createGroup.name = name;
    }
    else {
        EditorUtility.DisplayDialog("UI Tools Warning", "Cannot find UI Group Prefabs!", "OK");
    }
    return obj;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}







