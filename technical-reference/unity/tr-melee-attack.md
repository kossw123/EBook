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

**이것은 Unity에 내장되어 있는 Graphic PipeLine에 따라 다릅니다.**

## **Graphic Pipeline**

**graphic Pipeline은 3D 세계에 대한 기하학적인 표현과 이 세계를 바라보는 관점을 정의하는 가상 카메라를 통해 이미지를 만들어 내는 과정을 의미합니다.**

{% embed url="https://docs.unity3d.com/Manual/render-pipelines.html" %}

Unity에는 Rendering Pipeline에는 4가지 종류가 있습니다.

1. Bulit-in Render Pipeline
2. Universal Render Pipeline\(URP\)
3. High Difinition Render Pipeline\(HDRP\)
4. Scriptable Render Pipeline\(SRP\)

그리고 Rendering Pipeline은 아래의 작업들을 수행합니다.

* Culling\(특정 Pixel을 선별해서 은면 탐\)
  * 보통 3D Graphic에서 보이지 않는 면은 Rendering 하지 않습니다. 이때 Culling이라는 작업을 통해 특정 Pixel들을 선별하는 과정을 거칩니다.
  * Culing을 하는데 여러가지 기법들이 존재합니다.

{% embed url="https://www.slideserve.com/stacey/real-time-rendering" %}

* Rendering\(은면을 제외한 작성자가 보기 위한 모델 구\)
  * 실질적으로 Object를 표현하기 위한 도형의 배열, 시점, Texture, mapping, lighting, shader의 정보를 표현합니다.

{% embed url="https://parksh86.tistory.com/168" %}

* Post-processing\(후처\)
  * 원본 영상에 생동감 혹은 다른 효과를 부여하는 작업을 뜻합니다.
  * Unity에서는 Post-Processing이라는 Component를 통해 후처리 작업을 삽입할 수 있으며, 이에 대한 자세한 사용방법은 아래의 링크를 따라 문서에 가셔서 메뉴얼을 보시면 됩니다.

{% embed url="https://docs.unity3d.com/Manual/PostProcessingOverview.html" %}

각 Pipeline은 알맞는 용도와 기능이 있기 때문에 각 Pipeline마다 바꿔서 쓰긴 어렵기 때문에 게임을 설계하는데 있어서 어떤 Pipeline을 써야 할지에 대한 이해가 필요합니다.

각 Pipeline에 대한 특징입니다.

* Bulit-in Render Pipeline
  * Unity 기본 Pipeline입니다.
  * 다양한 Rendering Path를 지원합니다.
    * 1. Forward Rendering : 범용적으로 설정되어 있는 Path
      2. Deferred Shading : 조명과 그림자를 강조하는 Path
      3. Legacy Deferred : Deferred Shading와 유사하지만 다른 기술을 차용하는 Path
      4. Legacy Vertex Lit : lighting fidelity가 낮고 실시간 그림자를 지원하지 않는 Path
    * 제작 하려는 게임에 맞게 Path를 설정합니다.

{% embed url="https://docs.unity3d.com/Manual/RenderingPaths.html" %}



* Universal Render Pipeline\(URP\)
  * Scriptable Render Pipeline의 한종류로써 고사양 디바이스에서 고품질, 저사양 디바이스에서는 최적화를 지원합니다.
  * Post-Processing을 Pipeline에 통합해서 성능이 올랐습니다.

{% embed url="https://unity.com/kr/srp/universal-render-pipeline" %}



* High Difinition Render Pipeline\(HDRP\)
  * Scriptable Render Pipeline의 한종류로써 고사양 디바이스에서 적합합니다.
  * Custom Post-Processing을 사용하면 자동으로 Volume System에 통합됩니다.

{% embed url="https://unity.com/kr/srp/High-Definition-Render-Pipeline\#targets-high-end-hardware" %}



* Custom Render Pipeline\(CRP\)
  * 가장 게임에 적합한 Pipeline을 구성할 수 있습니다.
  * 작업량이 많고 그래픽스 프로그래머가 필요하기 때문에 개발난이도가 굉장히 높습니다.





