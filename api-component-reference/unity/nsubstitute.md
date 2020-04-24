---
description: NSubstitute
---

# NSubstitute - 작성중

## NSubstitute

{% embed url="https://nsubstitute.github.io/help/getting-started/" caption="NSubstitute Document" %}

> NSubstitute is a friendly substitute for . NET mocking libraries. It has a simple, succinct syntax to help developers write clearer tests. NSubstitute is designed for Arrange-Act-Assert \(AAA\) testing and with Test Driven Development \(TDD\) in mind.
>
> NSubstitute은 \(는\) . NET mocking libraries에 대한 친근한 대체품입니다. 개발자가보다 명확한 테스트를 작성할 수 있도록 간결하고 간결한 구문이 있습니다. NSubstitute은 AAA \(Arrange-Act-Assert\) 테스트를 위해 설계되었으며 TDD \(Test Driven Development\)를 염두에두고 설계되었습니다.

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



{% tabs %}
{% tab title="Mocking Libraries란?" %}
{% hint style="info" %}
일단 Mocking이라는 단어에 대해 설명해 드리자면

Unit Test\(단위 테스트\)에 사용되는 단어입니다. Test중에 임의의 Object는 다른 Object에 종속되거나 Coupling 될 수 있기 때문에 
{% endhint %}
{% endtab %}
{% endtabs %}

