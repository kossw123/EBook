---
description: tutorial Creating Believable Visuals
---

# tutorial Creating Believable Visuals

## 무엇을 하려고 하는가?

{% embed url="https://learn.unity.com/tutorial/creating-believable-visuals?language=en\#" %}

{% embed url="https://drive.google.com/open?id=1zo9BHEZg12LW8asMxNeIBw0jJ0gLY7jf" %}

* 아래의 Cinemachine + TimeLine기능의 프로젝트를 위한 SpotLight Tunnel에서의 Setting에 대해서 R&D합니다.
* 해당 문서에 대한 내용은 개인적으로 Programming + 3D Artist의 직업군에 대한 개념이 섞여 있기 때문에 알아두면 좋은 정보입니다.

{% hint style="info" %}
작성자가 사용하고 있는 2020.1.3f1 Version에서 각 Render Pipleline에 대한 Package의 설치로 **Wizard가 추가되어 따로 설정이 필요없이 버튼 하나로, 기본적인 아래의 내용에 대한 자동화가 이루어 집니다.**

하지만 기본적으로 Legacy Project의 HDRP, URP의 변경을 할 시, 사용했던 Material들에 대한 Shader가 다르기 때문에, 따로 조정이 필요합니다.

Wizard에 기본적인 Material을 HDMaterial로 변환하는 기능이 있으니 활용하시면 될 것 같습니다.
{% endhint %}

## 작성법 및 주의 사항

해당 Document는 자습서에 대한 개념이 강하기 때문에, 샘플 프로젝트를 생성하여 각 단락에 대한 예시를 작성합니다. 해당 단락은 다음과 같습니다.

1. Where to Start? \(내부 렌더링을 탐색하기 전 설정해야할 값\)
2. Preparing Unity Render Settings\(믿을만한 시각적 조건을 위한 목표 수\)
3. Lighting Strategy\(Scene에 대한 조명 전략\)
4. Modeling\(사전 제작 전 세워야 할 계획 및 모델링 전략\) 
5. Standard Shader/Material PBS and texturing\(표준 Shader와 Material PBS 및 Texturing 전략\)
6. Lighting and Setup\(조명 및 설정\)
7. Understanding Post Process Features\(포스트 프로세스\(후처리\)에 대한 특징 및 이해\)
8. Dynamically Lit Objects\(동적 조명\)
9. Sample Project File

**시작하기 전에 해당 단락은 원래는 이미 작성한 Graphic Rendering Pipeline 문서에 작성하려고 했으나, 방대한 양의 문서를 또 추가하기에는 양이 많이 때문에 따로 빼낸점 양해 부탁드립니다.**

**또한 기존의 문서 작성 방식인 tutorial, How-to-guide, Explanation, Technical reference방식이 아니라, tutorial, 용어 정리 및 필요에 따라 문서를 따로 생성할 예정입니다.**



### 1. Where to Start

해당 단락에서는 렌더링을 사용하기 전에 자신의 의도에 맞게 작성하기 위하여 지켜야 할 것들\(Convention\)에 대해 이야기 하고 있습니다. 해당 Convention은 아래와 같습니다.

1. **Scale and units \(규모와 단위\)**
2. **Unit translation DCC 3D software to Unity \(Digital Content Creation의 결과물을 Unity로 이식할 \)** 
3. **Point of reference scale model \(모델에 대한 기준점 or 척도\)**
4. **Texture output and channels \(Texture 출력과 채널\)**
5. **Normal map direction \(Normal Map의 방\)**

{% tabs %}
{% tab title="Scale and Units" %}
해당 문서에서는 Scale과 Units은 Scene에서 매우 중요한 역할을 하기 때문에 1Unit = 100cm로 가정하는 것이 좋다고 설명하고 있습니다.

이에 대해 생각해보면, Probuilder나, Blender, 3Ds Max, Maya등과 같이 다른 프로그램에서 작성된 결과물에 대한 규격의 통합은 필수적으로 생각됩니다. Scene에서는 규격이 곧 충돌체의 범위와 Unity에서 지형을 제작할 때의 규격을 뜻하고, 이를 통해 실사적인 부분은 아니지만, 전체적인 조화를 이룰 수 있도록 종용합니다.
{% endtab %}

{% tab title="Unit translation DCC 3D software to Unity" %}
DCC\(Digital Contents Creation\)에 대한 크기 및 규격에 대한 이야기를 하고 있습니다. 이미 위에서 얘기한 다른 프로그램에 대한 규격에 대해 이야기 하고 있습니다.
{% endtab %}

{% tab title="Point of reference scale model" %}
![](../../../.gitbook/assets/image%20%28241%29.png)

