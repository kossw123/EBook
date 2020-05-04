---
description: tutiorial Resuable UI System
---

# tutiorial Resuable UI System

## 무엇을 하려고 하는가?

* 재사용이 가능한 UI System Project를 생성합니다.
* 프로그램에 있어서 사용자가 사용하기 쉽게 하기 위한 UI를 작성하고, 이를 재사용 가능하게끔 하여, 다른 Project에서도 적용 가능하도록 합니다.
* App, Game등에 활용됩니다.
* 해당 문서는 상업적으로 이용되지 않습니다.

{% embed url="https://www.youtube.com/watch?v=8L9osm0h5J4&list=PL5V9qxkY\_RnJAZUTVXewQrJWbb5B7IU8y&index=2&t=0s" %}

* 
## 작성법

* 2D Project를 생성하고 아래의 링크에 들어가서 필요한 Texture들을 다운받습니다.

{% embed url="https://gumroad.com/l/iXYzT" %}

* 첫번째로 기본적인 초기화면을 작성합니다.
  * GameView에서의 Resolution을 1080 \* 1920으로 변경합니다.
  * Empty Object를 생성하여 UI Controller Object를 생성합니다.
    * 해당 Project의 모든 Object는 UI Controller Object 아래에 생성합니다.
  * Canvas Object을 생성합니다.
    * Canvas Scaler - Scale With Screen Size로 변경하고 Resolution을 1080 \* 1920으로 변경합니다.
  * Image Object를 생성하여 BackGround Object를 생성합니다.
    * Anchor Preset을 상하좌우 Stretch로 설정하여 모든 Anchor값을 0으로 설정하여 Image를 펼칩니다.
    * todo\_login\_bg\_001를 대입하여 BackGround를 생성합니다.
  * Image Object를 생성하여 Icon Object를 생성합니다. 
    * todo\_logo\_001를 대입하여 Icon을 생성합니다.
    * Width, Height를 적당히 조절하여 크기를 설정합니다.
  * InputField를 하나 생성합니다.
    * Anchor를 middle Stretch로 설정하여 Height를 제외한 모든 Anchor를 0으로 만들고, Height를 적당히 조정합니다.
    * Image Component를 삭제합니다.
    * Placeholder와 Text Object의 적당히 조정합니다. 이에대한 크기는 아래쪽의 그림에 표시하겠습니다.
    * Image Object를 하위 Object로 생성하여, todo\_user\_icon\_001을 대입하여 User Icon을 최종적으로 생성합니다.

여기까지 과정은 아래의 그림과 같습니다.

![InputField&#xC758; Placeholder, Text&#xC758; &#xD06C;&#xAE30;&#xB97C; &#xC870;&#xC815;&#xD55C; &#xADF8;&#xB9BC;](../../.gitbook/assets/image%20%2816%29.png)



