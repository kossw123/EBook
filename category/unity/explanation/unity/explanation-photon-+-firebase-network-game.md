---
description: Explanation Photon + FireBase Network Game
---

# Explanation Photon + FireBase Network Game

## 무엇을 하려고 하는가?

* Photon, Firebase에 대한 개요와, 기능을 구성하는 Logic등을 살펴봅니다.

## Client - Server Model

![Client - Server Model](<../../../../.gitbook/assets/image (161).png>)

* 다른 사람에게 자신의 행동에 대한 정보(자신의 위치, 아이템, 상태 등)을 보낼 때 쓰이는 Model입니다.
* 보통 Server를 사용하는 모든 프로그램들은 위의 형태와 같습니다.
* Client에서 직접적으로 다른 Client에 보내면 막대한 계산의 문제가 발생하기 때문에 이와 같은 Network Architecture가 탄생했습니다.

## Photon(PUN2)

{% tabs %}
{% tab title="Photon, PUN2 Package란?" %}
* PUN2는 멀티플레이를 위한 Unity Package입니다.
* 우리가 알고 있는 Client - Server Model을 가지고 있고, PUN2는 Room이라는 공간에 네트워크를 따라 들어온 Client들을 동기화 시켜줍니다.
* Photon은 Cloud 형식으로 제공됩니다.
  * 게임에서 사용할 수 있는 Server의 집합입니다.
  * 사용자가 Server에 접속하려면 아래와 같은 방법으로 접근합니다.
    *
      1. Name Server에 접근합니다.
         1. Name Server를 통해 AppID를 확인하고 어떤 지역으로 접근할지 확인합니다.
         2. 확인이 끝나면 Master Server로 forwarding 합니다.
      2. 각 지역의 Photon Server인 Master Server로 접속합니다.
         1. Master Server는 해당 지역내의 모든 Server에 대해 알고있습니다.
         2. Photon.Realtime namespace의 Class에 접근하여 Room에 관련된 함수를 가져 올 수 있습니다.
         3. Master Server는 Lobby, Room을 생성하는 부분을 담당합니다.
      3. Game Server에서 동기화를 하여 멀티플레이를 할 수 있도록 합니다.
         1. Game Server는 Room에 입장한 순간부터 Server를 사용하여 플레이어간의 동기화를 합니다.

![Photon Cloud가 지역으로 접근하는 방법](<../../../../.gitbook/assets/image (155).png>)
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

![PhotonServerSetting.asset의 Inspector](<../../../../.gitbook/assets/image (153).png>)

* d
{% endtab %}

{% tab title="Room" %}
* Room은 GameServer에서 생성되는 Client이 Server에 접속하는 순간 존재하는 공간입니다.
* Free Trial은 해당 공간안에 최대 10명의 Client가 존재할 수 있습니다.
* Room은 식별할 수 있는 이름을 가지고 있습니다.
* 가득차 있거나, 닫혀있지 않는 이상 이름을 가지고 Room에 참여할 수 있습니다.
* Master Server에서는 App으로 Room의 목록을 제공합니다.
{% endtab %}

{% tab title="동기화 하는 방법" %}
* PUN2 Unity Package에서는 플레이어간의 동기화를 Photon View.cs라는 Script Component를 사용하여 동기화를 감지합니다.
  * 본질적으로 Unity는 Component pattern을 가지고 있기 때문에, 이를 사용하려면 Component로 직접 Inspector에 추가해야 하는 부분입니다.
* 내부적으로 감지하기 때문에, 이에 대해서 심층적인 정보를 원하신다면 아래의 링크를 보시면 됩니다.

{% embed url="https://doc-api.photonengine.com/ko-kr/pun/current/class_photon_view.html" %}

* Logic이 어찌 됐든 Photon을 사용하여 플레이어 간의 동기화를 위해서는 PhotonView, PhotonTransformView 등과 같 Script Component를 사용하여 동기화를 합니다.
{% endtab %}

{% tab title="PhotonView" %}
* Photon에서는 네트워크에서 동작하는 GameObject를 쉽게 생성할 수 있습니다.
  * 생성된 GameObject에 PhotonView Component를 추가하면 네트워크에서 동작합니다.

![Player prefab의 Inspector](<../../../../.gitbook/assets/image (167).png>)

