---
description: Chapter 1. OpenGL
---

# Chapter 1. OpenGL

작성자의 의식의 흐름에 따라 문항이 작성되었기 때문에, 많은 혼란이 존재할 수 있음

기본적인 설치 과정은 LearnOpenGL의 번역문서에서 자세하게 볼 수 있음

{% embed url="https://learnopengl.com/Translations" caption="OpenGL Tutorial 번역문서 링크" %}





### 맨 처음 OpenGL이란 무엇인가부터가 궁금했다.

* Open Graphics Library의 준말, 즉 그래픽 관련 라이브러리이다.



### 그렇다면 라이브러리일 뿐인데, 왜이렇게 유명한가도 궁금했다.

* 최초의 컴퓨터 애니악은 40년대에 등장했고, 최초로 개인용 PC가 보급이 된것은 70년대 부터다.
* 컴퓨터를 샀으니, 무엇인가를 계산하는 일을 맡겨야 하는데 PC 기종마다 그래픽 하드웨어와, 소프트웨어를 개발하는건 매우 힘든 일이여서 기업 단위로 개발에 들어갔다.
* SGI\(실리콘 그래픽스\)가 대표적인 기업이였고, 거기서 개발한 IRIS GL API가 사용하기도 쉽고, 즉시 렌더링 모드\(immediate Rendering mode\)를 지원하여 대표적인 API로 표준화 됬다.
* 그러나 그 이외의 기업들과 새로운 기업들이 그래픽 시장에 진입하며, SGI의 점유율을  약화시켰다.
* 그래서 SGI는 IRIS GL을 오픈시켜서 오늘날 OpenGL이 탄생

즉 SGI에서 만들었는데, 시장의 확대로 인한 IRIS GL의 오픈소스의 결과물이 OpenGL이다.



### 근데 왜 OpenGL이 현대의 그래픽 API를 양분하는 하나의 축이 된걸까?

* OpenGL은 여러 기업이 모여 표준화를 정하기 때문에, 거의 모든 그래픽과 관련된 하드, 소프트웨어에서 호환됨
* C언어 라이브러리이기 때문에, 모델링을 제외한 순수 렌더링 측면에서 보면 가장 빠름
  * 사운드 및 라이트와 같은 다른 작업들을 하려면 다른 API를 쓰거나 DirectX를 쓰면 됨



### 그러면 OpenGL API를 어떻게 설치해야 하는가?

* OpenGL은 표준화된 오픈소스이기 때문에 왠만한 IDE에서는 OpenGL에 관련된 라이브러리를 이미 포함하는게 많기 때문에, OpenGL API를 따로 받을 필요는 없다.
* 그리고 자주 쓰던 IDE에서 라이브러리를 불러 오듯 OpenGL API를 소스코드에 첨부하여 사용하면 된다.



### 근데 OpenGL tutorial들을 보면 다운받아야 하는게 많은건가?

* OpenGL은 90년대 부터 쭉 이어져온 라이브러리이기 때문에, 확장도 확장이지만 OpenGL API를 더 쉽게 쓸수 있는 API가 존재한다.

특히 OpenGL Context\(OpenGL을 사용하기 위한 외부적인 요소\)를 생성하고 Window창을 생성하는 것 까지 하려면 시간이 너무 오래 걸리기 때문에, 2차적인 API를 받아 쓰는게 좋다.

* 2차적인 API의 대표적인 것들
  * GLUT
  * SDL
  * SFML
  * GLFW

그리고 작성자는 GLFW를 다운받아 사용한다.



GLFW.zip을 다운받아 아무데나 풀면, CMake File과 그 안에 CMakeList 존재한다.   
CMake는 Build Script를 쉽게 관리해주는 프로그램으로써 GLFW.zip은 CMake에 사전정의된 관리 규칙인 CMakeList.txt를 제공하여 사용자가 Build Binary File을 생성할 수 있게 도와준다.

* 여기서 Build Binary File는 IDE의 최종 결과물인 파일을 뜻한다.
* 작성자는 Visual Studio를 쓰고 해당 IDE의 최종 결과물인 .sin파일을 실행 시킬 수 있다.

생성된 .sin 파일을 실행 시켜 Build를 한번 해주면 Debug File과 동시에 Library File도 생성이 되는데  
생성된 라이브러리가 바로 GLFW API이다.

그리고 CMake를 통해 생성된 라이브러리를 가져다가 새로운 프로젝트에 넣어서 쓸 수 있지만,  
Tutorial에서는 친절하게 IDE에서 GLFW API 위치에서 자동으로 끌어다 쓸 수 있도록,   
include Directory와 Linker Directory를 설정시켜준다.

  


### 여기까지 GLFW API를 가져다가 OpenGL Context와 Window를 생성하게끔 할 수 있는 준비는 끝났다. 하지만

현재는 왠만한 그래픽 카드회사에서 OpenGL을 사용할 수 있도록 만들어져 있지만,   
사용자가 쓰고 있는 그래픽카드가 무엇인지 알 수 없기 때문에, OpenGL API를 사용할 때 어떤 설정인지  
알려줘야 한다.

그리고 이 작업 또한 굉장히 번거롭기 때문에, 외부 API를 사용한다.

이번에 설치해야할 API는 GLAD이고, 주로 OpenGL용 함수 포인터를 관리한다 까지만 알면 될듯 하다.

GLAD API를 다운 받으면 2개의 









