---
description: tutorial Photon + FireBase Network Game
---

# tutorial Photon + FireBase Network Game

## 하려고 하는 이유는?

* User간에 Multiplay를 즐기기 위해서는 Server가 필수적으로 들어갑니다.
* 그리고 Console Game만 제작하여 원하는 컨셉의 Game을 제작하는 것이 굉장히 제한적이라는 것을 느꼈습니다.
* 그렇기 때문에 대중적으로 사용되는 Photon Engine, FireBase를 통한 Network 통신에 관하여 정리해보려고 합니다.

## 무엇을 하려고 하는가?

* 이 문서는 tutorial 보다는 정리 문서에 가까우며, Sample Project는 해당 강의 영상을 보시면서 하는게 좀 더 깔끔하고, 정리가 잘되어 있습니다.
* 외부 API를 사용하기 때문에 그에 대한 정리 내용을 문서로 정리한다면, 굉장히 긴 문서가 되기 때문에 주로 Photon과 FireBase, Task Class, Threading에 관련된 내용을 중점으로 정리 될것 같습니다.
* Photon + Firebase Network Game 강의 영상은 아래와 같습니다.

{% embed url="https://www.youtube.com/watch?v=-QsfDgvcheQ" %}

## 작성법

* 강의 영상을 보시고 작성하는 것이 Best 입니다.
* 그렇기 때문에 해당 문서에서는 Photon과 Firebase에 관한 설정 및 스크립트에 관하여 작성하겠습니다.

## Photon

* 해당 Project에서는 Photon은 Server의 역할을 합니다.
* Unity에서는 Photon을 쓰려면, Photon Unity Network 2\(PUN2\)를 사용해야 합니다.
* Asset Store에서 PUN2로 검색하시면 Free Version으로 받으실 수 있습니다.

![PUN2 asset Import&#xD558;&#xBA74; &#xB9E8; &#xCC98;&#xC74C; &#xB098;&#xC624;&#xB294; Wizard Window](../../.gitbook/assets/image%20%2837%29.png)

* 일단은 Skip하고 Build Setting에서 Platform을 Android로 교체합니다.
* 후에 Player Settings으로 들어가서 Publising Settings -&gt; KeyStore Manager -&gt; 



