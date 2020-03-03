---
description: TR Mario Galaxy's Launch Star
---

# TR Mario Galaxy's Launch Star

## Blend Tree Animation

![Jammo\_Player Animator&#xC758; Normal Status Blend Tree](../../.gitbook/assets/image%20%2839%29.png)

{% embed url="https://wergia.tistory.com/54?category=739103" caption="Blend Tree Animation 사용" %}

{% embed url="https://docs.unity3d.com/kr/2018.4/Manual/BlendTree-2DBlending.html" caption="Blend Type에 대한 Document" %}

* **Blend Animation이란?**
  * Animation을 자연스럽게 움직이게 하기 위해서 작업시간과 인력이 없을 때 사용할 수 있는 기능입니다.
* **가장 기본적인 1D Blend Type을 어떻게 만들까?**
  * Animator에서 State를 생성할 때 Blend Tree 항목을 선택하여 생성하면 float parameter와 하나의 State가 생성됩니다.
  * 생성한 State를 더블클릭하여 들어가면 Inspector창에 Blend Type과 parameter의 값에 따른 Motion이 있는데 여기에서 특정 동작에 대한 Animation의 Clip을 넣고, Threshold라는 옵션의 값에 따라 시간에 따른 Clip의 재생위치를 선형 보간합니다.
    * 선형 보간이란 시작, 끝 값이 존재할 때 그 사이에 위치한 값을 추정하기 위해 선형적
* **디테일을 주기 위한 2D Simple Directional을 어떻게 만들까?**
  * 위와 동일하게 작성하지만 가장 큰 차이점이 parameter가 2개가 존재합니다.
  * 2개의 parameter로 2D game에서 뒤로 움직일 때 sprite를 Flip해서 움직이는 것과는 달리 Animation만 존재하면 간편하게 작성할 수 있습니다.

    ![](../../.gitbook/assets/image%20%2878%29.png)
* **2D Simple Directional과 다른 2D Freeform Directional**
  * Simple Directional과 동일하지만 같은 방향에 여러가지 모션을 추가할 수 있습니다.
  * Simple Directional이랑은 다르게 Blend Diagram을 작성할 때 여러 Motion Field를 추가하는데 좀 더 복잡한 움직임을 표현하는데 있어서 유용합니다.
  * 아래의 강의 영상은 2D Freeform Directional에 관한 내용입니다. 하지만 어려운 내용이 없고, 후반부의 Blend Tree 작성에 관한 부분을 중점으로 보시면 되겠습니다.

{% embed url="https://www.youtube.com/watch?v=32eXQhkkgR4" caption="2D Blend Animation - 2D Freeform Directional" %}

*  **2D Freeform Cartesian이란?**
  * 모션이 다른방향을 가지지 않을 때 유용합니다.
  * 하지만 구글링으로 검색한 결과 제대로 된 tutorial을 찾기 어려웠고, 굳이 사용해야하나 싶기도 해서 기회가 된다면 자세히 문서화 하겠습니다.

## Quaternion

{% embed url="https://docs.unity3d.com/kr/current/Manual/QuaternionAndEulerRotationsInUnity.html" %}

Unity의 기본좌표계는 왼손좌표계로써, x, y, z축을 가집니다. 이러한

그리고 회전을 할 때 각 축을 기준으로 회전을 시키는데 2개 이상의 축을 한번에 움직일 경우가 있습니다. 이런 경우 값이 한번에 움직일 경우 회전축에 치명적인 에러가 발생하게 됩니다.

**이러한 현상을 Gimbal Lock현상이라고 합니다.**

이러한 현상을 방지하기 위해 기존의 3개의 축을 가진 Euler Angles를 확장시켜 4개의 축을 가진 Quaternion이라는 개념을 사용합니다. 

* 이러한 Quaternion을 직접 사용하기 위해서 2가지 방법으로 할 수 있습니다.
  * Euler Angles를 Quaternion으로 변환\(Transform transform = Quaternion.Euler\(x, y, z\);\)
  * Quaternion type의 변수를 생성하여 사용하는 것\(Quaternion quaternion = Quaternion.identity;\)

## DOTween

{% page-ref page="../../api-component-reference/unity/dotween.md" %}

* DOTween을 적극적으로 쓰고 있습니다.
* 이 내용을 해당 문서에 다 담기는 어려운감이 있어 위의 Page Link에 기재했습니다.





