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
  * UnityEvent를 이용한 상태변수 Callback System 기초 구현

{% hint style="warning" %}
Tutorial을 보시기 전 주의점

* 해당 문서를 작성하는 이유는 위의 기능의 추가 중 Callback System을 도입하고나서, 즉흥적으로 기능을 추가하는 것에 대해 한계를 느꼈기 때문에 작성됨을 알립니다.
* 즉, 설계가 즉흥적으로 추가한 것이다 보니, 해당 프로젝트를 가지고 첨삭하는 것을 추천하지 않습니다.
{% endhint %}



