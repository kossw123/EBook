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
      <th style="text-align:center">&#xCD94;&#xC0C1;&#xC801;</th>
      <th style="text-align:center">&#xAD6C;&#xCCB4;&#xC801;</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:center">
        <p>1. Vertex Shader</p>
        <p>2. Shape Assembly</p>
        <p>3. Geometry Shader</p>
        <p>4. Rasterization</p>
        <p>5. Fragment Shader</p>
        <p>6. Tests and Blending</p>
      </td>
      <td style="text-align:center">
        <p>&#xCD08;&#xAE30; &#xC900;&#xBE44;&#xBB3C; : Input&#xC744; &#xC785;&#xB825;&#xD560;
          Vertex / Index Buffer</p>
        <p>1. Input Assembler</p>
        <p>2. Vertex Shader (&#xC870;&#xC791; &#xAC00;&#xB2A5;)</p>
        <p>3. Tessellation (&#xC870;&#xC791; &#xAC00;&#xB2A5;)</p>
        <p>4. Geometry Shader (&#xC870;&#xC791; &#xAC00;&#xB2A5;)</p>
        <p>5. Rasterization</p>
        <p>6. Fragment Shader (&#xC870;&#xC791; &#xAC00;&#xB2A5;)</p>
        <p>7. Color Blending</p>
        <p>&#xACB0;&#xACFC; : &#xACF5;&#xC815;&#xC744; &#xAC70;&#xCCD0; &#xB098;&#xC628;
          Data&#xB294; FrameBuffer&#xC5D0; &#xC4F0;&#xC5EC;&#xC9C4;&#xB2E4;.</p>
      </td>
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



### 위의 도표에서 과정을 알았는데, 각 과정에 대한 설명이 필요한가?

현재 작성자는 저런 방식을 통해 꼭 그래픽스를 실행해야하는가? 에 대한 답을 찾지 못한 상태이지만,  
그나마 납득할 수 있는 답변이 존재하긴 했다.

* 저렇게 하는게 가장 쉽다.

도표를 보면 여러가지 Shader들과 과정을 통해 하나의 그림을 출력하는데 각 과정마다 조금 자세하게 들여다 본다면, 수학적 관점과 컴퓨터학적 관점이 가득 들어가 있는 것을 확인할 수 있었다.

* 현재 세상을 양분하는 API\(OpenGL, DirectX\)가 도표와 비슷하게 돌아간다.
* 물론 저렇게 안돌아 갈수 있겠지만, 여러 문서를 보면 비슷하게 서술 되어 있었다.

하나의 그림을 출력하는데 모든 과정에 Overflow나 Exception이 발생하지 않는다는 가정하에서만 제대로 돌아가는 것이기 때문에, 각 과정마다 하나의 오류라도 있으면 안된다.  
그리고 Programmable Pipeline은 각 과정에 프로그래머가 직접 최적화를 하는 것이기 때문이기도 하다.



### 그렇다면 각 과정은 어떤방식으로 돌아가는가?

작성자는 2가지 관점으로 나눠서 이해했다.

* 컴퓨터과학적 관점
* 수학적인 관점



#### 컴퓨터학적 관

위에서 설명한 도표와 같은 과정으로 동작한다.

* 추상적
  1. Vertex Shader
     * Display에 표시하기 위해 3D 좌표를 2D로 변경한다.
  2. Shape Assembly
     * Primitive Shape\(기본 삼각형\)을 생성하고 Vertex Shader에서 변경한 Vertex Data\[1\]를  Primitive Shape에 대응 시킨다.
  3. Geometry Shader
     * 생성된 Primitive Shape에서 새로운 꼭지점을 생성해 또 다른 Primitive Shape를 생성한다.
     * 위 과정을 반복해서 모델링한 Object가 될 때까지 반복
  4. Rasterization
     * 최종 화면의 해당 Pixel\[3\]에 Geometry Shader의 결과를 전달해 매핑\[2\]하여 Fragment Shader가사용할 Fragment\[4\]를 선택한다.
     * 선택하기 전에 Cliping등 카메라에 안보여질 면에 대한 처리를 해서 보이지 않는 면에 대한 Fragment를 제거한다.
  5. Fragement Shader
     * Pixel의 최종 색상을 계산한다.
     * 여기에는 조명, 그림자, 조명의 색상등이 들어있다.
  6. Tests and Blending
     * Fragment의 깊이값을 확인하고 Fragment를 다른 Object의 앞이나 뒤에 있는지 확인하여  폐기한다.



* 구체
  1. Input Assembly
     * Input\(여기서는 Vertex Data\)를 정리한다.
  2. Vertex Shader
     * Vertex Data를 2D 화면\(Screen Space\)로 변경
  3. Tessellation
     * 물체의 화질을 높이기 위해 Primitive Shape\(삼각형\)을 특정한  규칙에 따라 작은 삼각형으로 나눈다.
  4. Geometry Shader
     * Primitive Shape를 지우거나 추가하는 부분이다.
  5. Rasterization
     * 2D Screen Space로 변경된 위치를 사용해 Mesh를 Fragment로 나누는 단계
  6. Fragment Shader
     * Rasterization에서 살아남은 Fragment의 색상 및 Depth를 계산하는 부분
     * 색상을 계산하기 위해 빛과 같은 색상에 영향을 끼치는 요소를 고려해야하는 부분
  7. Color Blending
     * Fragment의 색상들을 비교하여 최종 색을 결



* 첨부 설명

\[1\] : Vertex Data  
꼭지점이나 다른 도형을 그리기 위한 추가적인 정보  
보통 Vector3\(float 3개 Struct\)로 이루어져 있다.

\[2\] : 매핑  
하나의 값이 다른 값을 가리키게 만드는 것

\[3\] : Pixel  
모니터에 위치한 하나의 Dot\(점\)을 의미한다. 2D 좌표로 햇갈릴 수도 있지만, 엄연히 다른개념이다.  
2D 좌표는 특정 Point를 정확히 나타내는 위치다.

\[4\] : Fragment  
특정 물체를 그리는 과정에서 포함하고 있는 모든 정보.  
다른 용어도 그러하듯, 여러가지 의미가 내포되어 있다.

* Android에서의 Fragment : Activity에 포함되는 UI의 "일부"
* React에서의 Fragment : DOM에 별도의 Node를 추가하지 않고 여러 Children을  "하나의 파편\(일부\)"로 만드는 Class

Shader에서도 마찬가지로 Pixel의 위치, 색, 깊이 텍스쳐등 하나의 Pixel을 채우기 위해 필요한 모든 정보를 지닌 기본단위이다.  




#### 수학적 관점

![](../../../.gitbook/assets/image%20%28271%29.png)

위의 그림을 보면 각 Coordinate\(좌표계\)에서 Matrix를 통해 Transformation을 수행 후  
최종적으로 Display까지 표시되는 수학적인 관점에서의 그래픽 파이프라인이다.

위의 그림은 OpenGL을 바탕으로 작성되어 있다.

Matrix 관련 변환 행렬\(Transformation Matrix\)부분은 방대한 양때문에 해당 문서에서는 생략했다.  
하지만 아래의 링크에서 자세하게 나와있기 때문에 참고자료로 대체한다.

{% embed url="http://www.opengl-tutorial.org/kr/beginners-tutorials/tutorial-3-matrices/" %}



구글링에서 여러 정보는 찾아볼 수 있으나, 파편화된 정보를 가지고 하나로 이어 붙인다는 것은 굉장한 시간이 걸리기 때문에, 그래픽스 관련된 책을 보는 것을 추천한다.

그리고 책에는 그래픽스 파이프라인에 대한 정보가 아주 자세하게 나와있다.

{% embed url="https://www.hanbit.co.kr/store/books/look.php?p\_code=B1779572378" %}





### 과정과 그에 대한 설명이 대략적으로 이해했는데, 또 다른 파이프라인 그림들이 존재한다.

![https://m.blog.naver.com/jsjhahi/206651669](../../../.gitbook/assets/image%20%28268%29.png)

그래픽스 파이프라인이라고 구글링하면 가장 상단에 위치한 문서의 그림을 보면  
이제까지 설명한 그림과는 다른 과정이 전개된다.

* 그렇다면 어느 것이 맞는 것인가?
  * 둘 다 맞다.



* 하지만 과정을 비교해보자면, 비슷한 것 같지만 여러 가지가 추가된 것 같은데?
  * 위의 과정을 좀 더 자세하게 따져보자면, 그림과 같은 과정을 거치는게 맞다.



* 그리고 여러가지 추가한 것 이외에도, 과정이 안맞는다는 생각이 든다. 카메라 공간의 전개는 무엇이고,  다른 파이프라인이 추가된 것 같다.
  *  다른 파이프라인과 더불어 Pixel Shader도 추가된 것인데, 위의 과정은 D3D\(DirectX 3D\)에 대한 파이프라인 이며, 비슷하면서도 다른 이유는 업체가 다르기 때문이다.



* 그렇다면 각 업체마다 다른 파이프라인을 가지고 있는데,  Unity는 OpenGL을 사용하고 있고, Unreal은 DirectX 11을 사용하고 있는데 엔진을 바꿀 때, 혹은 엔진끼리 연동할 때 위의 과정을 모두 알아야 하는게 아닌가?
  * 그럴 필요까진 없다. 어차피 원리는 비슷하고 비슷한 원리에서 확장해 나가면  나중에 햇갈릴 이유도 없고, 다른 파이프라인이 나와도 원리를 이해 했기 때문에, 좀 더 수월하게 이해할 수 있다는 장점이 있다.



* 그러면 그 원리가 뭔지?
  * 아래에 나름대로 이해한 모든 것을 도표화 시켜봤다. 여러 문서에서 참고 했으며 작성자에게 가장 맞는 내용인 듯 하기에 정리했다.



### 그래픽스 파이프라인 도표

{% embed url="https://itigic.com/pipeline-3d-this-is-how-all-gpus-render-graphics/\#Pixel\_Shaders" %}



위의 링크에 따르면 파이프라인을 2가지로 나눠서 이해할 수 있었다.

* 월드 스페이스 파이프라인
* 스크린 스페이스 파이프라인







