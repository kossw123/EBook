# 바 사 아 자 차

## 바

### 버전 관리 시스템\(Version Control System : VCS\)

협업을 하여 어떠한 응용 프로그램을 제작시, 여러 인력과 똑같은 Convention\(협업시 지켜야할 규약\)을 가지고 일을 하는데 있어서, 가장 중요한 코드의 추가 및 변경을 기록하여 

* 특정 시점으로 돌아가거나, 
* 문제가 생긴 파일을 복원하는 등,

프로그램을 개발하는 현장에서 사용하는 프로그램입니다.

보통 어떤 프로젝트를 진행하는 과정에서 수정사항이나, 백업이 필요한 부분을 저장하기 위해 여러 개의 파일을 같은 장소에 복사하고 파일명을 구분되게 하는 방법을 사용하는데, **진행될수록 어떤파일이 어떤 부분을 변경하는 이유에 대해 찾기 어려워 지고, 최악의 경우 파일을 지워버릴 수 있기 때문에**, 이런 시스템이 탄생하게 되었습니다.

대중적인 VCS중 하나는 Git, SVN, CVS\(Concurrent Versions System\)등이 있습니다.



### 빌드\(Build\)

프로그래밍에서 Build는 작성한 소스 코드를 실행 가능한 결과물로 만드는 일련의 과정을 의미합니다. 이러한 Build Tool은 우리가 흔히 접할 수 있는 Visual Studio, Eclipse 등과 같은 IDE가 있습니다.

하지만 우리가 접하는 IDE는 필요한 기능의 집합이고, 그 중에는 원시 코드를 컴파일 방식 또는 인터프리터 방식으로 기계어로 번역하는 기능도 있을 것이고, 여러 필요한 기능들이 다 각기 다른 개념으로 존재합니다.

**그 중 Build는 궁극적으로 프로그램을 실행 시킬 수 있는 결과물을 만들어 내는 것이며, 위의 과정 또한 Build의 과정이라고 볼 수 있습니다.**

* Unity에서의 Build란?
  * 타겟 플랫폼을 설정하고 Scene을 통합하여, Build Setting의 설정을 통해 하나의 실행가능한 파일로 바꿔주는 것 입니다.
  * 이때, 크로스 플랫폼\(Cross Platform\)을 지향하는 경우 하드웨어 및 실행 방식에 따라 기본적인 차이가 생겨, 프로젝트 중 일부는 주 프로그램에서 다른 플랫폼 코드로 이식할 수 없습니다.

{% embed url="https://docs.unity3d.com/kr/530/Manual/CrossPlatformConsiderations.html" %}



### 빌드\(Build\) vs 컴파일\(Compile\)

보통 IDE를 사용하기 때문에, Build한다 = Compile한다 라는 의미로 혼용될 여지가 존재합니다.

* Compile = 내가 짠 코드를 기계어로 변환하는 과정
* Build = 작성한 코드를 실행 가능한 파일로 만드는 과정 

요약하자면, Compile은 Build화 하기 위한 과정 / Build는 실행 가능한 파일로 만드는 것 입니다.



### 빌드의 자동화

프로그램을 Build를 통해 실행가능한 결과물로 만든 다음 고객에게 제공하면, 고객은 사용 후 버그점과 개선점을 개발자에게 제시합니다.

이때, 프로그램을 수정하고 다시 Build화를 통해 배포하는 과정이 무수히 반복되기 때문에, 도중 일어나는 실수와 비효율적인 방식은 누구도 체험하고 싶지 않습니다.

그렇기 때문에, 자동화를 시킨다면, 비효율적인 방식을 바꾸고, 사람이 하면 실수가 일어날 수 있는 일을 프로그램이 대신한다면 이러한 사고를 예방할 수 있습니다.

**위의 예시처럼 사람이 하지 않기 때문에, 자동화라는 단어가 나온 것이라고 볼 수 있습니다.**

Visual Studio에서 Build 버튼을 누르면 Compile이 자동으로 되는 것도 Compile의 자동화라고도 볼 수 있습니다.

## 사

### SOLID 원칙

