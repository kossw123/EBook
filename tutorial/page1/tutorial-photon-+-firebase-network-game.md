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
* **주로 Network Game Logic을 작성하기 전에, 필요한 DB, Server에 대한 설정법이 주 내용이 될것 같습니다.**
* Photon + Firebase Network Game 강의 영상은 아래와 같습니다.

{% embed url="https://www.youtube.com/watch?v=-QsfDgvcheQ" %}

## 작성법

* 강의 영상을 보시고 작성하는 것이 Best 입니다.
* 그렇기 때문에 해당 문서에서는 Photon과 Firebase에 관한 설정 및 스크립트에 관하여 작성하겠습니다.

## Photon

* 해당 Project에서는 Photon은 Server의 역할을 합니다.
* Unity에서는 Photon을 쓰려면, Photon Unity Network 2\(PUN2\)를 사용해야 합니다.
* Asset Store에서 PUN2로 검색하시면 Free Version으로 받으실 수 있습니다.

![PUN2 asset Import&#xD558;&#xBA74; &#xB9E8; &#xCC98;&#xC74C; &#xB098;&#xC624;&#xB294; Wizard Window](../../.gitbook/assets/image%20%2844%29.png)

* 일단은 Skip하고 Build Setting에서 Platform을 Android로 교체합니다.
* 후에 Player Settings으로 들어가서 Publising Settings -&gt; KeyStore Manager -&gt; Create new를 통해 어떤 파일에서든 생성을 합니다.
* 아래의 항목들에 대한 정보를 입력합니다.

![Create to KeyStore ](../../.gitbook/assets/image%20%2821%29.png)

* Password : 비밀번호
* comfirm Password : 비밀번호
* Alias : 이름

다 입력을 하셨다면 Add Key를 눌러서 키를 추가합니다.

## FireBase

* 이제 Photon을 이용하여 Server에 대한 기초 설정은 완료 되었고, Server를 활용할 ID, Password등과 같이 MultiPlay를 위해 필요한 부분을 저장하는 DB를 작성합니다.

{% embed url="https://firebase.google.com/?hl=ko" %}

위의 링크로 들어가서, Firebase에 대해 시작하기를 눌러서 Project를 생성하겠습니다.

![Start to Firebase](../../.gitbook/assets/image%20%284%29.png)

* Project를 시작했다면 아래의 그림과 같은 Project에 대한 정보를 볼 수있습니다. 
* 그리고 프로젝트 추가를 통해 다음 과정을 진행합니다.

![Sample Project&#xB97C; &#xC704;&#xD574; &#xBBF8;&#xB9AC; &#xC0DD;&#xC131;&#xB418;&#xC5C8;&#xAE30; &#xB54C;&#xBB38;&#xC5D0; &#xC606;&#xC758; UNITY SERVER TEST Project&#xB294; &#xBB34;&#xC2DC;&#xD569;&#xB2C8;&#xB2E4;.](../../.gitbook/assets/image%20%28111%29.png)

* 프로젝트 이름을 정하고, 구글 Analystic에 대한 설정을 합니다.
* 이에 대한 Analytics 설정은 안하셔도 상관없습니다.
* 그 다음 과정을 진행합니다.

![](../../.gitbook/assets/image%20%28132%29.png)

* 그 다음에 계정 선택을 하여, 계정을 생성합니다. Analytics에 대한 지역을 진행하고, 이에 대한 설정을 완료하면 Project를 생성할 수 있습니다.

![](../../.gitbook/assets/image%20%28129%29.png)

* FireBase를 처음 시작하면 다음 그림과 같은 초기화면이 실행됩니다.

![](../../.gitbook/assets/image%20%288%29.png)

* 이때 Authentication\(인증\) 이라는 부분에 들어가서 우리가 생각하는 ID\(혹은 Game을 실행하기 위한 Email\), Password를 등록합니다.

![](../../.gitbook/assets/image%20%28145%29.png)

* 등록을 해야하는데 로그인 방법을 설정해야 하는데, 이에 대한 설정은 우리가 생각하는 Google Play, iOS의 Game Center등과 같은 로그인 방법을 설정하는 부분입니다.

![](../../.gitbook/assets/image%20%2829%29.png)

* Sample Project에서는 이메일 / 비밀번호 인증방법을 통해 ID, Password를 설정합니다

![](../../.gitbook/assets/image%20%2870%29.png)

* 이에 대한 설정을 한 다음 아래의 그림과 같이 사용자 추가를 할 수 있습니다.

![](../../.gitbook/assets/image%20%2810%29.png)

* 사용자에 대한 Email과 Password를 등록한다면 아래의 그림과 같이 사용자가 추가됩니다.

![](../../.gitbook/assets/image%20%2838%29.png)

* 그 다음 Server와 DB를 연동하기 위한 인증서가 필요합니다.
* 이를 위해 프로젝트 개요창에서 -&gt; 앱 추가 -&gt; Unity를 선택하여 플랫폼을 선택합니다.

![](../../.gitbook/assets/image%20%2827%29.png)

* Sample Project를 Android를 겨냥한 Project이기 때문에, Android에 대한 패키지 이름과 앱 닉네임을 입력합니다.
* google-service.json 이라는 파일을 다운 받으라고 합니다.
  * 이 폴더는 인증서에 대한 정보가 담겨 있는 파일입니다.
  * 이 파일은 Unity Sample File의 임의의 위에 넣습니다.
* Firebase Unity SDK를 다운 받습니다.
  * Sample Project에서는 dotnet4 -&gt; FirebaseAuth Package를 사용합니다.
  * 이 파일을 Sample Project에 Import를 합니다.

![Firebase Unity SDK File](../../.gitbook/assets/image%20%28102%29.png)

![FirebaseAuth](../../.gitbook/assets/image%20%28151%29.png)



Firebase를 다운 받고, goolge-service.json을 통해 인증서를 받았다면 Firebase에 등록을 해야합니다.

명령 프롬프트 창\(cmd\)을 통해 인증서에 대한 SHA1을 뽑아야 하는데 이에 대한 방법은 아래와 같습니다.

> 해당  Unity Project 경로의 Keystore 저장소의 Path로 이동
>
> > keytool -list -v -alias pong -keystore 추가한 key의 이름
> >
> > > 비밀번호를 입력
> > >
> > > > 인증서 지문이 출력되는데 SHA1 전체를 복사합니다.

인증서를 복사했다면, 다음과 같은 과정으로 인증서를 추가합니다.

* 프로젝트 개요 -&gt; 톱니바퀴 -&gt; 프로젝트 설정 -&gt; 내앱 -&gt; 디지털 지문 추가 -&gt; 복사한 SHA1 인증번호 입력

## 마치며, 해당 문서에 대한 부족

* Photon, Firebase에 대한 설정법이 위주가 된 문서였기에, 부족하다고 느끼실 수 있고, 영상과 약간 다른 절차로 했기 때문에, 혼동이 오실 수 있습니다.
* 이때는 강의영상을 보시는게 더 정확합니다.
* 다음 How-to-guide에서는 Sample Project에서 쓰인 AuthManager와 Photon과 통신하는 Script에 대해 기재하고 해당 부분에 대한 설명을 기재하겠습니다.
* Photon, FireBase에 대한 개요 및 설명 문서는 해설문서\(Explanation\)에 기재하도록 하겠습니다.

{% page-ref page="../../how-to-guide/unity/how-to-guide-photon-+-firebase-network-game.md" %}