* 위의 그림을 보면 PhotonView, Photon Transform View Component를 추가하여 동기화를 하고 있습니다.
* PhotonView Component는 네트워크 상의 Object와 정보를 동기화 하기 위해 사용된 것입니다.
* PhotonStream Class를 통해 직렬화된 정보를 주고 받은 다음 observe option 열거문값에 따라 실행합니다.
  * Owner : PhotonView Component가 추가된 Object의 주인을 의미합니다.
    * Owner 항목을 통해 지정한다면 어떤 이유로 인해 프로그램이 Delay되는 경우 소유자에게 알려줄 필요가 있기 때문에 존재합니다.
  * View ID : ID가 같다면 같은 Object로 취급하고 서로 동기화합니다.
  * Observe option : 값을 관측하고 동기화 하는 방식을 변경합니다.
    * Off : null을 반환하기 떄문에, 동기화 하지 않습니다.
    * Reliable Delta Compressed : DeltaCompressionWrite() 함수를 통해 이전 데이터와 새로운 데이터를 비교하여, 같다면 이전 데이터값들을 null로 만듭니다.&#x20;
    * Unreliable : 어떠한 검사도 하지 않고 바로 최근 데이터를 반환합니다.&#x20;
    * Unreliable On Change : 마지막으로 보낸 데이터(view.lastOnSerializeDataSent)가 최근 데이터(currentValues)와 같다면 교체합니다. 즉, 데이터를 보내지만 같다면 유지하고, 다르다면 송신합니다.
* Photon Transform Vierw Component는 네트워크 상의 Object와 위치정보를 동기화 하기 위해 사용된 것입니다.
  * 해당 Component는 PhotonView Component에 의해 감지 됩니다.
{% endtab %}
{% endtabs %}

## Firebase

{% tabs %}
{% tab title="Firebase란?" %}
* 보통 Server를 사용하기 위해 이전 내용을 불러오기 위해 저장소, 인증, DB tool과 같은 것들이 필요합니다.
* Firebase는 이런 저장소를 추가해주고 인증, DB, 저장소 등과 같은 기능들을 한번에 제공해 줍니다.

![Firebase의 도입으로 인한 작업량 감소 - realm academy](<../../../../.gitbook/assets/image (164).png>)

* Firebase를 사용하면 위의 그림과 같이 Server 개발에 필요한 작업량의 감소를 할 수 있습니다.
* Unity와 같이 하나의 Project에서 여러 기능들을 사용하여 최종적으로 통합하여 결과를 출력해주는 프로그램입니다.
* 그러한 Firebase의 Project는 아래와 같이 정의할 수 있습니다.
  * Firebase의 Project = 추가적인 관련 구성 및 서비스 + GCP(Google Cloud Platform)

{% embed url="https://firebase.google.com/docs/projects/learn-more?authuser=0" %}
{% endtab %}

{% tab title="Firebase 사용하는 방법" %}
* 기본적으로 Firebase에서 제공하는 SDK를 해당 Project에 다운을 받아야 합니다.
* 다운을 받으면 자동적으로 연동이 되는데, 이를 사용하기 위해서는 Script부분의 도움이 필요합니다.
* 아래의 링크는 Firebase 관련 API Document입니다.

{% embed url="https://firebase.google.com/docs/reference/unity?hl=ko" %}

* 이 Document를 사용하여 실제적으로 Firebase의 기능과 Unity 간의 연동이 필요합니다.
* 여기서 쓰는 FirebaseApp, FirebaseAuth, FirebaseUser는 아래의 namespace에 존재합니다.
  * using Firebase;&#x20;
  * using Firebase.Auth;&#x20;
  * using Firebase.Extensions;
{% endtab %}

{% tab title="Authentication" %}
* Sample Project에서는 Firebase의 Authentication(인증)기능을 사용하여, 아래의 기능들을 활성화 합니다.
  * Login 하기 위한 ID, Password 등록합니다.
  * Login 제공업체 선택을 하여 Login 방식 선택(PlayStore, Facebook, google 등)합니다.
* 만약 Authentication(인증)이 아닌 Game 내부적인 내용을 저장하고 싶다면, 따로 json type의 저장파일을 사용하여, 직렬화/역직렬화를 통해 저장, 불러오기 기능을 작성할 수 있습니다.
{% endtab %}
{% endtabs %}

## 마치며

* 해당 문서에서는 개요 및 설명을 통해 Firebase, Photon에 관련한 설명을 했다면 기술문서(Technical reference)에서는 관련된 함수를 통해 좀 더 Code에 대한 이해를 더하는 문서가 될 것 같습니다.
* 그렇기 때문에 여기 까지 읽으셨다면 기술문서(Technical reference)를 읽으시는 것을 권장합니다.

{% content-ref url="../../technical-reference/unity/tr-photon-+-firebase-network-game.md" %}
[tr-photon-+-firebase-network-game.md](../../technical-reference/unity/tr-photon-+-firebase-network-game.md)
{% endcontent-ref %}