* 단일 책임의 원칙 \( SRP : Single Responsibility Principle \)
* 개방 - 폐쇄의 원칙\( OCP : Open - Closed Principle \)
* 리스코프 치환 원칙 \( Liskov Substitution Principle \)
* 의존 역전 원칙 \( DIP : Dependency Inversion Principle \)
* 인터페이스 분리 원칙 \( ISP : Interface Segregation Principle \)

{% embed url="https://dev-momo.tistory.com/entry/SOLID-%EC%9B%90%EC%B9%99" %}





### CICD\(Continuous Integration / Continuous Delivery or Continuous Deploy\)

단어 그대로 지속적인 통합, 지속적인 배포 혹은 결합을 의미하며, **프로그램 개발 단계를 자동화 하여 보다 짧은 주기로 고객에게 제공하는 방법**입니다.

CI

* Continuous Integration은 자동화된 빌드 및 단위 테스트가 수행된 후 코드의 변경사항을 VCS에 정기적으로 병합하는 개발방식입니다.

CD

* CD는 의미에 따라 최종 목적지가 달라집니다.
  * Continuous Delivery
    * 개발자들이 프로그램에 적용한 변경사항이 Unit Test를 통해 Repository에 자동으로 업로드 되는 것을 의미합니다.
    * 이때 프로그램의 운영자는 해당 Repository에서 실시간으로 배포할 수 있습니다.
  * Continuous Deployment
    * 개발자들의 변경사항을 Repository에서 고객이 사용 가능한 환경까지 자동으로 배포하는 것을 의미합니다.

위 내용의 대한 개략적인 내용을 아래의 그림과 같습니다.

![https://www.redhat.com/ko/topics/devops/what-is-ci-cd](../../.gitbook/assets/image%20%28231%29.png)

이 방법을 도입하여 구현하기 위해서는 많은 고려해야 할 점들이 있으며, 이를 해결하기 위해 나온 솔루션들은 다음과 같습니다.

* CircleCI, Travis, Jenkins, Bamboo, GitLab 등등

{% embed url="https://www.katalon.com/resources-center/blog/ci-cd-tools" %}



## 아

### 인스턴스\(instance\)

구글링을 통해 여러가지 정보를 살펴봤지만, 각자 얘기하는 개념이 상이하고, 와닿지 않는 느낌이 강했습니다. 그렇기 때문에, 이해할 수 있는 가장 포괄적인 개념을 가지고 설명하겠습니다.

* **가장 포괄적인 개념은 "Class를 통해 소프트웨어에서 구현된 실체"라고 보시면 될것 같습니다.**

그렇다면 Class라는 설계도를 통해 new 키워드를 사용하여 메모리에 할당된 어떤 변수는 인스턴스인가?

* 꼭 그렇지만은 않습니다. 왜냐하면 
  * 애초에 객체를 생성하기 위해 Class가 필요하고 
  * 그 결과물이 인스턴스라면 
* 객체와 인스턴스에 대한 개념은 모호해지고 이러한 차이점은 나타나지 않을 것입니다.

그럼에도 불구하고 다른 단어로 비슷한 개념을 설명하는 이유가 무엇인가?

* 객체는 **대상을 가리키는 말. 즉, 개념**에 가깝고
* 인스턴스는 **관계를 가리키는 말. 즉, 개념과의 관계** 에 가깝기 때문입니다.



* OOP에서의 인스턴스
  * "**Class를 new 키워드를 통해 메모리에 할당한 객체**"를 실제로 사용할 때를 인스턴스라고 합니다.
  * 클래스와 객체 사이의 관계로 한정 지어서 사용할 필요 없이, "**어떤 개념으로부터 생성된 복제본"** 이라고도 말합니다.
* Unity에서의 인스턴스
  * 실제로 Game에서 돌아가게 하는 Prefab이나 GameObject들을 Scene에 배치하고 사용하게끔 하는 과정을 인스턴스화 라고 합니다.



### SI\(System Integration\)

* 서비스에 필요한 하드웨어, 소프트웨어, 네트워크, 스토리지 등을 통합하는 서비스를 제공하는 것입니다.
* 해당 분야의 업무는 클라이언트와의 미팅을 통해 원하는 서비스가 무엇인지, 어떤 기술을 요구하는지 파악하여 정의합니다.
* 이 분야의 목적에 맞게 UI, UX설계, App 개발, 하드웨어나 네트워크의 동기화 등의 프로세스를 가지고 있습니다.



### Event Driven Programming\(사건 기반 프로그래밍\)

분산된 시스템 간에 이벤트\(사건\)이 발생하면, 해당 이벤트\(사건\)에 맞는 행동을 생성, 발행\(Publishing\)하여 필요로 하는 수신자에게 전달하는 코드 작성 방식입니다.

Event Driven Architecture를 구성하기 위해 Event Driven Programming을 하려면 특정 패턴\(Event driven Pattern\)을 사용해야 하는데  해당 패턴의 구성요소는 보통 다음과 같습니다.

* Event generator : 시스템 내, 외부의 상태 변화를 감지하여 표준화된 형식의 이벤트 생성
* Event Channel : 이벤트를 필요로 하는 시스템 까지의 발송
* Event Processing engine : 수신한 이벤트를 식별, 처

위의 구성요소를 사용하여 3가지 방법으로 처리합니다.

* Simple Event Processing
  * 각각의 이벤트가 직접적으로 수행해야할 Action과 매핑되어 처리합니다.
  * 실시간으로 생기는 작업을 처리할 때 유용하며, 처리시간과 비용의 손실이 적습니다.
* Event Stream Processing
  * 중요도에 따라 필터링하여 걸러진 이벤트만을 전송합니다.
  * 정보의 흐름을 처리할 때 사용하며, BAM\(Business Activity Monitoring\)에 유용합니다.
* Complex Event Processing
  * 일상적인 패턴을 감지하여 더 복잡한 이벤트의 발생을 **추론**합니다.
  * ex\) 사람의 패턴을 파악하기 위해, 상위 이벤트인 해당 국가의 성향이라는 이벤트를 추론할 수 있다는 의미입니다.
    * 예시가 너무 비약됐으, 이런 느낌이라는 것만 알아주시면 될 것 같습니다.

