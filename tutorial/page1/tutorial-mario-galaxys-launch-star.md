---
description: tutorial Mario Galaxy's Launch Star
---

# tutorial Mario Galaxy's Launch Star\(작업중\)

## 왜 하려고 하는가?

* 3D Project를 통해 2D game에서의 Sprite의 한계를 넘어서 3D Modeling를 경험함과 동시에 아무래도 3D에서는 2D와는 또 다른 환경에서 제작을 하기 때문에 여러 장르의 개발을 연습과정을 문서화로 남기게 되었습니다.
* 아래의 강의 영상를 토대로 작성한 문서입니다.
* 해당 문서 및 문서가 사용되는 다른문서들은 오직 학습목적으로만 이용됩니다.

## 무엇을 하려고 하는가?

* Cinemachine을 이용한 역동적인 카메라 구현
* 3D Modeling을 이용하여 Character import 및 Mixamo를 이용한 Animation import
* Scripting을 이용하여 경로 구현
* DoTween API를 사용하여 간편제작 구현

## 작성법

{% embed url="https://www.youtube.com/watch?v=T\_3cne2tzYM&t=75s" caption="Mario Galaxy\'s Launch Star tutorial Video" %}

해당 강의 영상을 참고하여 아래와 같은 요소를 중점적으로 정리 및 문서화를 하겠습니다.

* Path Setup\(경로 설치\)
* Animations\(애니메이션\)
* Polish\(광택 : 제작에 질을 높이는 방법\)

처음으로 해당 영상의 Character Modeling은 아래의 링크에서 받으실 수 있습니다.

{% embed url="https://github.com/mixandjam/Jammo-Character" caption="Jammo-Character Github Link" %}

Mixamo를 이용한 3D Modeling의 import 과정은 tutorial이 아닌 다른 문서에서 다루도록 하겠습니다.

![Character &#xBC0F; &#xC0AC;&#xC804;&#xC900;&#xBE44;](../../.gitbook/assets/image%20%2838%29.png)

위의 Github Link에서 다운 받은 Character를 Plane Object 위에 배치하고 몇가지 수정을 합니다.

* Plane Object에 Rigidbody Component를 추가 후 Use Gravity = false로 설
* Plane Object에서 Mesh Collider Component의 Convex = ture로 설정
* Plane Object에서 Mesh Renderer의 Materials의 Element 0 = m\_ground라는 Materials로 설
  * Character와 Plane사이의 충돌을 위한 Component들은 따로 추가하지 않습니다.
    * Character Controller Component가 충돌을 위한 Collider, Rigidbody Component를 대신해     사용할 수 있습니다.

위의 사전준비한 내용을 Play하여 충돌 및 입력을 확인 후 Path SetUp을 위한 Cinemachine Package를 설치합니다.

Window -&gt; Package Manager -&gt; Cinemachine을 설치니다.

![Window -&amp;gt; Package Manager -&amp;gt; Cinemachine Install](../../.gitbook/assets/image%20%2837%29.png)

설치 후 Editor의 상단에는 Cinemachine이라는 탭이 하나 생깁니다. 클릭하여 Create Dolly Track with Cart항목을 눌러 새로운 Dolly Cart라는 Object를 생성합니다. 

Hierarchy에서 DollyTrack1, DollyCart1 Object가 생성되고 아래의 그림과 같은 Scene View가 생성됩니다.

![Create Dolly Track with Cart](../../.gitbook/assets/image%20%2815%29.png)

Import한 Jammo\_Player를 Unpack을 하고 Cinemachine 탭에서 FreeLook Camera를 추가하여 생성된 Object의 Inspector에서 Follow, LookAt Component에 Character를 넣고 Orbits를 아래의 그림과 같이 수정합니다.

![](../../.gitbook/assets/image%20%2848%29.png)







