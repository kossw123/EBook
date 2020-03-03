---
description: tutorial Melee Attack
---

# tutorial Melee Attack

## 무엇을 하려고 하는가?

* 움직임, Animation에 관한 문서는 많지만 실질적으로 Game이 성립하기 위한 요소 중 하나인, Action에 관한 문서가 없었기에 해당 문서를 작성합니다.
* 이 문서에서는 2D Melee Attack에 관한 기본적인 내용을 다루고 있습니다.
* 이 문서를 가지고 상업적으로 이용하지 않습니다.

{% embed url="https://www.youtube.com/watch?v=sPiVz1k-fEs&list=WL&index=2&t=218s" caption="Melee Attack Video" %}

## 작성법

* 아래의 링크를 들어가 Asset을 다운 받던지 Asset Store에서 "Bandits"라고 검색하면 해당 Pixel Art가 나옵니다.

{% embed url="https://assetstore.unity.com/packages/2d/characters/bandits-pixel-art-104130?aid=1101lPGj&utm\_source=aff" %}

* Tilemap을 사용하여 Map Design을 간단히 합니다. 하지만 아래의 그림과 같이 각 Grid간 간격에 대해 공백을 가지고 있습니다.

![](../../.gitbook/assets/image%20%2837%29.png)

* 위의 그림을 활용할 이를 위해 조정해야 할것이 있습니다.
  * **Tilemap을 생성해 Camera : 0.9 / Grid Sell Size : x\(0.32\), y\(0.2\)정도로 조정합니다.**
  * **Camera와 Sell Size를 축소 시키고 싶지 않다면, 해당 Tilemap을 구성하는 Sprite의 Pixels Per Unit의 크기를 조정하여 확대 시킵니다.**
    * **Pixels Per Unit이란?**
      * Unit 당 얼마정도의 Pixel을 할당할 것인가에 대한 Component입니다.
      * Unity는 Unit이라는 고유 단위를 사용하며, 1 Unit = 1M\(meter\)로 사용합니다.
      * 즉, 1M당 얼마정도의 Pixel을 할당하여 Sprite를 표시할 것인가에 대한 이야기 입니다. 자세한 내용에 대한 참조는 아래의 링크로 들어가셔서 볼 수 있습니다.
    * 크기를 조정할 대상은 다운받은 Asset의 EnvironmentTiles이나, HeavyBandit, LightBandit에 적용시킵니다.
    * Camera Size가 기본적으로 5에 맞춰져 있기 때문에 Asset의 Pixels Per Unit을 32 정도 맞추는게 적당했습니다. 
    * **이러한 방법은 Pixel이 깨질 수도 있으니, 사용할 때 주의해야할 부분입니다.**

{% embed url="https://devonce.tistory.com/20" %}

* 위의 해결 방안을 채택 했다면 아래의 그림과 같이 적절하게 표시됩니다.

![&#xC704; : Camera, Sell Size &#xCD95;&#xC18C; / &#xC544;&#xB798; : Sprite Pixels Per Unit &#xCD95;&#xC18C;](../../.gitbook/assets/image%20%2868%29.png)

* Tilemap에 Tilemap Collider2D\(Used By Composite = true\), Rigidbody2D\(static\), Composite Collider2D를 추가하고 제대로 충돌되는지 확인합니다.

![&#xCDA9;&#xB3CC;&#xC744; &#xD655;&#xC778;](../../.gitbook/assets/image%20%282%29.png)

* 그리고 LightGuard Prefab은 Unpack하여 새로운 Component들을 추가합니다. 추가할 Component는



