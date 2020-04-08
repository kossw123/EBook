---
description: tutorial TriviaGame TDD
---

# tutorial TriviaGame TDD

##  무엇을 하려고 하는가?

* 게임을 제작할 때 많은 버그와, 이슈, Side Effect가 발생됩니다. 이를 해결할 때 QA\(_Quality Assurance_\) 라는 가이드라인을 통한 수정을 할 수도 있지만, 제작하는 과정에서 미리 테스트를 통해서 발견 할 수 있는 버그들은 제작과정에서 고치는 것이 수월합니다.
* 그 방법의 하나로써 TDD\(Test - Driven - Development\)라는 방법론을 채택하여 Unity 자체에 탑재된 기능의 하나인 Test Runner를 사용하여 Trivia Game을 만들고, Unit 단위로 올바른 값이 들어간지 체크할 수 있습니다.

{% hint style="info" %}
보통 게임을 플레이 하면서 충돌이나, 플레이가 안된다거나, 렌더링이 안된다거나 등에 대한 오류가 대부분을  차지하고, 값에 대한 올바른 값이 들어갔는가에 대한 오류는 아주 극소수입니다.

하지만 그런 오류를 잡는것 뿐만 아니라, Unit 단위의 테스트를 통해, Unity로 Game 뿐만이 아니라 App 개발에 있어서 아주 유용하게 사용할 것 같아서 이 주제로 문서를 작성하게 되었습니다.
{% endhint %}

* 해당 자료는 상업적으로 이용하지 않고 이용되서도 안됩니다.
* 아래의 링크를 타고 들어가면 해당 문서에 대한 보다 풍부한 자료가 있습니다.

{% embed url="https://engineering.etermax.com/how-to-tdd-in-unity-using-the-mvp-pattern-a646ffbe996f" caption="MVP\(Model ,View, Presenter\)패턴을 사용한 TDD 사용법" %}

## 작성법





