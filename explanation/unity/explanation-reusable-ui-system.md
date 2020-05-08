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

## Code Explanation

{% tabs %}
{% tab title="1. Screen을 가져오기" %}
## IP\_UI\_System\(Screen을 가져오기\)

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

이렇게 IP\_UI\_Screen 관련된 Object를 받아와서 조작합니다.
{% endtab %}

{% tab title="2. Screen Script" %}
## Screen Script에 어떤 기능이 있는가?

**IP\_UI\_System에서 Screen들을 Controller하는 역할을 했다면 Screen에서는 어떤 화면에서 일어나는 일에 대한 행동을 작성합니다.**

{% code title="IP\_UI\_Screen.cs" %}
```csharp
public Selectable m_StartSelectable;

public UnityEvent onScreenStart = new UnityEvent();
public UnityEvent onScreenClose = new UnityEvent();
```
{% endcode %}

* Selectable 변수를 통해 선택할수 있는 모든 UI의 Object를 받아옵니다.
* onScreenStart, Close 변수는 Screen이 시작, 종료할 때 Event를 담는 Data 공간입니다.

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

* Animator에 대한 GetComponent를 통해 Access하여 Set Function을 통해 State Transition을 발생시킵니다.
* if\(m\_StartSelectable\) : 특정 UI Object에 대한 변수가 m\_StartSelectable에 존재한다면 선택한다는 뜻입니다.

  * ex\) Button Click 후 마우스를 옮겼는데도 Highlight가 없어지지 않는 경우  EventSystem.current.SetSelectedGameObject\(null\);

         이라는 문구를 통해 현재 선택된 EventSystem의 Object를 초기화 시켜줍니다.

{% code title="IP\_UI\_Screen.cs" %}
```csharp
public virtual void StartScreen() {
    if (onScreenStart != null) {
        onScreenStart.Invoke();
    }
    HandleAnimator("show");
}

public virtual void CloseScreen() {
    if(onScreenClose != null) {
        onScreenClose.Invoke();
    }
    HandleAnimator("hide");
}

void HandleAnimator(string Trigger) {
    if(animator) {
        animator.SetTrigger(Trigger);
    }
}
```
{% endcode %}

* 간단하게 Screen이 시작, 종료 될때의 함수를 뜻합니다. Virtual로 선언하여 상속받아 나중에 IP\_TimedUI\_Screen에서 사용할 수 있도록 합니다.
* HandleAnimator\(string Trigger\) 함수를 통해 parameter Set을 합니다. 
{% endtab %}

{% tab title="3. SwitchScreen\(\) 함수로 Screen 전환하기" %}
## SwitchScreen\(\) 함수로 Screen 전환하

**Screen을 활성화 하면 시작되는 Event와 종료될 시점의 Event 함수들을 가진 IP\_UI\_Screen Script를 만들었다면 이제는 Screen을 변경하고 활성화하고, 시작 Event 함수를 실행시켜주는 Code가 필요합니다.**

* 해당 프로젝트에는 Screen Object가 총 3개의 IP\_UI\_Screen Script Component가 존재합니다.
* 이를 통해 Screen을 가져왔기 때문에, Screen의 전환도 IP\_UI\_Screen Script Component를 통 이루어 집니다.

{% code title="IP\_UI\_System.cs" %}
```csharp
// SwitchScreen Method
public void SwitchScreen(IP_UI_Screen screen)
{
    currentScreen = screen;
    currentScreen.gameObject.SetActive(true);
    currentScreen.StartScreen();
}
```
{% endcode %}

* 위의 내용은 SwitchScreen의 함수를 실행 시켰을 때, parameter로 받은 어떤 Screen이 currentScreen이라는 공간에 저장되고, currentScreen을 활성화 시키고, StartScreen\(\)함수를 통해 Screen이 시작하면 실행할 Event\(\) 함수를 실행시킵니다.

