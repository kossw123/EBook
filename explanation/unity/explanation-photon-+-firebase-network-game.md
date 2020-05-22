---
description: Explanation Photon + FireBase Network Game
---

# Explanation Photon + FireBase Network Game

## 무엇을 하려고 하는가?

* Photon, Firebase에 대한 개요와, Sample Project에서 사용한 함수들에 대한 고찰을 합니다.



## Client - Server Model

![Client - Server Model](../../.gitbook/assets/image%20%28160%29.png)

* 다른 사람에게 자신의 행동에 대한 정보\(자신의 위치, 아이템, 상태 등\)을 보낼 때 쓰이는 Model입니다.
* 보통 Server를 사용하는 모든 프로그램들은 위의 형태와 같습니다.
* Client에서 직접적으로 다른 Client에 보내면 막대한 계산의 문제가 발생하기 때문에 이와 같은 Network Architecture가 탄생했습니다.

## Photon\(PUN2\)

{% tabs %}
{% tab title="개요" %}
* PUN2는 멀티플레이를 위한 Unity Package입니다.
* 우리가 알고 있는 Client - Server Model을 가지고 있고, PUN2는 Room이라는 공간에 네트워크를 따라 들어온 Client들을 동기화 시켜줍니다.
* Photon은 Cloud 형식으로 제공됩니다.
  * 게임에서 사용할 수 있는 Server의 집합입니다.
  * 사용자가 Server에 접속하려면 아래와 같은 방법으로 접근합니다.
    * 1. Name Server에 접근합니다.
         1. Name Server를 통해 AppID를 확인하고 어떤 지역으로 접근할지 확인합니다.
         2. 확인이 끝나면 Master Server로 forwarding 합니다.
      2. 각 지역의 Photon Server인 Master Server로 접속합니다.
         1. Master Server는 해당 지역내의 모든 Server에 대해 알고있습니다.
         2. Photon.Realtime namespace의 Class에 접근하여 Room에 관련된 함수를 가져 올 수 있습니다.
      3. Game Server에서 동기화를 하여 멀티플레이를 할 수 있도록 합니다.

![Photon Cloud&#xAC00; &#xC9C0;&#xC5ED;&#xC73C;&#xB85C; &#xC811;&#xADFC;&#xD558;&#xB294; &#xBC29;&#xBC95;](../../.gitbook/assets/image%20%28155%29.png)
{% endtab %}

{% tab title="구성 요소" %}
* PUN2 Package는 아래와 같이 구성되어 있습니다.
  * 최상위
    * 자체적인 PUN Code로 이루어진 네트워크 객체, RPC같은 Unity 기능이 있습니다.
  * 중간
    * Photon Server, Match Making, Callback 함수들에 관련된 Logic이 존재합니다.
    * 실시간 API입니다.
  * 최하위
    * DLL 형식의 파일을 가진 **Object 전송, 저장 가능한 형태로 바꾸는 직렬화/비직렬화**, 프로토콜 등에 대한 정보가 담겨 있습니다.
{% endtab %}

{% tab title="PUN Import, Setting" %}
* Package를 Project에 Import하면 맨 처음 띄워지는 PUN Wizard는 Photon 홈페이지에서 생성한 AppId 혹은 Email을 가지고 연동을 시킵니다. 이로 인해 Photon의 Free Trial의 Server에 대한 사용권한을 받을 수 있습니다.
* 그 후 Project 내부의 PhotonServerSetting를 통해 허가 받은 Server에 대한 설정을 할 수 있습니다.

![PhotonServerSetting.asset&#xC758; Inspector](../../.gitbook/assets/image%20%28153%29.png)

* d
{% endtab %}

{% tab title="Room" %}
* Room은 GameServer에서 생성되는 Client이 Server에 접속하는 순간 존재하는 공간입니다.
* Free Trial은 해당 공간안에 최대 10명의 Client가 존재할 수 있습니다.
* Room은 식별할 수 있는 이름을 가지고 있습니다.
* 가득차 있거나, 닫혀있지 않는 이상 이름을 가지고 Room에 참여할 수 있습니다.
* Master Server에서는 App으로 Room의 목록을 제공합니다.
{% endtab %}
{% endtabs %}



## Firebase

{% tabs %}
{% tab title="개요" %}
* 보통 Server를 사용하기 위해 이전 내용을 불러오기 위해 저장소, 인증, DB tool과 같은 것들이 필요합니다.
* Firebase는 이런 저장소를 추가해주고 인증, DB, 저장소 등과 같은 기능들을 한번에 제공해 줍니다.

![Firebase&#xC758; &#xB3C4;&#xC785;&#xC73C;&#xB85C; &#xC778;&#xD55C; &#xC791;&#xC5C5;&#xB7C9; &#xAC10;&#xC18C; - realm academy](../../.gitbook/assets/image%20%28161%29.png)

* Firebase를 사용하면 위의 그림과 같이 Server 개발에 필요한 작업량의 감소를 할 수 있습니다.
{% endtab %}

{% tab title="Firebase란?" %}
* Unity와 같이 하나의 Project에서 여러 기능들을 사용하여 최종적으로 통합하여 결과를 출력해주는 프로그램입니다.
* 그러한 Firebase의 Project는 아래와 같이 정의할 수 있습니다.
  * Firebase의 Project = 추가적인 관련 구성 및 서비스 + GCP\(Google Cloud Platform\)
* 
{% embed url="https://firebase.google.com/docs/projects/learn-more?authuser=0" %}
{% endtab %}

{% tab title="Authentication" %}
* Sample Project에서는 Firebase의 Authentication\(인증\)기능을 사용하여, 아래의 기능들을 활성화 합니다.
  * Login 하기 위한 ID, Password 등록합니다.
  * Login 제공업체 선택을 하여 Login 방식 선택\(PlayStore, Facebook, google 등\)합니다.
* 만약 Authentication\(인증\)이 아닌 Game 내부적인 내용을 저장하고 싶다면, 따로 json type의 저장파일을 사용하여, 직렬화/역직렬화를 통해 저장, 불러오기 기능을 작성할 수 있습니다.
{% endtab %}
{% endtabs %}

## 마치며

* 해당 문서에서는 개요 및 설명을 통해 Firebase, Photon에 관련한 설명을 했다면 기술문서\(Technical reference\)에서는 관련된 함수를 통해 좀 더 Code에 대한 이해를 더하는 문서가 될 것 같습니다.
* 그렇기 때문에 여기 까지 읽으셨다면 기술문서\(Technical reference\)를 읽으시는 것을 권장합니다.

{% page-ref page="../../technical-reference/unity/tr-photon-+-firebase-network-game.md" %}

