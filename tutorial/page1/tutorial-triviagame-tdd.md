---
description: tutorial TriviaGame TDD
---

# tutorial TriviaGame TDD

##  무엇을 하려고 하는가?

* 게임을 제작할 때 많은 버그와, 이슈, Side Effect가 발생됩니다. 이를 해결할 때 QA\(_Quality Assurance_\) 라는 가이드라인을 통한 수정을 할 수도 있지만, 제작하는 과정에서 미리 테스트를 통해서 발견 할 수 있는 버그들은 제작과정에서 고치는 것이 수월합니다.
* 그 방법의 하나로써 TDD\(Test - Driven - Development\)라는 방법론을 채택하여 Unity 자체에 탑재된 기능의 하나인 Test Runner를 사용하여 Trivia Game을 만들고, Unit 단위로 올바른 값이 들어간지 체크할 수 있습니다.

{% hint style="info" %}
보통 게임을 플레이 하면서 충돌이나, 플레이가 안된다거나, 렌더링이 안된다거나 등에 대한 오류가 대부분을  차지하고, 값에 대한 올바른 값이 들어갔는가에 대한 오류는 아주 극소수입니다.

하지만 그런 오류를 잡는것 뿐만 아니라, Unit 단위의 테스트를 통해, Unity로 Game 뿐만이 아니라 App 개발에 있어서 아주 유용하게 사용할 것 같아서 이 주제로 문서를 작성하게 되었습니다.
{% endhint %}

* 해당 자료는 상업적으로 이용하지 않고 이용되서도 안됩니다.
* 아래의 링크를 타고 들어가면 해당 문서에 대한 보다 풍부한 자료가 있습니다.

{% embed url="https://engineering.etermax.com/how-to-tdd-in-unity-using-the-mvp-pattern-a646ffbe996f" caption="MVP\(Model ,View, Presenter\)패턴을 사용한 TDD 사용법" %}

## 작성법 - UI 배치

* 배치에는 다소 수고스러움이 있으니 왠만하면 Complete Project File을 사용하는걸 추천합니다.
  * 작성법 - TDD 부분으로 넘어가시면 됩니다.
* UI 배치 관하여 연습을 하고 싶다면 Complete Project를 보면서 하나씩 해보는 것을 추천합니다.
* 위의 Hierarchy 그림처럼 GameObject를 배치하는 게 최종 목표입니다.