```csharp
public void SwitchScreen(IP_UI_Screen screen) {
    if(screen) {                                    // Condition 1
        if(currentScreen) {                         // Condition 2
            currentScreen.CloseScreen();
            previousScreen = currentScreen;
        }
        
        // SwitchScreen Method

        if(onSwitchedScreen != null) {              // Condition 3
            onSwitchedScreen.Invoke();
        }
    }
}
```

* SwitchScreen 함수를 맨 처음 실행 시켰을 때의 Code는 완성했으니, **다음화면으로 넘어가 "현재화면, 이전화면"이 존재하는 시점의 Code를 작성합니다.**
* Condition 1
  * Screen parameter가 존재한다면 아래의 Code를 실행시킵니다.
  * parameter가 존재하는지 확인하는 성격이 강합니다.
* Condition 2
  * 현재화면을 이전화면의 Data 공간으로 넘기고, CloseScreen\(\) 함수를 통해 Screen이 종료될 시 실행되는 Event\(\) 함수를 실행합니다.
* Condition 3
  * SwitchScreen\(\) 함수가 실행될 때 Inspector에서 Event를 등록한다면 실행되는 조건문입니다.

이제 SwitchScreen\(\) 함수를 통해 바꿨다면 맨 처음에 실행할 화면을 UI System이 시작과 동시에 띄우도록 하겠습니다.

{% code title="IP\_UI\_System.cs" %}
```csharp
void Start() {
    if(m_StartScreen) {
    SwitchScreen(m_StartScreen);
    }
}
```
{% endcode %}

* Start와 동시에 SwitchScreen을 통해 m\_StartScreen, 처음에 시작과 동시에 띄울 Screen을 실행합니다.
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="4. 이전화면으로 전환하기" %}
## 이전 화면으로 전환하기

**이전화면으로 돌아가기 위한 기능은 SwitchScreen\(\) 함수를 통해 제작이 되었습니다. 하나의 함수를 만들어서 필요할때 같은기능이지만 parameter는 다르게 실행하면 됩니다.**

SwitchScreen\(\) 함수는 Screen을 전환하기 때문에, 이를 통해 이전화면에 저장한 Screen을 불러옵니다.

```csharp
public void GoToPreviousScreen() {
    if(previousScreen) {
        SwitchScreen(previousScreen);
    }
}
```

* SwitchScreen\(\) 함수를 통해 미리 저장해놓은 이전화면 변수인 previousScreen을 불러옵니다.
{% endtab %}

{% tab title="5. LoadScene 함수 구현" %}
## LoadScene\(\) 함수 구현

**해당 Project에서는 Update\(\) 대신 Coroutine을 사용하여 LoadScene 기능을 구현했습니다.tutorial에서는 사용하진 않지만 Reusable UI System 이라는 취지를 살린 함수 부분입니다.**

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

* LoadScene\(int  sceneIndex\) 함수는 parameter로 int형 변수를 받아서 Load하고 싶은 Scene을 Coroutine을 통해 재생합니다.
* WaitToLoadScene\(int  sceneIndex\) 함수는 내용은 없지만 추후에 Load하게 될 함수입니다.
* 이에 대한 내용은 기술문서\(Technical Reference\)에 기술하였으니, 아래의 링크를 따라 들어가시면 좀 더 많은 정보를 얻으실 수 있습니다.

{% page-ref page="../../technical-reference/unity/tr-reusable-ui-system.md" %}
{% endtab %}

{% tab title="6. InitializeScreen 구현" %}
## InitializeScreen\(\) 함수 구

**맨 처음에 Login\_Screen 이외의 Object들을 비활성화 했기 때문에 맨 처음 만들어줄 때 활성화 시켜야 초기 Screen에서 다음 Screen으로 넘어갈 수 있는 Object가 생깁니다.**

```csharp
void InitializeScreens() {
    foreach(var screen in screens) {
        screen.gameObject.SetActive(true);
    }
}
```

* Foreach 반복문을 통해 Screens을 모두 다 받아와서 SetActive\(\)를 true로 만듭니다.
* System이 시작과 동시에 초기화를 할 수 있도록 Start\(\) 함수에서 실행시킵니다.

