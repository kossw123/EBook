---
description: TR Melee Attack
---

# TR Melee Attack\(작성중\)

## 무엇을 하려고 하는가?

* 이전의 Tilemap 문서를 작성하면서 발생한 issue가 해당 문서를 작성하면서도 발생했기 때문에 기술문서에 작성하게 되었습니다.
* 이 문서는 상업적으로 이용하지 않습니다.

## Pixel Per Unit과 Camera, Sprite Editor

* **Pixel Per Unit이란?**
  * Unity에서는 고유의 단위를 사용하는데 1M당 1Unit로 대체 됩니다.
  * 1Unit은 임의의 Pixel로 구성될 수 있습니다. 이를 1Unit에 몇개를 넣을지 조정하는게 Pixel Per Unit입니다.
  * Melee Attack의 Tilemap과 Object를 배치 할 때, 크기가 안맞아서 GridSize를 조절하거나 Pixel Per Unit을 조절하여 크기를 조정했는데, LightBandit Object는 아래 그림과 같이 1Unit당 기본이 100으로 설정되어있습니다.

![LightBandit Object Sprite](../../.gitbook/assets/image%20%2866%29.png)

* **Pixel Per Unit을 조정한다면?**
  * Pixel Per Unit이 100이라고 한다면, 1 Unit당 100개의 Pixel을 할당하는데 Sprite Editor를 보면 위 그림 하단부분의 512 \* 256의 가로세로 크기를 가집니다.
  * Sprite Editor로 Automatic Slice를 한다면 48 \* 48의 크기를 가지게 됩니다.
  * 이 결과는 512 \* 256의 크기에서 한 동작당 48 \* 48의 크기를 확인시켜줍니다.
  * 기존의 Main Camera Size가 1로 설정하고 Project View -&gt; Bandit - Pixel Art -&gt; Sprites -&gt; LightBandit의 PPU를 48로 바꿔준다면 아래의 그림과 같이 설정됩니다.

![LightBandit PPU&#xB97C; 48&#xB85C; &#xBC14;&#xAFBC; &#xACB0;&#xACFC;](../../.gitbook/assets/image%20%2880%29.png)

**즉, Grid Toggle을 켰을 때 확인되는 Grid 1칸당 1 Unit을 가지고 한 동작을 1:1로 Grid 1칸에 대응 시키려면 PPU를 48로 설정하면 해결된다는 것입니다. 그 결과 pixel간의 간격을 딱 맞출 수 있습니다.**

* **Pixel Per Unit와 Camera사이의 연관관계**
  * Camera는 PPU를 가지고

{% embed url="https://devonce.tistory.com/20\#recentComments" %}





