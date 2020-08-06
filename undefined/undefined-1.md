# 바 사 아 자 차

## 바



## 사



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



## 자





## 
