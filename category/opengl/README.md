---
description: OpenGL
---

# OpenGL

Unity로 GPU instancing을 해서 배경화면을 만들어 볼까 생각했었다.

그래서 GPU instancing을 하는 Fractal을 만들어 PC 배경화면에 적용을 해봤다.  
Steam의 Wallpaper Engine 프로그램을 통해 손쉽게 적용을 할 수 있었고, 이제 Mobile쪽으로 넘어가서  
적용해 보고자 했다.

그러나, 여러 시도를 한 결과는 별로 만족스럽지 못했다.

시도 해본 것은 다음과 같다.

* Huwai Tablet에 Unity로 생성한 apk파일을 이식했으나, ITunes처럼 기기 관리 관리 프로그램이 필요한것과 막상 깔아도 모델이 너무 구형이라 이식할 수 없었다.
* IOS에 이식하려고 여러 문서를 뒤져봤지만, 개발자 계정이 필요하다.
* Android Studio와 Unity Engine을 사용하여 GPU instancing을 할 수 있으나, OpenGL을 사용해야 하는 듯 했다.
  * 막상 생각해보면, PC와 달리 Mobile 자체가 PC보다 떨어지는 성능을 보이는데 Unity는 게임엔진일뿐더러, 게임엔진을 가지고 만들 수는 있으나, Android Studio로 만드는 게 좀 더 최적화에 맞지 않을까 라는 생각이 들었다.



어찌됐든, 목적을 위해 Android Studio를 사용해보기로 했다.

첫 빌드를 통해 기본적인 사항을 알았으나, Hierarchy가 굉장히 많이 달랐고, 배경화면을 만들기 위해 jpg를 연속적으로 재생하여 렌더링 처럼 보이는 방법이 굉장히 흔했다.

내가 원하는 Unity GPU instancing, 즉 영상처리가 아닌 그래픽스를 하는 방법은 아닌 듯 했다.

그 후 OpenGL을 이용한 Android Live Wallpaper Tutorial을 발견 했으나, OpenGL이라는 API를 다루지 못해손을 땔 수 밖에 없었다.



그러나 Unity도 OpenGL / Vulkan을 사용하고 있고, 이를 이용하여 렌더링을 하기 때문에 한번 알아봐야할 필요는 분명 있다.

그렇기 때문에 해당 페이지를 작성했다.

