---
description: Chapter 2. Graphics
---

# Chapter 2. Graphics

작성자의 의식의 흐름에 따라 문항이 작성되었기 때문에, 많은 혼란이 존재할 수 있음

기본적인 설치 과정은 LearnOpenGL의 번역문서에서 자세하게 볼 수 있음

{% embed url="https://learnopengl.com/Translations" caption="OpenGL Tutorial 번역문서 링크" %}



### Tutorial 보면서 Context와 Window창 까지 띄웠고, Tutorial에서 코드 부분에 대한 설명이 붙어 있지만, 충분하지 않다.

여기서 난관에 부딪혔다.

Tutorial에 보면 갑자기 추상적인 그래픽 파이프라인 사진과 함께 각 프로세스에 대한 설명을 쓰고,  
이를 바탕으로 코드를 설명함과 동시에 작성을하라고 한다.

작성이라고 해봤자 복붙이지만, 그래도 알고 쓰려면 자료를 계속 찾고 논리를 이어 붙여야 한다.

* 이제부터 작성하는 내용은 작성자의 의식의 흐름에 따른 논리이며,
* 작성자 외 타인이 보기에는 이게 뭔가 싶긴 하겠지만,
* 누군가를 이해 시킨다는 건 어렵기 때문에,
* 누군가 본다면 아래의 내용을 바탕으로 자신만의 OpenGl에 관련된 이해관계를 구축했으면 함



### 일단 OpenGL은 그래픽스\(Graphics\) 관련 API이다.

그래픽스란?

* 컴퓨터를 사용하여 그림을 생성하는 기술이다.
  * 이미 존재하는 그림에서 새로운 그림을 생성하는 기술인 영상처리\(Image Processing\)과는 다른 이야기이다.
* 그래픽스는 2가지 Category로 나뉜다.
  * 무엇을 그릴 것인가? - Modeling
  * 어떻게 그릴 것인가? - Rendering

그리고 각각의 카테고리에 대한 설명은 다음과 같이 요약된다.

* Modeling
  * Object\(그림, 혹은 그릴 대상\)을 정의하는 과정
* Rendering
  * Modeling에서 정의된 Object를 어떻게 그릴 것인가



OpenGL은 그래픽스 관련이기 때문에, Modeling Rendering 관련 함수가 존재하고 이를 사용하여 그림을   
그릴 수 있다.



### 그렇다면 Tutorial에서 볼 수 있는 과정은 대체 무엇을 뜻하는가?

개인적으로 Tutorial에서 볼 수 있는 내용은 대학에서 배울 수 있는 내용인 3D computer graphics에 대해  
알아야 이해할 수 있는 내용같다.

기초라고 하기에는 작성자의 실력이 고만고만하기 때문에 기초라고 표현했지만,  
관련 책을 보면 전체적인 과정은 렌더링 파이프라인에 대한 설명이 대부분이다.

그리고 조금 더 자세히 들어가면 수학적인 부분이 굉장히 많이 들어가기 때문에, 이해하기 쉽지 않다.

* 어찌됐든 Tutorial에서 볼 수 있는 과정은 Rendering Pipeline이다.

그러나 그래픽스 관련된 책을 보면 Tutorial에서 볼 수 있는 Rendering Pipeline과 다른 과정들이 나열 되어 있다.

이에 대한 혼란 때문에 이해를 제대로 하기 어려웠는데,   
결국 둘다 똑같은 Rendering Pipeline이라는 것을 깨달았다.



<table>
  <thead>
    <tr>
      <th style="text-align:left">&#xCD94;&#xC0C1;&#xC801;</th>
      <th style="text-align:left">&#xAD6C;&#xCCB4;&#xC801;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>1. Vertex Shader</p>
        <p>2. Shape Assembly</p>
        <p>3. Geometry Shader</p>
        <p>4. Rasterization</p>
        <p>5. Fragment Shader</p>
        <p>6. Tests and Blending</p>
      </td>
      <td style="text-align:left">
        <p>&#xCD08;&#xAE30; &#xC900;&#xBE44;&#xBB3C; : Vertex / Index Buffer</p>
        <p>1. Input Assembler</p>
        <p>2. Vertex Shader (&#xC870;&#xC791; &#xAC00;&#xB2A5;)</p>
        <p>3. Tessellation (&#xC870;&#xC791; &#xAC00;&#xB2A5;)</p>
        <p>4. Geometry Shader (&#xC870;&#xC791; &#xAC00;&#xB2A5;)</p>
        <p>5. Rasterization</p>
        <p>6. Fragment Shader (&#xC870;&#xC791; &#xAC00;&#xB2A5;)</p>
        <p>7. Color Blending</p>
        <p>&#xACB0;&#xACFC; : FrameBuffer</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">
        <ol>
          <li>3D &#xC88C;&#xD45C;&#xB97C; 2D &#xC88C;&#xD45C;&#xB85C; &#xBCC0;&#xACBD;&#xD55C;&#xB2E4;.</li>
          <li>
            <p>Primitive Shape(&#xC0BC;&#xAC01;&#xD615;)&#xC744; &#xC0DD;&#xC131;&#xD558;&#xACE0;</p>
            <p>Vertex Shader&#xC5D0;&#xC11C; &#xBC1B;&#xC740; Data&#xB97C;</p>
          </li>
        </ol>
      </td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

* 추상적인 부분은 Tutorial에서 볼 수 있는 그림을 나열한 것이고, 
* 구체적인 부분은 링크에서 따온 그래픽스 파이프라인이다.  그리고 아주 비슷하게 여러 문서에서 확인할 수 있었다.

{% embed url="https://mkblog.co.kr/2018/08/11/gpu-graphics-pipeline/" %}



### 여기서 Shader에 대한 이해를 하고 가면 좀 더 수월해진다.

Shader는 GPU에서 돌아가는 아주 작은 프로그램의 조각들이다. 

OpenGL은 원래 immdiate Rendering Mode를 사용했지만, 개발자들의 스킬업과 좀 더 좋은 최적화와 더불어 이것저것에 대한 욕심에 의해 Core - Profile Mode를 사용한다.  
즉 적어도 옛날 처럼 간편하게 작성하는게 아닌 Vertex 부터 Fragment까지 최적화를 할 수 있다는 이야기다.

Tutorial에서는 삼각형을 "렌더링"하기 위해 Vertex를 입력하고 Vertex에 들어가는 Data를 입력하기 위한 코드를 작성하는데 그 내용이 다음과 같다.

```c
#version 330 core
layout (location = 0) in vec3 aPos;

void main()
{
    gl_Position = vec4(aPos.x, aPos.y, aPos.z, 1.0);
}
```

위의 코드가 Shader고, 위 내용은 Vertex에 들어갈 Data를 입력하는 과정이다.

그래서 프로그래머가 직접 입력할 수 있기에 Programmable Function, 즉 조작 가능한 부분이라고 위의 도표에서 설명한다.

Shader가 들어가면 다 조작 가능한 부분이라고 보면 되겠다.



### 이제 그래픽스 파이프라인에 대한 이야기를 해보자

위의 도표에 대한 부가설명은 다음과 같다.

