---
description: DOTween
---

# DOTween

## 무엇을 하려고 하는가?

* DOTween에 대한 정의, 사용방법, 응용방법 등을 기재합니다.
* 생각보다 다른 문서에 DOTween을 쓸일이 많아 작성하게 되었습니다.
* 자세한 내용은 DOTween Document로 정리되어 있습니다만, 추가적인 이해가 필요했기에 이 문서를 작성하게 되었습니다.
* DOTween의 공식 문서를 보고 정리한 것입니다. 이것을 가지고 상업적인 목적으로 이용하지 않습니다.

{% embed url="http://dotween.demigiant.com/index.php" caption="DoTween 공식 홈페이지" %}

## DOTween 이란?

* Unity에서 동작하는 안전 객체 지향  애니메이션 엔진입니다.
* 기존 버전인 HOTween에 비해 더욱 빠르고, 효율적이며, 쓸모없는 garbage collector 할당을 피합니다.
* DOTween은 Unity 버전 2019 ~ 4.6과 호환됩니다.

## 명명법\(Nomenclature\)

* Tweener : 속성 / 필드값을 가져와 주어진 값으로 재생합니다.
* Sequence : Tweener와 유사하지만 다른 Tween 또는 Sequence를 그룹으로 재생합니다.
* Tween : Tweener와 Sequence를 모두 나타내는 일반적인 단어입니다.
* Nasted Tween : Sequence 안에 포함된 Tween입니다.
* Prefixes : 접두사는 IntelliSense를 최대한 활용하는 데 중요하므로 다음 사항을 기억하십시오.
  * DO : 모든 Tween Shortcut의 접두사\(변환 또는 재질과 같은 알려진 객체에서 직접 시작할 수있는 작업\) 주 DOTween 클래스의 접두사이기도합니다.
  * ```
    EX)    transform.DOMoveX(100, 1);
           ﻿﻿﻿﻿﻿﻿﻿﻿transform.DORestart();
        ﻿﻿﻿﻿﻿﻿﻿﻿   DOTween.Play();
    ```
  * Set : Tween에 연결할 수있는 모든 설정의 접두사 입니다.

    \(설정으로 적용되지만 실제로 설정이 아니기 때문에 보낸 사람 제외\).

    ```text
    EX) myTween.SetLoops(4, LoopType.Yoyo).SetSpeedBased();
    ```

  * On : Tween에 연결할 수있는 모든 콜백의 접두사입니다.
  * ```text
    EX) myTween.OnStart(myStartFunction).OnComplete(myCompleteFunction);
    ```

## DOTween의 특징

{% embed url="http://blog.demigiant.com/what-is-a-tween-engine/" %}

* **Speed and efficiency**
  * **빠르고 효율적입니다**. 쓸모없는 garbage collector 할당을 피하기 위해 **모든것이 cash로 저장되고 재사용 됩니다.**
* **IntelliSense and type-safety**
  * 모든 코드는 XML 주석으로 완성되어 있고, IntelliSense를 최대한 활용하게 되어있습니다.
  * 모든것이 type-safe\(형식안정성\)을 띄며 문자열을 사용하지 않습니다.
    * type-safe : Complier가 동작하는 동안 형식의 유효성을 검사하고 변수에 잘못된 형식을 할당하려고 하면 오류를 발생시킵니다.
* **Shortcuts**
  * 코드의 ShortCut을 지향하고 직접적인 표현을 사용합니다.

```csharp
// Move a transform to position 1,2,3 in 1 second
﻿﻿﻿﻿﻿﻿﻿﻿transform.DOMove(new Vector3(1,2,3), 1);
﻿﻿﻿﻿﻿﻿﻿﻿// Scale the Y of a transform to 3 in 1 second
﻿﻿﻿﻿﻿﻿﻿﻿transform.DOScaleY(3, 1);
﻿﻿﻿﻿﻿﻿﻿﻿// Pause a transform's tween
﻿﻿﻿﻿﻿﻿﻿﻿transform.DOPause();
```

