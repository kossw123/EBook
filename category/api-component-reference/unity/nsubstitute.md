---
description: NSubstitute
---

# NSubstitute

## NSubstitute

{% embed url="https://nsubstitute.github.io/help/getting-started/" %}
NSubstitute Document
{% endembed %}

> NSubstitute is a friendly substitute for . NET mocking libraries. It has a simple, succinct syntax to help developers write clearer tests. NSubstitute is designed for Arrange-Act-Assert (AAA) testing and with Test Driven Development (TDD) in mind.
>
> NSubstitute은 (는) . NET mocking libraries에 대한 친근한 대체품입니다. 개발자가보다 명확한 테스트를 작성할 수 있도록 간결하고 간결한 구문이 있습니다. NSubstitute은 AAA (Arrange-Act-Assert) 테스트를 위해 설계되었으며 TDD (Test Driven Development)를 염두에두고 설계되었습니다.

NSubstitute를 사용하기 위해 임의의 interface 예시를 아래의 Code Block과 같이 생성하고 일련의 과을 사용하여 NSubstitute를 생성합니다.

```csharp
public interface ICalculator
{
    int Add(int a, int b);
    string Mode { get; set; }
    event EventHandler PoweringUp;
}

calculator = Substitute.For<ICalculator>();        // NSubstitute 생성 부분
```

임의의 interface인 ICalculator를 정의 하는데 있어서 `Substitute.For<ICalculator>();`를 사용합니다. 이것은 모의 객체를 생성하는 부분입니다.

`Substitute.For<생성할 객체의 타입>(); `

위와 같은 문법으로 모의 객체를 생성합니다.

이제 생성한 모의 객체(Mock Object)를 가지고 반환값을 가져야 합니다. Complete Project File에서는 Test Script에서의 반환값을 가져야하는 Logic을 아래와 같이 선언하였습니다.

```
_firstQuestion.IsRightAnswer("ok").Returns(true);
_firstQuestion.IsRightAnswer("nope").Returns(false);
```

1. `_firstQuestion`: 모의 객체 변수
2. `IsRightAnswer("ok")` : IsRightAnswer() 함수는 Question Class의 RightAnswer와 parameter로 들어가는 변수에 "ok"를 넣었으니 "ok"와 RightAnswer를 비교합니다.
3. `Returns` : `IsRightAnswer()` 함수가 Ok를 반환하면 True, Nope을 반환하면 false입니다.

## 주석

{% tabs %}
{% tab title="Mocking Libraries란?" %}
{% hint style="info" %}
일단 Mocking이라는 단어에 대해 설명해 드리자면

Unit Test(단위 테스트)에 사용되는 단어입니다. Test중에 임의의 Object는 다른 Object에 종속되거나 Coupling 될 수 있기 때문에 임의의 Object type의 Mock Object(모의 객체)를 생성하여 Object가 동작하는 Logic과 동일하되, Test Case에서는 Mock Object를 통해 Test합니다.
{% endhint %}
{% endtab %}
{% endtabs %}
