---
description: TR Melee Attack
---

# TR Melee Attack

## 무엇을 하려고 하는가?

* 이전의 Tilemap 문서를 작성하면서 발생한 issue가 해당 문서를 작성하면서도 발생했기 때문에 기술문서에 작성하게 되었습니다.
* **Game을 제작하다가 Pixel 사이의 미세한 간격이 생기는 문제가 자주 발생하게 됩니다. 이때는 Sprite Editor를 사용하거나 PPU를 이용하여 맞춰주시거나, Pixel Perfect라는 Asset을 import하여 사용하시면 해결됩니다.**
* 이 문서는 상업적으로 이용하지 않습니다.

## Pixel Per Unit과 Camera, Sprite Editor

* **Pixel Per Unit이란?**
  * Unity에서는 고유의 단위를 사용하는데 1M당 1Unit로 대체 됩니다.
  * 1Unit은 임의의 Pixel로 구성될 수 있습니다. 이를 1Unit에 몇개를 넣을지 조정하는게 Pixel Per Unit입니다.
  * Melee Attack의 Tilemap과 Object를 배치 할 때, 크기가 안맞아서 GridSize를 조절하거나 Pixel Per Unit을 조절하여 크기를 조정했는데, LightBandit Object는 아래 그림과 같이 1Unit당 기본이 100으로 설정되어있습니다.

![LightBandit Object Sprite](../../../.gitbook/assets/image%20%2896%29.png)

* **Pixel Per Unit을 조정한다면?**
  * Pixel Per Unit이 100이라고 한다면, 1 Unit당 100개의 Pixel을 할당하는데 Sprite Editor를 보면 위 그림 하단부분의 512 \* 256의 가로세로 크기를 가집니다.
  * Sprite Editor로 Automatic Slice를 한다면 48 \* 48의 크기를 가지게 됩니다.
  * 이 결과는 512 \* 256의 크기에서 한 동작당 48 \* 48의 크기를 확인시켜줍니다.
  * 기존의 Main Camera Size가 1로 설정하고 Project View -&gt; Bandit - Pixel Art -&gt; Sprites -&gt; LightBandit의 PPU를 48로 바꿔준다면 아래의 그림과 같이 설정됩니다.

![LightBandit PPU&#xB97C; 48&#xB85C; &#xBC14;&#xAFBC; &#xACB0;&#xACFC;](../../../.gitbook/assets/image%20%28116%29.png)

**즉, Grid Toggle을 켰을 때 확인되는 Grid 1칸당 1 Unit을 가지고 한 동작을 1:1로 Grid 1칸에 대응 시키려면 PPU를 48로 설정하면 해결된다는 것입니다. 그 결과 pixel간의 간격을 딱 맞출 수 있습니다.**

* **Pixel Per Unit와 Camera사이의 연관관계**
  * Camera가 orthographic일 때 Camera의 Size 수치의 2배를 한 것이 Camera의 세로 Unit입니다.
  * 만약 Size가 n이라면 2n Unit이 Camera의 세로 Unit입니다.
  * 가로 Unit은 사용자의 화면 비율에 따라 달라집니다.
  * **즉, Size의 크기에 따라 orthographic Camera의 크기가 정의 됩니다.**
  * **결국 Camera의 세로 Unit은 \(Size\*2\)Unit이며 설정값에서 PPU의 값에 따라 1Unit의 값이 달라지기 때문에, Size변경시 Sprite의 크기가 변동되는 현상이 나타납니다.**

{% embed url="https://devonce.tistory.com/20\#recentComments" %}

**일반적으로 Object가 Camera에 표시 될 때 어떻게 표시되는가? 에 대한 의문이 들어서 문서로 남깁니다.** 

**먼저 어떤 Camera Projection\(보여지는 방법\)인가에 따라 다릅니다.**

* **Orthographic와 Perspective?**
  * Orthographic\(직교\) and Perspective\(원근\)의 차이는 우리가 공간을 볼 때 어떻게 보는가에 대한 방법입니다.
  * Orthographic\(직교\)
    * 원근감이 없기 때문에 z축에 따른 거리 표현할 수 없습니다.
    * 2D게임에서 일반적으로 사용합니다.
  * Perspective\(원근\)
    * 원근감이 있기 때문에 z축에 따른 거리를 표현할 수 있습니다.
    * 3D게임에서 일반적으로 사용합니다.

{% embed url="https://docs.unity3d.com/Manual/CamerasOverview.html" caption="Camera Document" %}

**보여지는 방법은 그렇다 쳐도 카메라에 어떻게 투영되는가? 에 대한 의문이 있을 수 있습니다.**

**이것은 Unity에 내장되어 있는 Graphic PipeLine에 따라 다릅니다. 자세한 내용은 아래 Page Link에 문서를 보시기 바랍니다.**

{% page-ref page="../../../api-component-reference/unity/untitled.md" %}

\*\*\*\*

\*\*\*\*

\*\*\*\*

* **Sprite Editor 사용하기**

![LightBandit Sprite Asset](../../../.gitbook/assets/image%20%2868%29.png)

보통 2D 게임을 작성시 위의 그림과 같이 하나의 Character 당 왼쪽상단 부분의 Slice를 사용해서 여러개의 Sprtie를 나눠서 쓰게 되어있습니다. 이때 Asset은 투명한 배경색을 사용하여 뒷 배경에 방해되지 않는지 확인해야 합니다.

이때 Type을 설정할 수 있습니다.

* Automatic : 자동으로 구역을 나눕니다.
* Grid by Cell Size : 한 동작당 Cell Size를 파악해 나눕니다.



  ![](../../../.gitbook/assets/image%20%2874%29.png)

  * Pixel Size : 동작당 크기를 어느정도 설정할 것인가에 대한 설정값
  * Offset : Sprite의 기준점이라고 할 수 있습니다. 항상 Sprite가 같은 환경에서 나오는 값이 아니기 때문에 Offset을 사용하여 보정값으로 활용합니다.
    * 한 동작당 잘라진 Sprite가 있을 때 Offset은 항상 왼쪽 최상단을 \(0, 0\)으로 설정합니다. 이를 \(0, 0\)이외의 값으로 설정 한다면 아래의 그림과 같이 위치하게 됩니다.
    * Offset을 \(5, 5\)로 설정한 LightBandit Sprite

      ![](../../../.gitbook/assets/image%20%2842%29.png)
  * Padding : Sprite Editor상에 표시된 Grid들 간의 간격을 설정하고 싶을 때 사용됩니다.

  ![](../../../.gitbook/assets/image%20%28123%29.png)

* Grid by Cell Count : Grid by Cell Size와 달리 행과 열을 입력하여 Grid를 분배합니다. 그 외의 설정값은 동일합니다.



## 마치며

* Sprite를 맞추기 위해 겪었던 시행착오를 겪었기 때문에 이번 문서는 쉽게 작성했습니다.
* 하지만 이러한 Sprite를 조절하기 때문에 나오는 Transform의 Speed문제와 설계 단위에서 Collider Size에 대한 조정이 매번 Test마다 겪었기 때문에 불편하다고 느껴졌습니다.
* Scripting에서는 어떤 게임에서든지 비슷한 Logic으로 돌아가기 때문에 문제는 없었지만, Editor상에서 조절해야하는 부분이 더욱 부각되어 이 Melee Attack문서는 해볼만한 가치가 있었습니다.

