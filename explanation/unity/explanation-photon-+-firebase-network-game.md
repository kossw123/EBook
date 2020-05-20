---
description: Explanation Photon + FireBase Network Game
---

# Explanation Photon + FireBase Network Game

## 무엇을 하려고 하는가?

* Photon, Firebase에 대한 개요와, Sample Project에서 사용한 함수들에 대한 고찰을 합니다.



## Photon\(PUN2\)

{% tabs %}
{% tab title="개요" %}
* PUN2는 멀티플레이를 위한 Unity Package입니다.
* 우리가 알고 있는 Client - Server Model을 가지고 있고, PUN2는 Room이라는 공간에 네트워크를 따라 들어온 Client들을 동기화 시켜줍니다.
* Photon은 Cloud 형식으로 제공됩니다.
  * 게임에서 사용할 수 있는 Server의 집합입니다.
  * 아래와 같은 방법으로 접근합니다.
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

{% tab title="PUN2 " %}

{% endtab %}
{% endtabs %}