* **Extremely accurate**
  * 시간은 매우 정확한 방식으로 계산됩니다.
* **Logical and easy to use API**
  * 효율성, 직관적 성 및 사용 편의성을 높이기 위해 만든 API입니다.
* **Animate everything \(almost\)**
  * 모든 숫자값과 숫자가 아닌 일부 값을 재생할 수 있습니다. rich-text를 이용하여 문자열에 Animation을 적용할 수 있습니다.
* **Modules**
  * DOTween 모듈을 통해 Unity 시스템 \(물리 / 오디오 / UI / 등\)에 대한 참조를 활성화 / 비활성화 할 수 있습니다.
* **Snapping, axis constraints and other options**
  * 스냅\(Snap\), 축 구속 조건\(axis constraints\) 및 기타 옵션을 설정할 수 있습니다.
    * snap : Object를 일정한 간격으로 움직
* **Full control**
  * Tween을 제어하기 위해 재생, 일시 정지, 되감기, 다시 시작, 완료, 이동 및 기타 유용한 방법이 많이 있습니다.
* **Grouping**
  * Tween을 Sequence로 연결하여 복잡한 애니메이션을 만듭니다.
* **Blendable tweens**
  * 강력한 DOBlendable ShortCut 덕분에 일부 Tween이 실시간으로 서로 혼합 될 수 있습니다.
* **Paths**
  * 입맛대로 선형 및 곡선 경로에 따라 Animation을 적용 가능 합니다.
* **Change values and duration while playing**
  * 재생중에서도 Tween의 Start/end 값 또는 지속시간을 언제든지 변경 가능합니다.
* **Safe mode**
  * 옵션 안전 모드를 활성화하고 재생 중에  Tween을 적용시킨 Object가 파괴되는 등 DOTween이 예기치 않은 상황을 처리하도록합니다.
* **Yield for coroutines**
  * Coroutine에 적용시킬 수 있습니다.
* **Multiple rotation modes**
  * 회전 Tween은 가장 짧은 경로, 전체 경로를 사용하거나 로컬 또는 세계 기반 축을 사용할 수 있습니다.
* **Shared methods**
  * Tweener, Sequence 둘다 동일한 방식으로 저장하고 제어할 수 있습니다.
* **Plugins**
  * DOTween을 가지고 별도의 Plugin을 별도의 파일로 제작할 수 있습니다.
* **Extras**
  * Extra Virtual method는 어느정도의 딜레이를 준 다음 함수를 호출하는 작업을 합니다.
* **All the basics**
  * 콜백, 루프, 용이성 \(AnimationCurves 및 사용자 정의 기능 포함\), SpeedBased 및 기타 여러 트위닝 옵션과 
  * 업데이트 유형 선택 : 정기, 고정, 늦음 및 더하기 옵션으로 시간을 독립적으로 조정할 수 있습니다.

## 작성법

{% tabs %}
{% tab title="초기화" %}
자세한 사용법은 DOTween Document에 자세히 나와있습니다.

{% embed url="http://dotween.demigiant.com/download" caption="DOTween Download" %}

1. DOTween을 사용하기 위해서는 위의 링크에서 API를 다운 받아야 합니다.
2. 다운을 받고 사용하려는 project 경로에 삽입합니다.

   \*\*\* 만약에 DOTween 파일이 중복이 된다면 오류가 떠서 작동이 안되는 경우가 있습니다.

3. DOTween을 Script에서 사용하기 위해 namespace에 추가합니다.

   **ex\) using DG.Tweening;**

4. Initalize\(초기화\)를 해야합니다.

   \*\*\* 초기화를 따로 설정하지 않는다면 자동적으로 default setting으로 됩니다.
{% endtab %}

{% tab title="Tweener 작성" %}
이제 초기화를 했다면 Tweener 혹은 Sequence를 만들어서 사용하면 되는데, 이때 만드는 방법은 여러가지가 존재합니다.

