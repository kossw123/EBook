---
description: Unity Object의 Kinematic RigidBody를 이용한 Scripting
---

# PhysicsObject

## 왜 해야 하는가?

Unity는 Visual Scripting, 즉 UI를 이용하여 보다 편리하게 Game 작성을 위함인데,

왜? 굳이? Scripting이라는 작업을 통하여 보다 번거롭게 해야 하는가?

보통 RigidBody를 Dynamic으로 많이 쓰지만 Scripting으로 기능을 작성하여 혹여나                                    Dynamic RigidBody에서 쓰지 않을 기능들을 추려낼수 있다는 것으로, 성능 향상에 약간이나마 도움이        될 수도 있다는 생각에 이 글을 작성하게 되었습니다.

그리고 이 예제를 통해 2D Game의 작성 방법과 그에 관련한 움직임 및 물리작용을 공부한다는 목적이       있습니다.

작성된 글은 아래의 링크를 타고 Unity Tutorial을 참고하여 나름대로 정리한 글입니다.                                         아래의 링크를 타고 들어가시면 볼 수 있고 자막도 있지만 군데군데 자막이 안달려 있는 영상도 있으니       유념해 주시고 봐주시기 바랍니다.

[ **Live Session: 2D Platformer Character Controller - URL**](https://learn.unity.com/tutorial/live-session-2d-platformer-character-controller#)

\*\*\*\*



## **무엇을 하려고 하는가?**

![Scripting&#xC744; &#xD1B5;&#xD55C; &#xC6C0;&#xC9C1;&#xC784; &#xC81C;&#xC5B4;](../../.gitbook/assets/gif.gif)

 **Unity를 공부하면서 다른 프로젝트를 통해 움직임을 구현하는 것은 Unity 자체에 내장되어 있는 기능들을 통해 쉽게 구현이 가능하고 재미 또한 있었습니다.**

**하지만 움직임을 Scripting으로 제어한다는 것은 기본적으로 Unity에 있는 기능들을 기존보다 깊게             파고들어야 하기 때문에 어려움이 있었으나, 분명 스킬업이 되었습니다.**

1. 2D platformer game을 위한 맞춤형 스크립트 기반 물리학을 만드는 법을 배웁니다.
2. 수평 이동, 중력 및 취소 가능한 점프를 스크립트를 통해 배웁니다.
3. 충돌을 수동으로 감지하고 경사면과 각진 표면을 처리하는 방법을 배웁니다.



## 작성법



1. 


