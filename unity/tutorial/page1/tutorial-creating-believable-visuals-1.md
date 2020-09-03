---
description: tutorial Creating Believable Visuals
---

# tutorial Creating Believable Visuals

## 무엇을 하려고 하는가?

{% embed url="https://learn.unity.com/tutorial/creating-believable-visuals?language=en\#" %}

{% embed url="https://drive.google.com/open?id=1zo9BHEZg12LW8asMxNeIBw0jJ0gLY7jf" %}

* 아래의 Cinemachine + TimeLine기능의 프로젝트를 위한 SpotLight Tunnel에서의 Setting에 대해서 R&D합니다.
* 해당 문서에 대한 내용은 개인적으로 Programming + 3D Artist의 직업군에 대한 개념이 섞여 있기 때문에 

## 작성법 및 주의 사항

해당 Document는 자습서에 대한 개념이 강하기 때문에, 샘플 프로젝트를 생성하여 각 단락에 대한 예시를 작성합니다. 해당 단락은 다음과 같습니다.

1. Where to Start? \(내부 렌더링을 탐색하기 전 설정해야할 값\)
2. Preparing Unity Render Settings\(믿을만한 시각적 조건을 위한 목표 수\)
3. Lighting Strategy\(Scene에 대한 조명 전략\)
4. Modeling\(사전 제작 전 세워야 할 계획 및 모델링 전략\) 
5. Standard Shader/Material PBS and texturing\(표준 Shader와 Material PBS 및 Texturing 전략\)
6. Lighting and Setup\(조명 및 설정\)
7. Understanding Post Process Features\(포스트 프로세스\(후처리\)에 대한 특징 및 이해\)
8. Dynamically Lit Objects\(동적 조명 개\)
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

해당 단락에서는 게임에서의 조명에 대한 중요함과, 어떻게 설정하는 것이 좋은가에 대해 설명하고 있습니다.





