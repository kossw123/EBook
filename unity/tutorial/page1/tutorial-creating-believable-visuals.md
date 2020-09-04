---
description: 'tutorial 유니티로 시네마틱 제작하기 : 타임라인 & 시네머신'
---

# tutorial 유니티로 시네마틱 제작하기 : 타임라인 & 시네머신

## 무엇을 하려고 하는가?

* 믿을만한 Visual 만드는 Skill에 대해서 알아봅니다.
* Game은 Visual과 프로그래밍의 합작이라고 생각 했기 때문에, 프로그래밍 뿐만이 아니라 Visual적인 Document 또한 필요하다고 느꼈기 때문에, 해당 문서를 작성합니다.

{% embed url="https://www.youtube.com/watch?v=6uWa3JnL2hk" %}

* 위의 문서를 기초로 OZTV님의 제작하신 Cinemachine + TimeLine을 기반으로 한 Shortcut Animation영상 제작 tutorial을 통해 R&D합니다.
* 해당 문서는 상업적인 용도로 사용되지 않고 사용해서는 안됩니다.

## 개

{% embed url="https://drive.google.com/file/d/1H4mX9yAwuhBzSbh2bRxM7jIuuUQsDoWu/view" caption="유니티로 시네마틱 제작하기 : 타임라인 & 시네머신 Sample File" %}

**Creating Believable Visuals와 Cinemachine을 작성하기 위한 HRDP package의 라이브러리가 오류가 생기기 때문에, OZTV님의 샘플 프로젝트를 사용하는점 양해 부탁드립니다.**

해당 문서의 중점은 아래와 같습니다.

* Timeline을 통해 Object의 Animation을 시간의 흐름에 따라 이동, 회전 및 활성화 여부를 할당할 수 있습니다.
* Cinemachine을 통해 생성한 Virtual Camera들로 영화같은 Scene을 표현할 수 있습니다.
* Unity는 작성한 Scene을 RecorderClip을 통해 영상파일로 추출할 수 있습니다.

지금은 Asset Store에 없는 Adam Character Pack을 쓰고 있습니다. 그렇기 때문에, 실습할 때는 꼭 샘플 프로젝트를 다운받아서 사용하시길 바랍니다.

![](../../../.gitbook/assets/image%20%28243%29.png)

이를 통해 Timeline을 작성합니다. Project View에서 간단한 Timeline Asset을 생성하여 Hierarchy창에 추가한다면, Timeline 이름과 같은 Object가 생성되고 Playable Director Component가 추가됩니다.

![](../../../.gitbook/assets/image%20%28244%29.png)

Playable Director를 통해 **Timeline instance\(Scene에서 사용할 수 있는 Timeline Object\)**와 **Timeline Asset\(Project View에서 Asset형식으로 생성한 데이터\)** 사이의 링크를 저장합니다. Playable Director 컴포넌트는 Timeline instance가 언제 재생되고 어떻게 시계를 업데이트하고, 재생이 종료되면 어떻게 되는지 설정합니다.

Animation과 비슷한 역할 이지만, 여러 Object를 사용하여 Animation들을 만들수 있다는 점에 다른 차별점을 두고 있습니다. 또한 Timeline은 여러 Animation을 엮어 Audio, Cinematic Content, Particle Sequence로 하나의 복잡한 Cut Scene을 만들 수 있다는 점에중점을 두고 있습니다. 

## 작성

![](../../../.gitbook/assets/image%20%28242%29.png)

위의 사진은 해당 프로젝트의 CutScene에 대한 Timeline Asset의 내용입니다.







