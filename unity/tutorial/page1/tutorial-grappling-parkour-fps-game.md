---
description: tutorial Grappling Parkour FPS Game
---

# tutorial Grappling Parkour FPS Game

## 무엇을 하려고 하는가?

사이드 프로젝트를 진행하면서, 현재 진행한 프로젝트에 관한 내용을 정리하려고 합니다.

* 다음과 같은 기능들이 해당 프로젝트 내부에 존재합니다.
  * First Person View의 Rigidbody를 이용한 Movement
  * Physics.Overlap Component를 이용한 충돌 지점 판별 여부
  * Spring Joint을 이용한 Rigidbody의 연결, Line Renderer를 이용한 Spring Joint의 표시
  * Probuilder를 통한 맵제작
  * Post Processing을 이용한 Global Volume 처리
  * UnityEvent를 이용한 상태변수 Callback System 기초 구현

{% hint style="warning" %}
Tutorial을 보시기 전 주의점

* 해당 문서를 작성하는 이유는 위의 기능의 추가 중 Callback System을 도입하고나서, 즉흥적으로 기능을 추가하는 것에 대해 한계를 느껴 해당 내용을 정리하고자 작성됨을 알립니다.
* **즉, 설계가 즉흥적으로 추가한 것이다 보니, 해당 프로젝트를 가지고 첨삭하는 것을 추천하지 않습니다.**
{% endhint %}

## 작성하기 전 받은 Package

해당 프로젝트를 진행하기 전 다음과 같은 Package를 다운 받으시는 것을 추천드립니다.

* Window -&gt; Package Manager -&gt; 상단의 "+" UI 옆 Box에서 All Packages를 선택하고 다음과 같은 Package를 다운받습니다.
  * Post Processing
  * Probuilder

## 작성법

### Player - Object

Capsule Object를 추가하여 해당 Object의 이동과 카메라 방향으로의 이동을 구현합니다.

![](../../../.gitbook/assets/image%20%28219%29.png)

해당 그림은 Capsule Object를 추가하여 하위 Object로 Cube Object를 생성하여 이름을 Visor로 놓고 다시 Visor Object 아래에 Main Camera를 둔 형태입니다.

### Player - Script

해당 사이드 프로젝트에서는 하나의 추상 클래스를 상속받는 PlayerMovement Script가 존재합니다.



