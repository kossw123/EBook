---
description: TileMap을 이용한 Map Design 제작
---

# TileMap

## 왜 해야 하는가?

제가 Unity로 게임을 작성하는데 있어서 큰 문제중 하나는 Map Design였습니다. 보통 Terrain이나 어떤 기초 도안을 가지고 자동적으로 만들어 주는 Terrain Tool도 존재합니다만, 특히 x,y축만 있는 2D게임에 대해 이런 Terrain을 쓰지 않기 때문에 좀더 효율적으로 작성 하기 위해 Unity에는 TileMap기능이 있습니다.

Tilemap의 기본적인 기능을 알아볼때 Scripting보다는 Editor상에서 필요한 작업량이 많기 때문에 Tutorial이 길게 작성할 수 밖에 없음을 사과드립니다.

{% embed url="https://www.youtube.com/watch?v=ryISV\_nH8qw" %}

아래의 링크를 타고 들어가시면 작성하려는 기능의 내용을 볼 수 있습니다.

{% embed url="https://blogs.unity3d.com/kr/2018/05/29/procedural-patterns-you-can-use-with-tilemaps-part-i/" %}



## 무엇을 하려고 하는가?

![Procedural patterns you can use with Tilemaps](../../.gitbook/assets/procedural-patterns.gif)

Tilemap 기능에 대해 소개를 하고 RuleTile 및 Random Tile과 Procedural patterns이라는 Unity Tutorial을  통해 Map Design을 해보려고 합니다.

* Tilemap을 이용한 간단한 Map Design 및 RuleTile 작성
* 절차적인 패턴\(Procedural patterns\)을 이용한 Map Design 작성

## Tilemap 작성법

Hierarchy창에서 Create, 혹은 우클릭은 눌러서 Tilemap Object를 생성합니다.

![Create -&amp;gt; 2D Object -&amp;gt; Tilemap](../../.gitbook/assets/hierarchy-tilemap.png)

Grid Object가 생성되고 자식 Object로 Tilemap Object가 생성되는 것을 확인할 수 있습니다.                                 Grid Object는 말그대로 격자를 표시하기 위한 Grid Component를 포함한 Object이고, Tilemap Object는 Grid Object에 들어갈 Tile을 표시하기 위한 Tilemap Renderer, Tilemap Component 포함되어 있습니다. 

이에 대한 자세한 기능들은 후에 Explanation 항목에 표시하겠습니다.

Tilemap을 넣으려면 Tile Palette라는 기능이 필요하며, 이것은 말 그대로 미술시간때 필요한 팔레트 처럼 Tile Sprite들을 정리하고 섞기 필요한 도구입니다.

![Window -&amp;gt; 2D -&amp;gt; Tile Palette](../../.gitbook/assets/tile-palette.png)

Tile Palette 기능을 표시했다면 여기에 필요한 Tile들을 넣어야 합니다. 이를 위해 PhysicsObject에서 사용했던 2D Platformer Game의 Sprite들을 활용하여 넣겠습니다. 

![&#xC0C8;&#xB85C; &#xC0DD;&#xC131;&#xD55C; Test Tile Palette](../../.gitbook/assets/image.png)

이렇게 새로 생성했다면 드래그 앤 드롭으로 생성할 Tile의 Sprite들을 넣어줍니다. 여기서는 기본적으로 Sprite들이 Tileset Size로 Slice되어 있지만 안되어 있는 Sprite들도 존재합니다. 이에 대한 주의점은 후에 Explanation 문서에 기술 하겠습니다.

![Test Palette&#xC5D0; &#xAE30;&#xC874;&#xC5D0; &#xC788;&#xB358; TileSet&#xB4E4;&#xC744; &#xB123;&#xC5B4;&#xC900; &#xACB0;&#xACFC;](../../.gitbook/assets/image%20%283%29.png)

위 그림과 같이 Tile들이 격자형태로 나눠진 것을 확인할 수 있으며 이것을 가지고 Tilemap Object에 넣어 주면 TileMap을 배치 할 수 있습니다.

![Tilemap&#xC744; &#xB123;&#xB294; &#xACFC;&#xC815;](../../.gitbook/assets/tilemap-insert.gif)

## Rule Tile 작성법

기존에 있던 Tilemap의 보조하는 기능으로써 Unity 내부에 자체적으로 존재하는것이 아니라 외부적으로      기능을 import해야합니다. 여기서는 아래의 Github에서 다운을 받아 Project에 직접 import 합니다.

{% embed url="https://github.com/Unity-Technologies/2d-extras" caption="Asset Store에 가도 Rule Tile기능들은 존재합니다." %}

자료를 다운받아 압축을 풀고 Project에 넣는다면 2d-extras-master라는 File이 Project View에 생성되면서 "Project View"에서 우클릭 후 Create로 가면 가장 위쪽에 Tile이라는 항목이 생성되고 여기서 Rule Tile들을 생성할 수 있습니다.

![Hierarchy&#xCC3D;&#xC5D0;&#xC11C; Create&#xD558;&#xBA74; &#xC0DD;&#xC131;&#xC774; &#xC548;&#xB418;&#xACE0; &quot;Project View&quot;&#xC5D0;&#xC11C; &#xC6B0;&#xD074;&#xB9AD;&#xC2DC; &#xC774;&#xB807;&#xAC8C; &#xB739;&#xB2C8;&#xB2E4;.](../../.gitbook/assets/image%20%284%29.png)

여기서 Rule Tile에 대해 알아보겠습니다. 

Rule Tile이란? Tile을 만들 때 어떤 규칙이 정해진 타일이라는 것인데, 여기서 규칙이라는 것은 어느 방향을 이야기 합니다. 어느 방향으로는 그리면 안될지 결정한다는 것입니다. 

어차피 Tilemap을 작성해 봤자 하나의 Sprite들을 가지고 여러개를 이어 붙여서 만든것인데, 굳이 필요한 이유를 말씀드리자면 아주 큰 Map Design을 작성할 시에 여러 Sprite를 이어서 만든 어떤 그림이 필요할 때가 종종 있습니다. 그를 대비해 알아둔다면 아주 유용하게 사용할 수 있을 것입니다.

![Rule Tile&#xC758; &#xAE30;&#xBCF8;&#xD654;&#xBA74;](../../.gitbook/assets/image%20%287%29.png)

Rule Tile을 생성했다면 









{% embed url="https://docs.unity3d.com/Manual/class-Tilemap.html" %}



