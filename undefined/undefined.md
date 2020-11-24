# 가 나 다 라 마

## 가

### 객체

* 소프트웨어의 세계에서 구현해야할 대상

### 객체 vs 클래스 vs 인스턴스

* 객체 : 소프트웨어의 세계에서 구현해야할 대상
* 클래스 : 대상를 구현하기 위한 설계도
* 인스턴스 : 설계도에 따라 소프트웨어에서 구현된 실체

{% embed url="https://cerulean85.tistory.com/149" %}



### 깃\(Git\)

버전관리 Tool중 하나로써 가장 대중적으로 알려진 VCS중 하나로써, Git Repository라는 저장소에 소스 코드를 넣어서 **로컬에서 버전관리를 합니다**.

### 깃허브\(Github\)

Git과 똑같은 개념이지만, Cloud Server를 사용하여 인터넷 상에서 Repository를 생성하여 소스 코드를 관리합니다.



## 나



## 다

### Debug

* 소프트웨어에서 발생한 문제를 찾는 것
* 개발중인 상태를 의

## 라

### 라이브러리

* 활용 가능한 도구\(클래스 혹은 인터페이스, 컴포넌트 등\)의 집합
* 작성자가 임의대로 호출하여 사용할 수 있습니다.



## 마

### 메타 데이터\(Meta Data\)

Unity에서의 Metadata는 다음과 같이 구성되어 있습니다.

* Asset을 식별할 수 있게 하는 고유한 값
* Asset에 대한 설정값

위 2개의 정보들로 구성이 되어있는데, Metadata를 삭제한다면, Unity는 Asset을 식별하긴 하지만, 새로운 Asset으로 인식하여 새로운 Metadata를 생성하기 때문에, 기존의 설정값과, 그에 따른 Dependency는 모두 엉망이 되고 맙니다.

### 메타 데이터의 VC\(Version Control\)

Unity같은 Framework는 resource조차 Metadata라는 데이터를 가공하여 원본과 같이 인식하기 때문에, 협업에 관하여 많은 문제들이 발생합니다.

1. 같은 시간대에 다른 두 개발자가 다른 Metadata를 가진 Asset을 업로드 한다면?
2. Metadata를 Push하지 않는다면?

등등 과 같은 문제점이 발생하는데 이때는 UnityPackage 형태로 packging\(포장\)하여 VCS에서 주고 받으면 됩니다.

또는 Unity 자체적으로 제공하는 Collaborate를 통해 CI를 제공하기 때문에, 이를 활용하는 것도 하나의 방법이 될 수 있습니다.

{% embed url="https://docs.unity3d.com/Manual/UnityCloudBuildVcs.html" %}





