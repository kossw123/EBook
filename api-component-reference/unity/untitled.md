# Graphic Rendering Pipeline\(작성중\)

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
  * 작업량이 많고 그래픽스 프로그래머가 필요하기 때문에 개발난이도가 굉장히 높습니다

