---
description: tutorial Creating Believable Visuals
---

# tutorial Creating Believable Visuals

## 무엇을 하려고 하는가?

{% embed url="https://learn.unity.com/tutorial/creating-believable-visuals?language=en\#" %}

{% embed url="https://drive.google.com/open?id=1zo9BHEZg12LW8asMxNeIBw0jJ0gLY7jf" %}

* 아래의 Cinemachine + TimeLine기능의 프로젝트를 위한 SpotLight Tunnel에서의 Setting에 대해서 R&D합니다.

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

