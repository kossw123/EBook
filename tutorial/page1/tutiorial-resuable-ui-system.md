---
description: tutiorial Resuable UI System
---

# tutiorial Resuable UI System

## 무엇을 하려고 하는가?

* 재사용이 가능한 UI System Project를 생성합니다.
* 프로그램에 있어서 사용자가 사용하기 쉽게 하기 위한 UI를 작성하고, 이를 재사용 가능하게끔 하여, 다른 Project에서도 적용 가능하도록 합니다.
* 해당 문서는 상업적으로 이용되지 않습니다.
* 문서보다 영상을 보면서 작성하시는게 좀 더 스킬업에 도움이 될 수 있습니다.

{% embed url="https://www.youtube.com/watch?v=8L9osm0h5J4&list=PL5V9qxkY\_RnJAZUTVXewQrJWbb5B7IU8y&index=2&t=0s" %}

* Reusable UI System은 아래의 그림을 모방하여 UI를 구성합니다.

![&#xCD08;&#xAE30;&#xD654;&#xBA74; UI &#xAD6C;&#xC131;](../../.gitbook/assets/image%20%28116%29.png)

## 작성법

* 2D Project를 생성하고 아래의 링크에 들어가서 필요한 Texture들을 다운받습니다.

{% embed url="https://gumroad.com/l/iXYzT" %}

* 첫번째로 기본적인 초기화면을 작성합니다.

{% tabs %}
{% tab title="1. Camera 설정" %}
* GameView에서의 Resolution을 1080 \* 1920으로 변경합니다.
{% endtab %}

{% tab title="2. UI Controller 생성" %}
* Empty Object를 생성하여 UI Controller Object를 생성합니다.
  * 해당 Project의 모든 Object는 UI Controller Object 아래에 생성합니다.
{% endtab %}

{% tab title="3. Canvas 설정" %}
* Canvas Object을 생성합니다.
  * Canvas Scaler - Scale With Screen Size로 변경하고 Resolution을 1080 \* 1920으로 변경합니다.
{% endtab %}

{% tab title="4. Canvas 하위 Object 설정" %}
* Image Object를 생성하여 BackGround Object를 생성합니다.
  * Anchor Preset을 상하좌우 Stretch로 설정하여 모든 Anchor값을 0으로 설정하여 Image를 펼칩니다.
  * todo\_login\_bg\_001를 대입하여 BackGround를 생성합니다.



* Image Object를 생성하여 Icon Object를 생성합니다. 
  * todo\_logo\_001를 대입하여 Icon을 생성합니다.
  * Width, Height를 적당히 조절하여 크기를 설정합니다.



* InputField를 하나 생성합니다.
  * Anchor를 middle Stretch로 설정하여 Height를 제외한 모든 Anchor를 0으로 만들고, Height를 적당히 조정합니다.
  * Image Component를 삭제합니다.
  * Placeholder와 Text Object의 적당히 조정합니다. 이에대한 크기는 아래쪽의 그림에 표시하겠습니다.
  * Image Object를 하위 Object로 생성하여, todo\_user\_icon\_001을 대입하여 User Icon을 최종적으로 생성합니다.
  * Placeholder의 Text에 UserName이라고 작성합니다.
  * Font를 LATO-SEMIBOLD로 교체하고 크기와 배치를 적당히 조정합니다.



* 생성한 InputField를 복사하여 Password InputField를 생성합니다.
  * 이때 위에서 변경한 Image Component와, Placeholder의 Text를 변경합니다.



* Forget Password Button을 생성합니다.
  * Image Component를 삭제합니다.
  * Text Component의 문구를 수정하고, Font\(LATO-SEMIBOLD\), 크기와 색을 수정합니다.
* Button Object를 생성하여 Sign In Object를 생성합니다.
  * Text, Font, Alignment, Color를 수정합니다. 
  * 임의로 수정하셔도 무방합니다. 견본은 아래의 그림에 있습니다.
* Panel Object를 생성하여 위치를 수정하고 Text를 수정하고 Button Object를 생성합니다.
  * 수정된 위치의 옆에 Button Object를 배치하고 Image Component를 제거합니다.



마지막으로 Canvas Child Object로 Empty Object를 생성 후 Login\_Screen이라고 작성하고 작성한 모든 Object를 하위 Object로써 넣습니다.

이로써 하나의 Screen이 완성되었습니다.
{% endtab %}

{% tab title="" %}

{% endtab %}
{% endtabs %}

여기까지 과정은 아래의 그림과 같습니다.

![Login\_Screen &#xC644;&#xC131;&#xBCF8;](../../.gitbook/assets/image%20%28129%29.png)

* 두번째 Screen인 Register\_Screen을 작성합니다.
* Register\_Screen은 Login\_Screen과 같은 방법으로 작성하되, 배치와 Text 문구만 변환한 것이기 때문에 자세한 설명은 그림으로 대체하겠습니다.

![Register\_Screen &#xC644;&#xC131;&#xBCF8;](../../.gitbook/assets/image%20%2884%29.png)

* 각 Screen을 조정할 Controller Object의 Script를 작성하고 해당 Object에 넣습니다.
  * 총 2가지 Script가 작성됩니다.
    * IP\_UI\_System : Controller Object의 Script입니다.
    * IP\_UI\_Screen : 각 Screen에 삽입될 Script입니다.

{% tabs %}
{% tab title="IP\_UI\_System.cs" %}
{% code title="IP\_UI\_System.cs" %}
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using UnityEngine.Events;
namespace DataPractice.UI {
    public class IP_UI_System: MonoBehaviour {
        [Header("Main Properties")]
        public IP_UI_Screen m_StartScreen;
        [Header("System Events")]
        private UnityEvent onSwitchedScreen = new UnityEvent();
        [Header("Fade Properties")]
        public Image m_Fader;
        public float m_FadeInDuration = 1 f;
        public float m_FadeOutDuration = 1 f;
        public Component[] m_Screens = new Component[0];
        private IP_UI_Screen previousScreen;
        private IP_UI_Screen currentScreen;
        public IP_UI_Screen PreviousScreen { get { return previousScreen; } }
        public IP_UI_Screen CurrentScreen { get { return currentScreen; } }
        private void Start() {
            m_Screens = GetComponentsInChildren < IP_UI_Screen > (true);
            InitializeScreen();
            if (m_StartScreen) {
                SwitchScreen(m_StartScreen);
            }
            if (m_Fader) {
                m_Fader.gameObject.SetActive(true);
            }
            FadeIn();
        }
        public void SwitchScreen(IP_UI_Screen aScreen) {
            if (aScreen) {
                if (currentScreen) {
                    currentScreen.CloseScreen();
                    previousScreen = currentScreen;
                }
                currentScreen = aScreen;
                currentScreen.gameObject.SetActive(true);
                currentScreen.StartScreen();
                if (onSwitchedScreen != null) {
                    onSwitchedScreen.Invoke();
                }
            }
        }
        public void GoToPreviousScreen() {
            if (previousScreen) {
                SwitchScreen(previousScreen);
            }
        }
        public void FadeIn() {
            if (m_Fader) {
                m_Fader.CrossFadeAlpha(0 f, m_FadeInDuration, false);
            }
        }
        public void FadeOut() {
            if (m_Fader) {
                m_Fader.CrossFadeAlpha(1 f, m_FadeOutDuration, false);
            }
        }
        public void LoadScene(int sceneIndex) {
            StartCoroutine(WaitToLoadScene(sceneIndex));
        }
        IEnumerator WaitToLoadScene(int sceneIndex) {
            yield return null;
        }
        void InitializeScreen() {
            foreach(var screen in m_Screens) {
                screen.gameObject.SetActive(true);
            }
        }
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="IP\_UI\_Screen.cs" %}
{% code title="IP\_UI\_Screen.cs" %}
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;
using UnityEngine.EventSystems;
using UnityEngine.UI;
namespace DataPractice.UI {
    [RequireComponent(typeof(Animator))]
    [RequireComponent(typeof(CanvasGroup))] 
    public class IP_UI_Screen: MonoBehaviour {
        public UnityEvent onScreenStart = new UnityEvent();
        public UnityEvent onScreenClose = new UnityEvent();
        Animator animator;
        public Selectable m_StartSelectable;
        private void Start() {
            animator = GetComponent < Animator > ();
            if (m_StartSelectable) {
                EventSystem.current.SetSelectedGameObject(m_StartSelectable.gameObject);
            }
        }
        public virtual void StartScreen() {
            if (onScreenStart != null) {
                onScreenStart.Invoke();
            }
            HandleAnimation("show");
        }
        public virtual void CloseScreen() {
            if (onScreenClose != null) {
                onScreenClose.Invoke();
            }
            HandleAnimation("hide");
        }
        void HandleAnimation(string Trigger) {
            if (animator) {
                animator.SetTrigger(Trigger);
            }
        }
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

* Screen의 작성은 끝났고, FadeIn, Out효과와 Show, Hide Animation을 추가합니다.

{% tabs %}
{% tab title="1. Animator, Animation 생성" %}
하나의 IP\_UI\_Base\_Controller라는 이름의 Animator를 생성합니다.

Login\_Screen Object의 Animator에 생성한 Animator를 넣고, Animation View를 열어서 3개의 Clip을 생성합니다.

* IP\_Base\_Screen\_Idle : Show, Hide Animation을 제외한 Animation
* IP\_Base\_Screen\_Show : 보여줄 때의 Animation
* IP\_Base\_Screen\_Hide : 숨길 때 Animation

![Animator&#xC5D0; &#xB4E4;&#xC5B4;&#xAC08; Clip &#xBAA9;&#xB85D;](../../.gitbook/assets/image%20%28106%29.png)

* Idle Animation에서 변경할 내용입니다. 0프레임 부터 20프레임까지의 변화를 기입합니다.
  * Alpha\(0\)
  * Interactable\(false\)
  * Block RayCasts\(false\)
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

