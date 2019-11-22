---
description: Unity Object의 Kinematic RigidBody를 이용한 Scripting
---

# PhysicsObject

## 왜 해야 하는가?

Unity는 Visual Scripting, 즉 UI를 이용하여 보다 편리하게 Game 작성을 위함인데,

왜? 굳이? Scripting이라는 작업을 통하여 보다 번거롭게 해야 하는가?

보통 RigidBody를 Dynamic으로 많이 쓰지만 Scripting으로 기능을 작성하여 Dynamic RigidBody에서 쓰지 않을 기능들을 추려낼수 있다는 것이 장점이 된다.

 해당 강의 영상은 아래의 링크를 타고 Unity Tutorial을 참고하여 나름대로 정리한 글입니다. 아래의 링크를 타고 들어가시면 볼 수 있고 자막도 있지만 군데군데 자막이 안달려 있는 영상도 있으니 유념해 주시고 봐주시기 바랍니다.

[ **Live Session: 2D Platformer Character Controller - URL**](https://learn.unity.com/tutorial/live-session-2d-platformer-character-controller#)



## **무엇을 하려고 하는가?**

![Scripting&#xC744; &#xD1B5;&#xD55C; &#xC6C0;&#xC9C1;&#xC784; &#xC81C;&#xC5B4;](../../.gitbook/assets/gif.gif)

 **Unity를 공부하면서 다른 프로젝트를 통해 움직임을 구현하는 것은 Unity 자체에 내장되어 있는 기능들을 통해 쉽게 구현이 가능하고 재미또한 있었다.** 

**하지만 움직임을 Scripting으로 제어한다는 것은 기본적으로 Unity에 있는 기능들을 기존보다 깊게             파고들어야 하기 때문에 어려움이 있었으나, 분명 스킬업이 되었다.**



## 작성

