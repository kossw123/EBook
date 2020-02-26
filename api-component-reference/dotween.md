---
description: DOTween
---

# DOTween

## 무엇을 하려고 하는가?

* DOTween에 대한 정의, 사용방법, 응용방법 등을 기재합니다.
* 생각보다 다른 문서에 DOTween을 쓸일이 많아 작성하게 되었습니다.
* DOTween의 공식 문서를 보고 정리한 것입니다. 이것을 가지고 상업적인 목적으로 이용하지 않습니다.

{% embed url="http://dotween.demigiant.com/index.php" caption="DoTween 공식 홈페이지" %}

## DOTween 이란?

* Unity에서 동작하는 안전 객체 지향  애니메이션 엔진입니다.
* 기존 버전인 HOTween에 비해 더욱 빠르고, 효율적이며, 쓸모없는 garbage collector 할당을 피합니다.
* DOTween은 Unity 버전 2019 ~ 4.6과 호환됩니다.

## 명명법\(Nomenclature\)

* Tweener : 
* Sequence : 값을 제어하는 ​​대신 다른 Tween을 제어하여 그룹으로 애니메이션하는 특수 Tween입니다.
* Tween : Tweener와 Sequence를 모두 나타내는 일반적인 단어입니다.
* Nasted Tween : Sequence 안에 포함된 Tween입니다.
* Prefixes : 접두사는 IntelliSense를 최대한 활용하는 데 중요하므로 다음 사항을 기억하십시오.
  * DO : 모든 Tween Shortcut의 접두사\(변환 또는 재질과 같은 알려진 객체에서 직접 시작할 수있는 작업\) 주 DOTween 클래스의 접두사이기도합니다.
  * ```csharp
    transform.DOMoveX(100, 1);
    ﻿﻿﻿﻿﻿﻿﻿﻿transform.DORestart();
    ﻿﻿﻿﻿﻿﻿﻿﻿DOTween.Play();
    ```
  * Set : Tween에 연결할 수있는 모든 설정의 접두사 

    \(설정으로 적용되지만 실제로 설정이 아니기 때문에 보낸 사람 제외\).

  * On : Tween에 연결할 수있는 모든 콜백의 접두사입니다.

## DOTween의 특징

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
  * \*\*\*\*
* **Plugins**
  * 
* **Extras**
  * 
* **All the basics**
  * 

