---
description: TR TriviaGame TDD
---

# TR TriviaGame TDD - 작성중

## 무엇을 하려고 하는가?

* TriviaGame을 이루는 Script 구조에 대해서 좀 더 모호성을 줄이고 가독성을 높이는 방법이 사용되었기 때문에 그에대한 명령문과, 방법에 대해 자세히 다룹니다.

## namespace를 통한 다른 기능, 같은 이름에 대한 구별화

* TriviaGameView.cs에서 아래와 같이 namespace를 통한 클래스 이름에 대한 충돌을 방지하고 있습니다.
* 사용방법 보다는 전체적인 Script들의 Class에 대해 구분지어 놓은 구조에 대해서 살펴보시면 분명 얻어가는 것이 있다고 생각합니다.

```text
using System.Linq;
using TMPro;
using TriviaGame.Domain;
using TriviaGame.Presentation;
using UnityEngine;

namespace TriviaGame.Delivery
{
    // 
    public class TriviaGameView...
}
```

{% hint style="info" %}
namespace TriviaGame.Delivery

위의 구문을 통해 namespace를 선언하고 있습니다. 여기서는 TriviaGame.Delivery 자체가 하나의 namespace 입니다.

접근시 using명령문을 써서 상단에 미리 선언해야 합니다.

ex\) using TriviaGame.Domain;



GamePlayScreen Object에서는 이에 관련된 Component들을 확인 할 수 없는데 어떻게 접근이 가능한 것인가에 대한 고찰을 해보면 namespace는 선언과 동시에 

**자동적으로 전역 namespace 라는 단일 공간에 선언이 됩니다. 이로 인해 전역적인 만큼 Script가 존재하기만 한다면 검색하여 해당 namespace 주소를 가져와 컴파일단위로 함께 처리 됩니다.**

여기까지만 알아도 이 Project에 대한 궁금증을 해결되었기에, 나중에 기회가 된다면 좀 더 깊게 알아보겠습니다.
{% endhint %}

##  Action, Delegate

delegate는 함수포인터의 기능을 가지고 있고 그렇기 때문에 참조하기 위한 변수를 만들어야 합니다.

{% embed url="https://mrw0119.tistory.com/19" %}

하지만 Action은 반환값과 인자값이 없는 함수 포인터의 기능을 가지고 있습니다.

즉, 일련의 작업과정 수행을 위해 호출합니다.

{% embed url="https://bong9.blog/2014/09/02/action-%EA%B3%BC-delegate-%EB%9E%8C%EB%8B%A4%EC%8B%9D-action-func-event/" %}

Delegate와 Action은 C++에서 사용하는 포인터와 같은 기능을 하고 있지만, 주소를 가리키는게 변수가 아닌 함수 라는 것을 유념해야 합니다. 

## NUnit

* 개요
  * 성공적인 Software를 작성할 때는 SDLC\(Software Development Life Cycle\)이라는 주기에 따라 작성되는 경향이 있다고 합니다.
  * 보통 이러한 개발 방법론과 같은 경우 적용 범위가 한없이 넓기 때문에 처음 하기에는 적용도 어려울 뿐만 아니라 자칫 잘못하면 개발 난이도가 높아져 난항을 겪으실 수 있습니다.
  * Nunit은 이러한 개발 주기를 가지고, .NET으로 개발한 Software에서 Test부분에서 유용하게 사용될 수 있습니다.
  * NUnit은 .NET 언어가 Test Case를 작성하기 위한 필수 Library라고 할 수 있습니다.

![&#xC18C;&#xD504;&#xD2B8;&#xC6E8;&#xC5B4; &#xAC1C;&#xBC1C; &#xC0DD;&#xBA85; &#xC8FC;&#xAE30;\(SDLC\)](../../.gitbook/assets/image%20%2893%29.png)

{% embed url="https://ko.wikipedia.org/wiki/%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4\_%EA%B0%9C%EB%B0%9C\_%EC%88%98%EB%AA%85\_%EC%A3%BC%EA%B8%B0" caption="SDLC에 대한 Document" %}

{% embed url="https://www.c-sharpcorner.com/article/introduction-to-nunit-testing-framework/" caption="NUnit을 사용한 Test Case 작성 예시" %}

* 사용된 Syntax, Statement 정리
  * `[TestFixture]` : Test, Setup, teardown method를 포함하는 클래스를 표시하는 기능입니다.
  * `[SetUp]` : TestFixture 내부에서 사용되어 테스트 메소드 호출전에 수행되는 공통 기능의 집합입니다.
  * `[Test]` : Test할 Method를 식별합니다. 사용하지 않는다면 NUnit을 사용하는 Project에서는 식별이 되지 않습니다. `[TestFixture]` 에서 다른 조건들을 확인하기 위해 `Assert` Class의 함수를 사용합니다.
    * Assert Class
      * `AreEqual(object expected, object actual)` : 기대값과 실제값을 비교하여 모두 null이거나 둘 다 동일한 값을 갖는 경우 두 개체는 동일한 것으로 간주합니다.
      * 