{% code title="IP\_UI\_System.cs" %}
```csharp
void Start() {
    InitializeScreen();
}
```
{% endcode %}
{% endtab %}

{% tab title="7. Fade 효과" %}
## Fade 효과

최근의 UI를 보면 여러가지 기법이 사용하여 사용자가 좀 더 부드럽고, 편안한 효과를 볼 수 있도록 하는데 그 중 하나가 Fade 효과입니다.

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

* 미리 선언한 Image Component를 가져와서 CrossFadeAlpha\(\) 함수를 통해 Fade 효과를 부여합니다.

{% embed url="https://docs.unity3d.com/kr/530/ScriptReference/UI.Graphic.CrossFadeAlpha.html" %}

* 그리고 시작\[Start\(\)\]과 동시에 Fade Image를 활성화 하고, FadeIn\(\) 함수를 실행합니다.

```csharp
void Start() {
    if(m_Fader) {
        m_Fader.gameObject.SetActive(true);
    }
    FadeIn();
}
```

* **이때 FadeIn\(\) 함수만 실행하는데, 이와 같이 작성한다면 Code의 간결함은 있지만, 추후에 크기가 커진 Code를 작성시 Side Effect를 발생시킬 확률이 굉장히 높습니다.**
* **그렇기 때문에 Project를 활용하여 다른 프로그램을 작성시에 이 부분은 필히 수정해야 할 부분입니다.**
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="WaitScreen 화면 적용시키기" %}
## WaitScreen을 적용시키기

WaitScreen Object를 생성하여 기존 Fade 효과가 아닌 우리가 흔히 볼수있는 Loading 화면을 구현합니다.

해당 Object는 Coroutine을 사용하여 onTimedCompleted EventSystem 변수를 통해 Event를 실행시킵니다.

{% code title="IP\_TimedUI\_Screen.cs" %}
```csharp
public class IP_TimedUI_Screen : IP_UI_Screen
{
        public float m_ScreenTime = 2f;
        private float startTime;
        public UnityEvent onTimeCompleted = new UnityEvent();
}
```
{% endcode %}

* Screen이 활성화되는 동안의 시간과\(m\_ScreenTime\), 현재 프레임부터 시간을 Count 하여 Delay Time을 주는 시간\(startTime\)이 있습니다.
* EventSystem 변수를 통해 이 변수에 어떠한 Event가 등록되는 순간 실행되도록 합니다.
  * 이 변수는 활성화가 끝나는 시간에 실행되는 Event 변수입니다.

가상함수인 StartScreen에 Coroutine을 사용하여 yield 명령어에 따라 해당 IEnumerator 함수를 종료합니다.

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

* Project에서는 m\_ScreenTime을 활용하여 Coroutine을 활용하고 있지만, startTime은 사용되지 않습니다.
* 이를 활용하려면 여러 방법이 있는데 제가 생각한 방법은 아래와 같습니다.
  * startTime과 m\_ScreenTime을 비교하는 조건문을 걸어서 검증하는 방법
  * WaitForTime\(\) 함수에 parameter로 float 변수를 넣어서 startTime과 비교하는 방

{% code title="startTime 활용 방안" %}
```csharp
// Statement 1
if(startTime > m_ScreenTime)
StartCoroutine(WaitForTime());

// Statement 2
IEnumerator WaitForTime(float time) {
    if(startTime > time)
    yield return new WaitForSeconds(time);
}
```
{% endcode %}
{% endtab %}

{% tab title="" %}

{% endtab %}

{% tab title="" %}

{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

## 마치며

* 해설문서\(Explanation\)을 작성하면서, 부족한 부분의 설명은 모두 기술문서\(Technical Reference\)에 작성했습니다.
* 재사용이 가능한 UI Project를 작성함으로써 개발에 있어서 할수 있는 영역이 넓어졌다는 느낌이 드는 작업이였습니다.
* 부족한 부분을 8iioww@gmail.com로 보내주시면 수정 및 보완하도록 하겠습니다.

{% page-ref page="../../technical-reference/unity/tr-reusable-ui-system.md" %}