이러한 프로그래밍의 방식은 아래와 같은 장단점을 가지고 있습니다.

* 장점
  * 느슨한 결합\(DeCoupling\)
  * 다른 시스템의 정보를 알 필요가 없다.
  * 확장성, 탄력성을 고려하기 쉽다.
* 단점
  * 시스템 전체를 파악하기 어렵다.
  * 디버깅이 어렵다.
  * Transaction 단위가 존재하면, 해당 Transaction을 Rollback해야하는 것을 고려해야 합니다.

보통 Unity에서는 실시간으로 처리해야 할 일들이 많기 때문에, 보통 Simple Event Processing을 사용한 Event System을 자체적으로 제공하는 듯 합니다. Script 작성시 UnityEvent&lt;T0, .... T3&gt; generic Class를 보며, 이러한 추론을 했습니다. 

이를 바탕으로 좀 장기적인 시선으로 본다면, 익숙해 지기만 한다면, GTA나 여러 오픈월드 게임을 비롯한 많은 Event가 발생하는 프로그램에서의 깊은 이해가 가능할 것 같습니다.



### IDE\(Integrated Development Enviroment\)

통합 개발 환경이라는 직역 그대로의 의미이며, 통합된 개발 환경을 제공하기 위해 GUI를 제공하고, 필요한 기능의 집합입니다.



### Universal Windows Platform

MS사가 개발한 API중 하나로써, 제각기 개발할 필요 없이 다음과 같은 플랫폼들의 개발을 돕기 위해 사용됩니다.

* Window10
* Window10 Mobile
* Xbox One
* Microsoft HoloLens
  * MS가 개발한 혼합현실 기반 착용 기기 

해당 플랫폼을 사용하는 장점은 다음과 같습니다.

* 화면 종류 뿐만이 아니라 다양한 입력 수단을 지원
  * 화면 종류 : iOS, Android, PC, Tablet, 
  * 입력 수단 : 키보드, 마우스, 터치, 펜
