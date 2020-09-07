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

## 개요 

{% embed url="https://drive.google.com/file/d/1H4mX9yAwuhBzSbh2bRxM7jIuuUQsDoWu/view" caption="유니티로 시네마틱 제작하기 : 타임라인 & 시네머신 Sample File" %}

**Creating Believable Visuals와 Cinemachine을 작성하기 위한 HRDP package의 라이브러리가 오류가 생기기 때문에, OZTV님의 샘플 프로젝트를 사용하는점 양해 부탁드립니다.**

해당 문서의 중점은 아래와 같습니다.

* Timeline을 통해 Object의 Animation을 시간의 흐름에 따라 이동, 회전 및 활성화 여부를 할당할 수 있습니다.
* Cinemachine을 통해 생성한 Virtual Camera들로 영화같은 Scene을 표현할 수 있습니다.
* Unity는 작성한 Scene을 RecorderClip을 통해 영상파일로 추출할 수 있습니다.

지금은 Asset Store에 없는 Adam Character Pack을 쓰고 있습니다. 그렇기 때문에, 실습할 때는 꼭 샘플 프로젝트를 다운받아서 사용하시길 바랍니다.

![](../../../.gitbook/assets/image%20%28243%29.png)

이를 통해 Timeline을 작성합니다. Project View에서 간단한 Timeline Asset을 생성하여 Hierarchy창에 추가한다면, Timeline 이름과 같은 Object가 생성되고 Playable Director Component가 추가됩니다.

![](../../../.gitbook/assets/image%20%28245%29.png)

Playable Director를 통해 **Timeline instance\(Scene에서 사용할 수 있는 Timeline Object\)**와 **Timeline Asset\(Project View에서 Asset형식으로 생성한 데이터\)** 사이의 링크를 저장합니다. Playable Director 컴포넌트는 Timeline instance가 언제 재생되고 어떻게 시계를 업데이트하고, 재생이 종료되면 어떻게 되는지 설정합니다.

Animation과 비슷한 역할 이지만, 여러 Object를 사용하여 Animation들을 만들수 있다는 점에 다른 차별점을 두고 있습니다. 또한 Timeline은 여러 Animation을 엮어 Audio, Cinematic Content, Particle Sequence로 하나의 복잡한 Cut Scene을 만들 수 있다는 점에중점을 두고 있습니다. 

## 작성법 

![Red : Key&#xAC12;&#xC744; &#xD1B5;&#xD55C; &#xAC12;&#xC758; &#xC774;&#xB3D9; / Green : Activation Track &#xC124;&#xC815; / Blue : Animation Track &#xC124;&#xC815;](../../../.gitbook/assets/image%20%28244%29.png)

위의 사진은 해당 프로젝트의 CutScene에 대한 Timeline Asset의 내용입니다.

Timeline은 Key, Animation, Activation Track을 통해 Cut Scene을 구성할 수 있습니다.

해당 프로젝트는 미리 준비된 Asset을 Timeline Object instance에 필요한데로 나열하는 것에 불과 하지만, 가장 중요한점 2가지가 있습니다.

* Timeline에 등록한 Animation의 Offset 설정
* Cinemachine의 Virtual Camera의 설정
* Track과 Key의 추

### Timeline에 등록한 Animation Offset 설정 

Prefab으로 된 Object의 Animation은 위치에 상관없이 자연스럽게 실행되지만, 일부 설정이 안된 Prefab은 Animation을 실행 시킬 때, Animation의 위치 설정값에 따라 실행되는 경우가 있습니다.

이를 해결하기 위해 2가지 방법이 존재합니다.

1. Track에서 Offset을 설정
2. Clip에서 Offset을 설정

Timeline에 등록한 Track을 설정하면 Offset을 설정할 수 있습니다.

![](../../../.gitbook/assets/image%20%28246%29.png)

Track Offsets의 설정에 따라 다음과 같이 설정됩니다.

* Apply Transform Offsets : 설정한 위치값에서 실행합니다.
* Apply Scene Offsets : Scene의 위치한 현재 Object의 위치에서 실행합니다.

샘플 프로젝트에서는 Apply Scene Offsets을 사용하여 Scene에 위한 현재 Object의 위치에서 실행하고 있으며, Animation Track과 Key값을 통한 Tram\(지하철\)의 이동값을 통해 Timeline을 구성하고 있습니다.



### Cinemachine의 Virtual Camera 설정

Cinemachine Package는 편한 만큼 이전부터 존재했고, 다양한 설정값이 존재합니다. 그러나 시간이 지나면서 Unity 자습서에 나와있는 사용법과는 다소 괴리감이 존재한다고 생각합니다.

#### Virtual Camera의 개요

Virtual Camera는 Unity에서 Camera를 추가하는 것과는 달리 좀 더 가벼운 감이 있습니다. 기존 Camera Component보다 좀 더 확장된 기능을 가지고 있지만, Camera Controller의 개념으로 생각하시면 될것 같습니다.

Cinemachine은 Cinemachine Brain이라는 Camera Object에 붙어있는 Component를 참조하며, Blend 및 LookAt, Follow 기능등, Script로 작성해야하는 것들에 대한 부담을 줄여줍니다.

* LookAt : 바라볼 대상을 설정합니다.
* Follow : 대상이 이동함에 따라 Camera도 같이 이동합니다.
* Lens : 카메라가 대상을 바라볼때 어떻게 바라볼지에 대한 설정값을 입력합니다.
* Transition : 설정값에 따라 Blend합니다.
* Body : 카메라가 대상을 추적하는 과정의 설정값 입니다.
  * 기본적으로 Transposer로 설정이 되어 있어서 추적이 되지만, 여러 상황에 맞춰서 설정값을 바꿔서 추적할 수 있습니다.
* Aim : 카메라가 대상을 어느 시점에서 바라볼지 설정합니다.

여기서 중요한 점은 여러대의 Virtual Camera를 사용하여 Camera가 여러대 있는 듯한 연출을 통해, Scene에 퀄리티를 추가하는 것이며, 이를 위해 총 4대의 Virtual Camera를 추가하고 있습니다.

![](../../../.gitbook/assets/image%20%28250%29.png)

4대의 Virtual Camera를 Timeline의 Cinemachine Track을 통해 움직이고 있습니다.

![](../../../.gitbook/assets/image%20%28249%29.png)