**위의 그림 처럼 Grid가 표시되는 것처럼 기준 척도를 정할 수 있는 모델이 있으면 도움이 될 수 있다고 설명합니다.**

하지만 실제적으로 외부 Package 또는 프로그램에서는 가능할지도 모르지만, 작성자는 아직까지 ProGrid이외의 Editor package는 사용하지 않았기 때문에, 해당 내용은 이해만 할 수 있었습니다.
{% endtab %}

{% tab title="Texture output and channels" %}
**Unity가 3D 프로그램과 일관된 단위 변환을 위해 미리 올바른 정보를 입력해야 한다는 점을 설명하고 있습니다.**

{% hint style="info" %}
3D Model을 작업하는 과정\(3D Modeling\)에서 물체의 표면에 세부적인 질감을 묘사하거나 색을 칠하는 것을 **Texture Mapping**이라고 합니다.

어떠한 색이나, 질감을 Map이라고도 하고, 도형 또는 면 단위의 폴리곤의 겉에 저장하는 과정을 Mapping이라고 합니다. 그에 따른 Map의 종류는 여러가지가 있는데, 대표적인 예로 아래의 그림과 같은 Map이 존재합니다.
{% endhint %}

![&#xC608;&#xC2DC;&#xB85C; &#xC124;&#xBA85;&#xD558;&#xB294; Albedo, Metalic, Normal](../../../.gitbook/assets/image%20%28240%29.png)

Substance Painte라는 프로그램에서 3D Model에 대한 설정값을 그림으로 설명하고 있습니다.

* Albedo : 색상정보만 담고 있는 Map을 의미합니다.
* Metallic : 금속 물질을 표현하는 Map입니다.
* AO\(ambient occlusion\) : 각 점이 얼마나 광원에 노출 되어있는지를 계산하기 위한 Map
* Glossiness : 광원의 반사 및 부드러움 반사율 등을 정의합니다.
* Normal : Bump map의 한 종류로써 대상의 표면에 홈 및, 스크래치와 같은 세부적인 표면을 추가할 수 있는 특수한 Map입니다.

{% hint style="info" %}
Bump Map이란 물체의 픽셀마다 표면 법선을 흔들어 높낮이가 있어 보이게 하는 컴퓨터 그래픽 기술 중 하나입니다. 자세한 것은 서술하지 않지만, 법선을 조정하여 물체의 홈을 그리는 특수한 Map이라고 생각하면 될 것 같습니다.
{% endhint %}

Document에서 중요하게 여기는 것은 Alpha Channel을 다루는 데 혼동되는 경우를 설명합니다.

{% hint style="info" %}
Alpha Channel이란 이미지에 대한 색상 값을 제외한 모든 Channel입니다.

Channel이란 일반적으로 데이터가 흐르는 개별적인 경로를 의미합니다. 그러나 Modeling부분에서도 비슷한 개념으로 쓰이는데, 각 데이터에 맞는 경로를 의미한다고 보시면 됩니다.
{% endhint %}

![](../../../.gitbook/assets/image%20%28235%29.png)

위의 그림은 혼동되는 경우에 대한 그림인데, 간략히 설명하자면 PNG 파일에서는 Alpha값이 투명하게 나오지만, TGA 파일에서는 Alpha값이검은색으로 예상되어 검은색의 질감을 가지게 됩니다.

파일형식이 다른 파일은 Alpha Channel을 통제할 수 없거나, 따로 설정을 해야하는데 이로인해 예상되는 결과값이 나올수도 있고 아닐수도 있다는 결과가 나옵니다.
{% endtab %}

{% tab title="Normal map direction" %}
앞서 Texture output and Channel 부분에서 서술했다 시피 Normal Map이라는 법선벡터의 높이를 조정하는 Map이 있는데, 프로그램마다 이를 정의하는 Pivot의 기준이 다르기 때문에, 잘못된 결과를 출력할 수 있다는 점을 설명하고 있습니다.

![](../../../.gitbook/assets/image%20%28239%29.png)

Unity에서 사용하기 위해 이를 유의하면서 제작하는 것이 불필요한 작업을 줄일 수 있는 방법이 될 수 있습니다.
{% endtab %}
{% endtabs %}



### 2. Preparing Unity Render Settings

렌더링 하기전에 Unity 자체에서 설정해야할 값\(Convention\)에 대해 서술하고 있습니다.

1. Linear rendering mode
2. Rendering mode
3. High Dynamic Range \(HDR\) Camera
4. HDR Lightmap encoding \(optional\)
5. Tonemapper for your Scene
6. Enable Image effect for viewport

{% tabs %}
{% tab title="Linear rendering mode" %}
Linear Rendering Mode에 대해 설명하기 전에 Rendering을 하는 과정에 대해 설명합니다. 아래의 링크를 참조하여 간략하게 서술합니다.

{% embed url="https://learn.unity.com/tutorial/introduction-to-lighting-and-rendering\#5c7f8528edbc2a002053b528" %}

Rendering은 어떤 오브젝트에 텍스쳐나, 빛 등 시각적인 효과를 표현하는 과정입니다. 이때, 표현하는 과정을 Pipeline이라고 하며, 이 Pipeline은 여러 종류가 있습니다. 크게 Unity에서는 아래의 4개의 Pipeline으로 분류되어 사용되고 있습니다.

1. Bulit-in Render Pipeline
2. Universal Render Pipeline\(URP\)
3. High Difinition Render Pipeline\(HDRP\)
4. Scriptable Render Pipeline\(SRP\)

여기서 해당 문서의 프로젝트는 HDRP를 사용함을 Edit -&gt; Project Setting -&gt; Graphics -&gt; Scriptable Render Pipeline Settings에서 확인할 수 있습니다.

해당 Pipeline을 사용함으로써 얻을 이점은 다음과 같습니다.

* Scriptable Render Pipeline의 한종류로써 고사양 디바이스에서 적합합니다.
* Custom Post-Processing을 사용하면 자동으로 Volume System에 통합됩니다.

이러한 고사양 디바이스에 적합한 Pipeline도 많은 설정값을 요구로 하는데 그 중 하나가 Color Space입니다.

Color Space는 말 그대로 색상 공간을 뜻하며, **사람의 눈은 빛의 세기를 선형적으로 인식하지 않기 때문에, 모니터가 기존 신호에서 보정이 된 신호를 전송하여 자연스럽게 보이는 이미지를 보여줍니다.** 이를 감마 보정이라고 하며, **선형 보정은 어떤 신호를 선형적인 구조로 바꿔서 감마보정보다 정확한 결과를 제공합니다.**

최종적으로 Shader와 Lighting에 결과를 끼치게 됩니다.

![](../../../.gitbook/assets/image%20%28237%29.png)

위의 그림은 Linear와 Gamma Space의 차이를 그리고 있는데, 좀 더 사실적으로 광원의 경계를 표현하는 것을 확인할 수 있습니다. 또 다른 예시로 아래와 같은 그림을 통해 확인할 수 있습니다.

![](../../../.gitbook/assets/image%20%28238%29.png)

위의 그림으로 빛 강도가 증가함에 따라 표면에서의 반응이 선형적으로\(빛의 강도에 따라 일정하게\) 증가함을 알 수 있습니다.

이러한 Rendering 설정을 Pipeline에 맞게 설정하여 자연스러움을 추구하는 것을 목적으로 해야한다고 해당 문서에 서술하고 있습니다.

간략히 설명하자면 다음과 같습니다.

* 물체를 표현하기 위한 Rendering 과정을 Pipeline이라고도 하는데, 현재 프로젝트에는 HDRP를 사용하고 있습니다.
* HDRP를 활용하기 위해서 Color Space라는 색상공간을 기존의 Gamma에서 Linear로 변경할 필요가 있습니다.
{% endtab %}

{% tab title="Rendering mode" %}
해당 문서는 Unity Version이 2017.3이기 때문에, Pipeline을 통한 통합설정이 아닌 개별적으로 설정하는 듯 합니다. 그렇기 때문에, 참고 문서가 존재하지 않고 내용을 보니 Rendering Mode와 Pipeline의 내용이 겹치는 부분이 많기 때문에 생략합니다.
{% endtab %}

{% tab title="High Dynamic Range \(HDR\) Camera" %}
일반적으로 Rendering은 0~1 사이의 소수에 의해 RGB가 표현하는데 실제로 조명을 정확히 반영하지 않습니다.

사람의 눈은 주변 조명에 맞추려고 하기 때문에, 조명 세기가 낮은쪽이 높은 쪽보다 밝기 차이에 민감합니다. 어두운 방에서 하얗게 보이는 물체가 밝은 방에서 회색으로 보이는 물체보다 실제로 밝지 않을수 있습니다.

이러한 2가지 특성에 의해 HDR\(High Dynamic Range\) 특성이 탄생하고, 이를 활용하면 좀 더 선명한 효과를 얻을 수 있습니다.

![](../../../.gitbook/assets/image%20%28236%29.png)

위의 Use Graphics Settings의 HDR 설정값은 Edit -&gt; Project Setting -&gt; Graphics -&gt; Tier Setting에 존재합니다.

이러한 HDR은 활성화 시키게 된다면 Scene에서 픽셀을 0~1의 범위가 아닌 범위 밖의 픽셀값들도 수용하며, 저장하고 이를 모니터에 출력을 해야하는데 HDR의 값을 모니터가 표시 못하는 상황이 존재합니다. 이때 LDR\(Low Dynamic Range\)로 변환하여 출력을 해야하는데 이를 Tone Mapping이라고 합니다.

{% hint style="info" %}
해당 문서에서는 HDR의 효과에 대해 설명하고 활성화 시키는 것에 대한 이점에 대해 설명하고 있습니다.
{% endhint %}
{% endtab %}

{% tab title="HDR Lightmap encoding \(optional\)" %}
선택적으로 적용할 수 있는 항목인데, 해당 프로젝트에서는 Lighting을 Bake하지 않았지만, 

고강도의 HDR된 Lighting으로 설정하여 작업할 계획이라면, Edit -&gt; Project Setting -&gt; Player -&gt; Other Setting -&gt; LightMap Encoding을 적절하게 설정하여 일관된 Lighting을 조성하도록 추천하고 있습니다.
{% endtab %}

{% tab title="Tonemapper for your Scene" %}
HDR의 데이터를 모니터에 출력하기 위해 LDR로 변환하는 과정을 ToneMapping이라고 합니다.

해당 문서의 프로젝트에서는 Post Processing Package를 Import된 상태이며, Project View에서 Post-Processing Profile을 생성하여, Color Grading Effect를 추가한다면 항목 아래 ToneMapping Mode에서 ACES로 설정하라고 합니다.

ACES는 영화적인 느낌을 위해 근사치\(실제 값과 가까운 값을 사용\)를 사용하여 Neutral Mode보다 대비가 더 강하고 실제 색상 및 채도에 영향을 미치는 Mode입니다.

Dithering 활성화에 관한 내용은 현재 Post Processing Package에 Dithering효과가 없기 때문에 생략하겠습니다.

{% hint style="info" %}
요점은 HDRP을 제대로 활용하려면 HDR로 밝기 데이터를 받을 필요가 있으며, 이를 모니터에 제대로 출력하기 위해 Post Processing을 이용하여 ToneMapping을 한다는 것입니다. 
{% endhint %}
{% endtab %}

{% tab title="Enable Image effect for viewport" %}
현재 HDRP를 활용하기 위해 Color Space를 Linear로 변경하였으며, 이를 HDR을 통해 휘도가 0~1의 이외값 까지 Camera에 Rendering되야 하기 때문에, ToneMapping을 통해 LDR로 바꿔서 최종이미지를 출력한다는 이야기였습니다.

해당 문서에서는 간단하게 ViewPort를 통해 이미지를 활성화 시키라는 이야기입니다. 이를 통해 지속적으로 변경된 결과를 얻을 수 있고, 개선이 빠르다는 장점을 설명하고 있습니다.
{% endtab %}
{% endtabs %}



### 3. Lighting Strategy

해당 단락에서는 게임에서의 조명에 대한 중요함과, 어떻게 설정하는 것이 좋은가에 대해 설명하고 있습니다. 해당 전략의 가장 중요한 점은 시간의 절약과, 성능, 시각적 충실도를 높일 수 있다는 것인데, 이를 위해 조명의 3가지 구성요소로 나눠서 설명하고 있습니다.

* 조명의 3가지 구성요소
  * Hemisphere\(하늘\)
  * Direct light\(태양 + 지역 조명\)
  * inDirect light\(반사 조명\) 

간단하게 보이지만 실시간 조명, 혼합 조명, 베이크 된 조명, 정적 및 동적 오브젝트를 혼합하고 일치시키는 방법을 선택한다면 결국 여러가지 갈래로 나뉘어져 수많은 경우의 수를 낳습니다.

일반적으로 5가지의 조명 설정을 사용합니다.

1. Basic realtime
   1. Baked
2. Mixed Lighting
3. Realtime Light and GI
4. Guns Blazing all options enabled

{% tabs %}
{% tab title="Basic realtime" %}
기본 실시간 조명 설입니다.

Console 및 PC에 초점을 두고 있습니다.

일반적으로 시각적 프로젝트 및 프로토타입 단계에서 사용됩니다.

* 이점
  * 모든 Direct Light와 그림자는 실시간이므로 움직일 수 있습니다.
  * 사전 계산이나 Bake 및 Mesh의 계산이 필요가 없기에 반복적인 호출이 빠릅니다.
  * 동적, 정적 Object는 동일한 방법을 사용해서 조명되고 Light Probe를 사용하지 않습니다.

{% hint style="info" %}
Light Probe?

Scene에 Light가 없어도 Realtime Lighting과 유사한 효과를 줍니다. Scene에 Light가 있는 곳 주변에 Light Probe를 배치하고 Baking할 때 Light Probe는 미리 주변부의 광원 데이터를 저장합니다.

그리고 실시간으로 Dynamic Object\(움직이는 물체\)에 광원 데이터를 전달하여 해당 객체를 보간 시켜 마치 실시간 조명과 같은 효과를 냅니다.
{% endhint %}

* 단점
  * Hemisphere의 법선 벡터의 높낮이를 계산하는 Occlusion이 없고, 오직 직선형의 Skybox와 주변 데이터와 색에 Direct Light로 비추지 않습니다.
  * GI\(Global Illumination\)이 없으며, 간접조명이 없으면 제대로된 시각적 효과를 못낼 수 있습니다.

![](../../../.gitbook/assets/image%20%28248%29.png)
{% endtab %}

{% tab title="Baked" %}
모든 조명을 Baked하여 Light Probe를 통해 광원 데이터를 미리 저장하는 설정입니다.

PC, Console VR, 저가형 PC를 대상으로 사용되는 설정입니다. 일반적으로 런타임이 길지만, Isometric Mobile Game, 높은 프레임의 VR Game에 사용됩니다.

* 이점
  * 모든 조명은 Static Object에 대해 Baked 되고, 주변 Ambient Occulsion 및 inDirect Light를 생성합니다.
    * 즉, Static Object의 Lighting을 계산하고, 주변의 흠집같은 물체의 높낮이와, 반사되는 빛을 생성합니다.
  * 지역에 대한 Light Baked를 지원하고 그림자 각도를 Static Object에 적용할 수 있습니다.
  * 모든 설정중 런타임 성능이 가장 빠릅니다.
* 단점
  * Lighting이 미리 Baked되기 때문에, Lighting 계산속도를 늦출 수 있어서 Progressive Light Mapper를 사용하지 않는 경우 Scene 변경시 다시 빌드 해야합니다.
  * Dynamic lit Object는 Light Probe만 사용하여 조명됩니다.
  * Cube Map / Reflection\(반사\)에 의존 시 반사광이 없습니다.
  * Dynamic Object들의 그림자가 없습니다.
  * Scene에 사용된 Light Texture의 크기에 따라 런타임의 메모리 소모가 다릅니다.
  * UV Texture의 제작이 필요할 수 있습니다.

{% hint style="info" %}
Progressive Light Mapper?

Bake된 LightMap을 제공하는 Fast Path - Tracing 알고리즘을 기반으로 한 System입니다. Scene Geometry 위에 오버레이 됩니다.
{% endhint %}
{% endtab %}

{% tab title="Mixed Lighting" %}
Shadow Mask + Light Probe를 Mixed Lighting입니다. ShadowMask를 설정하면 Realtime Direct Lighting과 Baked inDirect Lighting을 결합합니다.

VR, Console, PC를 대상으로 태양의 움직임과 같은 Realtime Lighting이 중요하지 않는 Game에서 사용됩니다.

* 이점
  * 모든 Baked된 Light와 비슷하지만 Mixed Lighting에서 Dynamic Object는 Realtime Specular\(실시간 반사광\)을 얻고, Realtime Shadow를 그립니다.
  * Static Object는 Baked된 ShadowMask를 가져와서 더 나은 시각적 품질을 제공합니다.
* 단점
  * Object당 4개의 ShadowMask만 가질 수 있습니다.
  * Realtime Lighting을 위해 런타임에 추가적으로 성능을 사용합니다.
  * 특정 설정에서 큰 영향을 끼칠 수 있습니다.
{% endtab %}

{% tab title="Realtime Light and GI" %}
GI + Light Probe를 통해 전역적인 조명과, Dynamic Object의 광원데이터를 미리 할당합니다.

Console, PC를 대상으로 시간에 따른 조명의 Update가 필요하고, Dynamic Lighting이 필요한 야외를 그리는 Game에서 사용합니다.

* 이점
  * Realtime inDirect Lighting으로 빠른 Lighting update가 가능합니다.
  * Dynamic 및 Static Object는 Realtime Specular\(실시간 반사광\) 및 그림자를 얻습니다.
  * Baked된 Lighting보다 적은 메모리를 사용합니다.
  * GI Update를 위한 고정된 CPU 비용의 시간적 성능이 향상됩니다.
* 단점
  * 렌더링된 Occulsion은 Baked Lighting 만큼 섬세하지 않으며, 일반적으로 SSAO 및 Object별 AO를 사용해서 보충해야 합니다.
  * Static Object에 대한 지역, Light Angle\(빛이 분산, 방출되는 각도\)  그림자가 없습니다.
  * Realtime Lighting은 특정 설정을 하면 성능에 큰 영향을 끼칩니다.
    * 최적화된 UV 설정없이 Static Lighting에 기여하는 Object가 많은 경우 사전에 Lighting Generation 시간이 상당히 오래 걸립니다.
{% endtab %}

{% tab title="Guns Blazing all options enabled" %}
모든 설정에 대한 이해와 각 조명에 대한 조합을 이해하는 어려운 조건을 충족했을 시 사용할 수 있는 설정입니다.

PC, Console을 대상으로 메모리 사용량 및 성능이 제한되어 많은 요구사항이 있는 Game에서 활용합니다.

* 이점 
  * Lighting과 관련된 모든 기능을 사용할 수 있으며, 제작자에게 모든 기능을 제공합니다.
* 단점
  * 런타임, 메모리 사용시 성능을 미리 Baked하여 할당합니다.
    * 애초에 사용하기 어렵기 때문에, 어느 부분에서 비용이 높을 경우 직접적으로 수정을 해야하며, 이는 어려운 난이도를 띄고 있습니다.
  * 설정하는데 시간이 너무 오래걸립니다.
    * UV 제작 및 Baked 하는데 걸리는 시
{% endtab %}
{% endtabs %}



### 4. Modeling

해당 단락에서는 프로젝트를 진행하기 전 Modeling을 설계하는 것이 다음과 같은 이점을 가지고 있다고 서술하고 있습니다. 3D Modeling 전반 프로세스를 다루지 않기 때문에, 각 내용마다 아이디어의 단초를 제공하는 역할을 하고 있습니다.

즉, 하나의 흐름을 가지고서 아래 내용을 읽지 않는 것을 권장합니다.



Static Object에 대한 LightMap을 표시하는 것은 불필요한 일이라고 서술합니다.

![](../../../.gitbook/assets/image%20%28264%29.png)

* Geometry의 Lighting mode를 기초로한 Modeling 규칙이 필요합니다.
  * Modeling을 구성하는 Mesh를 보는 Player에게 굳이 보이지 않는 Geometry를 작성하여 불필요한 성능의 낭비, 시간낭비를 할 필요가 없습니다.
  * Object에 Lighting을 그리고 싶다면, GI를 사용하여 LightMapping의 LightMap에 대한 적절한 값을 넣는 것이 효율적입니다.
* Dynamic한 Light와 Light Probe에서만 조명을 받는 Object의 경우 Geometry에 대한 UV 제한이 없습니다. 

{% hint style="info" %}
Geometry??

기하학\(幾何學\)이라고 번역되지만, 컴퓨터에서는 렌더링에 있어서 필요한 Mesh의 치수, 모양, 상대적 위치 등을 포함하고 있는 데이터입니다.

보통 좌표계나, 2D 변환, 3D 변환 같은 개념들이 포함되어 있지만, 정확한 의미는 찾지 못했습니다. 나름 이해한 방식으로 정리를 하자면, Mesh를 그리는데 있어서 필요한 데이터라고 정리하겠습니다.
{% endhint %}

* Model의 UV Layout을 전략적으로 설계합니다. 아래의 예시는 각 Map에 대한 UV값을 보여주고 있습니다.
  * Normal Map Baking\(UV1\) / 보통 2D Lighting
  * Light Map Baking\(UV2\) / 보통 3D Lighting
  * Realtime Light Map\(UV3\) / 실시간 Lighting
* Tile로 맞추는 것처럼 딱 맞지 않는 Texture가 있는 경우 Geometry에 대한 동일한 메모리 공간을 사용하면서 시각적 품질을 개선할 수 있습니다.

{% tabs %}
{% tab title="UV1차트의 경우" %}
![](../../../.gitbook/assets/image%20%28265%29.png)

필요한 만큼 분할하고 Normal Map Baking을 위해 비어있는 공간을 최소화하여 효율적으로 배치하는 것이 중요합니다.
{% endtab %}

{% tab title="UV2 차트의 경우" %}
![](../../../.gitbook/assets/image%20%28251%29.png)

왼쪽의 경우 Lighting된 Mesh의 Shadow가 고르게 분포되지 않는, Seams\(이음새\) issue가 발생하는 경우고 오른쪽의 경우 issue를 해결한 경우입니다.

이를 위해 일관된 Light Map texels을 고르게 분포시켜야 하는데 이는 일관된 UV chart와 Shell 간의 일정한 Scale을 유지해야 합니다.
{% endtab %}

{% tab title="UV3 차트의 경우" %}
실시간으로 Lighting을 하기 위해 GI를 사용하며, 이 경우 Model에서 큰 표면을 나타내는 넓은 영역에 대한 UV 공간의 우선순위를 첫번째로 잡습니다.

자세한 내용은 아래의 링크에 있습니다.

{% embed url="https://learn.unity.com/tutorial/precomputed-realtime-gi-global-illumination\#5c7f8528edbc2a002053b559" %}
{% endtab %}
{% endtabs %}



### 5. Standard Shader/Material PBS and texturing

해당 단락에서는 물체의 표면을 사실적으로 표현하는 Material의 표면속성을 정의 하는데 있어서, 효율적인  3가지 전략을 제공하고 있습니다.

* 표준 Shader 사용
* Material PBS의 사용
* Texturing

#### Standard Shader의 사용

Unity에서는 각 재질별로 사용하는 Shader가 달랐기에, 사실적으로 표현하는 능력이 떨어졌었습니다. 하지만 **Standard Shader의 등장으로 모든 Material을 사실적으로 표현할 수 있게 되었습니다**.

물체의 표면을 정의하는 Material을 만들 때 Unity에서는 기본적으로 Standard Shader를 사용합니다. 기존의 사용하던 Shader는 Legacy Shader를 통해 접근할 수 있습니다.

해당 기능을 활성화, 비활성화를 통해 Material Editor의 다양한 Texture Slot과 parameter를 사용할 수 있습니다. 이때 PBR\(Physics Base Rendering, 물리 기반 렌더링\)의 지원을 받아 Material을 완벽히 표현할 수 있습니다.

{% hint style="info" %}
PBS? : 현실을 모방하는 방식으로 Material과 빛 사이의 상호작용을 표현합니다.
{% endhint %}

**결론적으로 Unity의 Standard Shader는 PBS를 포함한, 모든 Material을 표현할 수 있는 Shader라는 것입니다.**

#### Material PBS의 사용

Standard Shader를 사용하기 위해 Material에서 다음과 같은 설정이 필요합니다.

{% tabs %}
{% tab title="Standard / Standard Specular Setup의 사용" %}
Material을 보면 Standard가 있고, Standard \(Specular Setup\)이라는 항목이 존재합니다.

각각 Standard Shader를 기반으로 해 모든 Material을 표현하지만, Specular\(반사광\)의 표현을 할지 말지를 결정하는 과정입니다.

Specular Setup은 Material의 Albedo에서 Specular 색상을 제거하고 싶을 때 사용합니다.

* Standard Shader에서 Specular의 밝기와 색상은 Albedo, Metallic, Smoothness Map의 값에 따라 자동으로 계산합니다.
* Specular Shader에서는 Metallic Map 대신 Specular Map을 설정하여 Specular의 색상과 밝기를  **직접** 정합니다.
{% endtab %}

{% tab title="Standard Shader Map의 특징" %}
Standard Shader는 다음과 같은 Map을 가지고 있습니다.

* Albedo : 오직 색만을 가지고 있는 Map
* Metallic : 금속 재질을 표현하는 Map
* Normal : 법선 벡터의 높이를 추출하여 사물의 굴곡을 표현하는 Map
* Occulsion\(Ambient Occulsion\) : 각 점이 광원에 얼마나 노출되었는지 표현하는 Map
{% endtab %}

{% tab title="Standard Shader Map을 작성시 주의점" %}
1. 어두운 Albedo, 즉 RGB의 값이 낮을수록 많은 빛을 흡수하여 비정상적인 Light를 연출합니다.
2. 그렇다고 너무 밝으면 많은 빛을 반사하기에 비정상적인 Light를 연출합니다.

![Non - Metallic &#xD45C;&#xBA74;&#xC758; Albedo&#xC758; inDirect Light](../../../.gitbook/assets/image%20%28252%29.png)

3. Material Validation를 사용하면 Material값이 가이드 라인을 따라 가는지 안가는지 확인할 수 있습니다.

![](../../../.gitbook/assets/image%20%28253%29.png)

기본적인 Guideline 아래의 링크에 있습니다.

{% embed url="https://docs.unity3d.com/Manual/StandardShaderMaterialCharts.html" %}



4. Metallic Map의 값에 따라 반사되는 주변 빛을 정의하는 동시에 표시되는 Object의 표면에 Albedo의 크기도 결정합니다.

![](../../../.gitbook/assets/image%20%28259%29.png)

위 그림은 순수 Metallic Material이 빛을 얼마나 반사하고 Albedo의 색을 흡수하는지 보여주는 그림입니다.

5. Material이 아닌 최종적으로 나타는 Object의 표면에 주의를 기울여야 합니다.

![](../../../.gitbook/assets/image%20%28261%29.png)

위 그림은 페인트를 바른 금속난간에 Texture를 적용하여 Shaded에 최종적으로 그리고 있는 과정을 보여주고 있습니다. 

다른 Map들을 보면 "페인트를 바른 부분"을 제외한 영역에 Metallic으로 지정해야 제대로된 품질을 가질 수 있음을 보여줍니다. **결국 계획을 세워도 최종적인 품질에만 신경을 써야하고, Material의 값들까지 신경쓴다면 차후 변경에 어려움을 느낄 수 있다는 점이 핵심입니다.**

6. Metallic의 값을 0, 1의 사이값으로 지정해야 합니다. 부분적으로 먼지나 흙으로 덮인 금속 물체에 대한 표현이 어려워질 수 있음에 초점을 두고 있습니다.

7. Smoothness는 표면의 미세한 세부 사항을 제어하고, 0, 1의 사이값을 설정하여 최종적인 품질에만 신경을 써야합니다. 

8. Normal Map을 추가하는 것이 시각적으로 큰 효과를 가져올 수 있습니다.

![](../../../.gitbook/assets/image%20%28258%29.png)

9. Occlusion Map을 추가하는 것이 좋습니다.

Lighting 부분에서 inDirect Lighting과 Light Baking을 통해 Occlusion에 대한 설정을 했는데, Material에서 다시 설정해야하는 이유는 다음과 같습니다.

* Material에서 설정하는 Occlusion의 경우, 세부적이기 때문에 좋은 품질로부터 오는 데이터를 offline으로 렌더링 하는 동안 더 좋게 만들 수 있습니다.
* Dynamic Object가 Light Baking에서 Occlusion을 얻지 않고 Light Probe, 주변 빛에서 낮은 품질의 Occlusion값을 얻기 때문에, Object를 Dynamic하게 비추는데 엄청난 도움이 됩니다.

10. 참고용 사진을 통해 더욱 빠르게 Texture를 작성할 수 있습니다. 이때, 흐린 날이나, 빛이 고르게 분산되는 조건이 필요합니다.
{% endtab %}
{% endtabs %}



### 6. Lighting and Setup



### 7. Understanding Post Process Features

해당 단락에서는 Post Processing에 관한 이해와 특징을 살펴보면서, 이 기능을 통해 어떤식으로 화면을 설정하는지 알아보는 문서입니다.

**Post Processing은 렌더링을 최종적으로 출력하기 전에 효과를 추가하는 기능입니다.**

* 장점
  * 즉각적인 렌더링 효과의 수정이 가능합니다.
* 단점
  * 효과를 추가하는 만큼 빌드하는데 시간이 더 걸릴 수 있습니다.



* 효과
  * 안티 앨리어싱
  * Ambient Occulsion\(주변 폐색\)
    * Lighting으로 생기는 그림자에 대한 설정을 할 수 있습니다.
  * Auto Exposure
    * 어두운 부분에서 빛에 노출되는 부분에 대한 설정을 해서, 터널에서 밝은 조명에 노출 될 때 눈부심 효과를 추가할 수 있습니다.
  * Depth of Field
    * 공간의 한 부분만 초점을 잡아 영화 같은 느낌을 줄 수 있습니다.
  * Motin Blur
    * 어떤 Motion의 움직임에 대한 Blur 처리를 하여, 움직일 때 잔상효과를 줄 수 있습니다.
  * Bloom
    * 어떤 광원을 바라볼 때 초점이 맞지 않는 시각적 효과를 제공합니다.
  * Screen Space Reflection
    * 왠만하면 추가하는 것이 좋다고 설명합니다. 이 기능은, SSAO와 유사하게 화면에 렌더링 되는 부분만 반사하는 기능이며, 렌더링 성능에 영향을 줍니다.
  * Chromatic Aberration
    * 카메라에 잡히는 색상을 분산시켜서 사실감을 더해줍니다.
  * Grain
    * 후처리 하는 과정에서 사용하며, 이름과 동일하게 미세한 곡물같은 Noise를 추가합니다.
  * Vignette
    * Scene뷰의 가장자리를 어둡게 만듭니다.
  * Color Grading\(색 보정\)
    * 특정 색을 보정하여 강조된 색상에 대한 시각적 효과를 부여합니다.
  * Lens Distortion
    * 렌즈 효과를 추가합니다.