* generic ways
  * 람다식을 사용하여 유연성이 높고, public or private, static or dynamic value들을 거의 다 Tweening 할 수 있습니다.
  * DOTween.To\(\) 함수를 통해 기존의 HOTween에서 사용되던 From 함수를 대체합니다.

![generic way of DOTween.To\(\) ](../../.gitbook/assets/image%20%2830%29.png)

* shortcuts ways
  * Unity Component들에 대한 직접적인 참조가 가능하며 개인적으로는 움직임을 구현하는 것 이외의 Component들을 한줄의 코드로 조절이 가능하여 자주 사용합니다.

![](../../.gitbook/assets/image%20%2884%29.png)

* additional generic method
  * 위에 서술한 방법을 제외한 함수입니다.
  * 특정한 방식으로 동작합니다.
{% endtab %}

{% tab title="Sequence 작성" %}
Tweener가 하나의 속성 / 필드값을 가지고 적용시켰다면 Sequence는 여러개의 Tweener를 혹은 Sequence 그룹화 한 다음 동작합니다.

Sequence의 특징은 다음과 같습니다.

* 계층의 깊이에 상관없이 다른 Sequence에 포함될 수 있습니다.
* Sequence화 시킨 Tween들은 순차적일 필요가 없습니다. 이때 Insert\(\) 함수로 Tween을 겹칠 수 있습니다.
* 하나의 Sequence에서 쓴 Tween은 다른 Sequence에서 재사용 할 수 없습니다.
* 빈 Sequence를 사용하면 안됩니다.
* Sequence가 시작되기 전\(다음 프레임으로 넘어가기 전\)에 아래의 방법을 적용시켜야 합니다. 그렇지 않으면 Sequence가 동작하지 않습니다.
* Delay, Loop는 중첩된 Tween 내부에서도 작동합니다.

작성 방법은 아래와 같습니다.

1. 새로운 Sequence를 생성하고 참조합니다.
2. 생성한 Sequence에 Tweener, intervals, callback을 추가합니다.

```csharp
// 새로운 Sequence 생성
﻿﻿﻿﻿﻿Sequence mySequence = DOTween.Sequence();
﻿﻿﻿﻿﻿// Append()를 통해 Sequence 끝에 DOMoveX tweener를 추가합니다.
﻿﻿﻿﻿﻿mySequence.Append(transform.DOMoveX(45, 1));
﻿﻿﻿﻿﻿// Append()를 통해 DORotate를 하여 y축으로 180도를 1초마다 회전합니다.
﻿﻿﻿﻿﻿mySequence.Append(transform.DORotate(new Vector3(0,180,0), 1));
﻿﻿﻿﻿﻿// 전체 Sequence를 1초 지연시킵니다.
﻿﻿﻿﻿﻿mySequence.PrependInterval(1);
﻿﻿﻿﻿﻿// Insert()를 통해 Sequence에 DOScale을 추가하고, Sequence가 실행되는 동안
// 계속 지속시킵니다.
﻿﻿﻿﻿﻿mySequence.Insert(0, transform.DOScale(new Vector3(3,3,3), mySequence.Duration()));
```
{% endtab %}
{% endtabs %}

## 자주 쓰이는 Method

해당 단락을 앞으로 추가할 예정입니다.

* Append\(\) : Sequence 끝에 Tween을 추가합니다.
* Join\(\) : Sequence 끝에 추가한 Tween과 동일한 시간에서 실행됩니다.

## 마치며

* 위의 과정과 같이 Tweening을 거친다면 Animation 및 여러 Component를 간략화된 코드로 조절할 수 있어서 굉장히 편합니다.
* 하지만 이러한 API를 사용하여 동작한다고 한다면 여러면에서 가독성은 좋아지고 재사용성도 좋아지지만 Sequence에 대한 관리가 굉장히 중요할 듯 합니다.