* 대부분의 개발 프레임워크가 지원하는 환경
* MS Store를 사용 가능
  * 현재 MS Store의 관리와 관심이 없기 때문에 장점이긴 하지만, 단점인 면이 더 큽니다.\(...\)
* UWP 개발앱은 샌드박스 형식으로 설치, 제거됩니다.
  * 이전의 레지스트리를 변경하여 제거가 어려웠던 시점보다는 많이 편리해졌습니다.
* 지속적인 UX 개
  * 코타나 인공지능 프로그램이 존재합니다.
  * Bing을 기반으로 사용자의 관심사를 수집하거나, 음성인식을 통해 Siri, 빅스비와 같은 서비스를 제공합니다.
* multi - paradigm을 지원하는 .NET이기 때문에, 다양한 언어로 UWP를 작성할 수 있습니다.

{% embed url="https://unity3d.com/kr/partners/microsoft/porting-guides" %}



### Interpreter

* Complie 방식과는 달리 작성자가 코드를 짜는 도중에 실시간으로 기계어로 번역하는 프로그램

## 자

### 잡 시스템\(C\# Job System\)

* C\#에서 멀티스레드 방식의 코드를 작성 할 수 있게끔 도와주는 System
* 멀티스레드 방식으로 프로그래밍하지만, 스레드를 별도로 생성하진 않습니다.

Unity는 기본적으로 하나의 스레드에서 시스템이 동작합니다.  
Unity 내부에서는 여러가지 싱글 스레드가 존재하는데 다음과 같습니다.

1. Main : 익숙하게 작업을 진행하는 스레드
2. Worker : CPU 코어 갯수에 맞춰 월드의 변경사항을 적용하여 렌더링 준비를 해주는 스레드
3. Render : 렌더링 과정을 처리하는 스레드

그 중 Job System은 Worker Thread에 작업을 지시하게 해주는 System입니다.



이러한 Job System을 사용하기 위해서는 생성하고, 스레드에 예약을 걸어야 합니다.  
Job 작성 방법은 다음과 같습니다.

1. Job interface를 상속하고 Struct에 구현하기
2. 해당 Job이 사용하는 Field를 선언
3. 상속된 Execute를 구현하고 그 안에서 Job을 구현

Job 예약 방법은 다음과 같습니다.

1. Job을 instancing
2. Job의 Field Data를 초기화
3. Schedule 함수를 호출

```csharp
using Unity.Burst;
using Unity.Collections;
using Unity.Jobs;

public class Fractal : MonoBehaviour
{
    struct Fundamental : IJob
    {
        ///... Field
        
        public void Execute(int index)
        {
            ///...Job 구현
        }
    }
    
    void Start()
    {
        /// Job 초기화 후 예약
        var job = new Fundamental
        {
            ///...Field 초기화
        }.Schedule();
    }
}
```



다음과 같은 Job System을 구현했을 때 장점은 다음과 같습니다.

1. 엄청 빠릅니다. - Fractal을 만들어 1000개 이상의 Object를 회전시키는데, Batch가 10~20정도 입니다.   Job System을 사용하지 않는 경우의 Batch 7000 - 8000정도 입니다.
2. Worker Thread를 사용하여 다른 Thread를 추가하지 않기 때문에 메모리가 절약됩니다.
3. Job System 내부에 Safety System이 있어서 Race Condition을 방지 합니다.
4. Unity 자체 로직에 맞춰 돌아가서 안정적인 프레임이 기대됩니다.

그러나 장점만 보기에는 멀티스레드 방식인 만큼 단점도 그대로 적용됩니다.

1. Job에서 Static Data에 접근할 수 없습니다.
2. 예약된 Batch는 무조건 Flush\(실행, 출력\)해야 합니다.
3. NativeContainer의 Variable를 수정하기 위해서는 데이터를 임시로 복사하고 다시 저장해야 합니다.
4. 일반 멀티 스레드에서는 무한루프 발생 시 해당 스레드만 무한루프에 빠지지만,  Unity는 전체가 무한루프에 빠진다.
5. Worker Thread에 무거운 기능을 추가하기가 어렵습니다.

## 