![&#xC704;&#xC758; &#xADF8;&#xB9BC;&#xCC98;&#xB7FC; &#xBC30;&#xCE58;&#xD569;&#xB2C8;&#xB2E4;.](../../.gitbook/assets/image%20%2865%29.png)

* Canvas
  * Canvas Scaler - UI Scale Mode를 Scale With Screen Size로 설정해 1920 \* 1080로 조절합니다.
* Child Object로 GamePlayScreen을 배치하고 그림과 같이 Child Object를 설정합니다.
* BackGround, Bonzo, Tina
  * BackGround
    * Image Ccomponent 추가 후 Color를 임의의 색으로 조정합니다.
  * Bonzo
    * Image Component 추가 후 Bonzo Sprite 설정합니다.
    * 화면의 양 옆에 배치합니다.
  * Tina
    * Image Component 추가 후 Tina Sprite 설정합니다.
    * 화면의 양 옆에 배치합니다.

 여기까지 했으면 아래와 같은 그림의 Object 배치가 완료 됩니다.

![](../../.gitbook/assets/image%20%2896%29.png)

* Score
  * Score Object를 추가하고 Image Component를 추가하여, ScoreContainer Sprite로 설
  * Anchors를 활용하여 Top, Left로 설정합니다.
    * 이렇게 하면 설정한 위치를 기준으로 Position, Scale이 적용 됩니다.
  * 설정한 Anchors를 기반으로 Width, Height를 설정하여 크기를 조정합니다.
    * 작성자는 300 / 200정도로 설정했습니다.
  * Score Label Object를 Child Object로 추가합니다.
* ScoreLabel
  * Score Object의 Child Object로써, TextMeshPro - Text\(UI\) Component를 추가합니다.
  * 굵게\(Font Style : B\), 크기는 50\(Font Size : 50\), Alignment\(Center, Middle\)을 설정합니다.
    * 임의대로 바꿔도 무방합니다.
* ScoreText
  * Score Object Child Object로써, TextMeshPro - Text\(UI\) Component를 추가합니다.
  * ScoreLabel과 같이 Font에 대한 설정을 임의 대로 합니다.
    * 작성자는 \(B, 50, Center / Middle\)로 설정하고 Vertex Color를 Black으로 설정했습니다.

![&#xC784;&#xC758;&#xB85C; &#xC791;&#xC131;&#xD55C; Score Object&#xC5D0; &#xB300;&#xD55C; &#xACB0;&#xACFC;](../../.gitbook/assets/image%20%2889%29.png)

* Question
  * Image Component를 추가하여 QuestionContainer Sprite로 설정합니다.
  * Width, Height\(1050, 920\)를 설정합니다.
* QuestionContainer
  * Question의 Child Object로써 Image Component를 추가하여 TextContainer Sprite로 설정합니다.
  * Anchors를 아래와 같이 설정합니다.
    * min X : 0.5 / min Y : 1
    * max X : 0.5 / max Y : 1
  * Pivot을 설정합니다.
    * X : 0.5 / Y : 1
* QuestionText
  * TextMesh Pro - Text\(UI\) Component를 추가합니다.
  * Anchors를 아래와 같이 설정합니다.
    * min X : 0 / min Y : 0
    * max X : 1 / max Y : 1
  * Pivot을 설정합니다.
    * X : 0.5 / Y : 0.5
  * Font를 아래와 같이 설정합니다.
    * Font Style : B
    * Auto Size Check
      * Min : 18 / Max : 60
    * Vertex Color : Black
    * Alignment : Center / Middle
* ContainerAllAnswer
  * Question Object의 Child Object로써, Answer 기능에 관련한 Object를 배치하기 위한 BackGround Object입니다.
  * Image Component를 추가하여 AnswerContainer Sprite를 설정하고 Color.Alpha값을 줄여줍니다.
  * 아래와 같이 Rect Transform을 설정합니다.
    * Pos X : 0 / Pos Y : -157 / Pos Z : 0
    * Width : 1000 / Height : 538
* Answer
  * Question Object의 Child Object로써, 해당 Object에 Child Object를 추가하여 Answer Object를 관리합니다.
  * Vertical Layout Group을 추가하여 Control Child Size의 Width를 Check합니다.
    * 이렇게 하면 간편하게 수직적으로 Object를 추가할 때 Transform이나 Position, Anchors의 변화 없이 추가할 수 있습니다.
  * Rect Transform을 아래와 같이 설정합니다.
    * Anchors
      * middle Stretch로 설정
      * Left : 50 / Pos Y : -167 / Pos Z : 0
      * Right : 50 / Height : 512
* Answer N Object
  * Answer Object의 Child Object로써, 해당 Object는 Button Component를 통해 동작합니다.
  * Button Component 추가 후 On Click\(\) Event를 추가해줍니다.

일단 여기까지 하고 Script를 추가해야 하는데 아래의 Script단락에서 추가 후에 Prefab으로 만들어서 똑같이 추가해 줍니다.

여기까지 하셨다면 아래의 그림과 같은 결과가 나옵니다.

![](../../.gitbook/assets/image%20%283%29.png)



## 작성법 - Script

해당 프로젝트의 완전환 파일은 아래의 링크에서 다운 받으실 수 있습니다.

{% embed url="https://github.com/ValeriaColombo/unity\_tdd\_example" caption="Complete Project File " %}



* 새로운 2D Project를 생성하여 아래의 Hierarchy 처럼 배치합니다. 
* Script들을 생성하고 아래의 내용을 넣습니다.

{% tabs %}
{% tab title="First Tab" %}

{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}









