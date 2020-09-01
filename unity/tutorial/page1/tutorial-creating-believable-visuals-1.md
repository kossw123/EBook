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
![](../../../.gitbook/assets/image%20%28238%29.png)

**위의 그림 처럼 Grid가 표시되는 것처럼 기준 척도를 정할 수 있는 모델이 있으면 도움이 될 수 있다고 설명합니다.**

하지만 실제적으로 외부 Package 또는 프로그램에서는 가능할지도 모르지만, 작성자는 아직까지 ProGrid이외의 Editor package는 사용하지 않았기 때문에, 해당 내용은 이해만 할 수 있었습니다.
{% endtab %}

{% tab title="Texture output and channels" %}
**Unity가 3D 프로그램과 일관된 단위 변환을 위해 미리 올바른 정보를 입력해야 한다는 점을 설명하고 있습니다.**

{% hint style="info" %}
3D Model을 작업하는 과정\(3D Modeling\)에서 물체의 표면에 세부적인 질감을 묘사하거나 색을 칠하는 것을 **Texture Mapping**이라고 합니다.

어떠한 색이나, 질감을 Map이라고도 하고, 도형 또는 면 단위의 폴리곤의 겉에 저장하는 과정을 Mapping이라고 합니다. 그에 따른 Map의 종류는 여러가지가 있는데, 대표적인 예로 아래의 그림과 같은 Map이 존재합니다.
{% endhint %}

![&#xC608;&#xC2DC;&#xB85C; &#xC124;&#xBA85;&#xD558;&#xB294; Albedo, Metalic, Normal](../../../.gitbook/assets/image%20%28237%29.png)

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

![](../../../.gitbook/assets/image%20%28236%29.png)

Unity에서 사용하기 위해 이를 유의하면서 제작하는 것이 불필요한 작업을 줄일 수 있는 방법이 될 수 있습니다.
{% endtab %}
{% endtabs %}



