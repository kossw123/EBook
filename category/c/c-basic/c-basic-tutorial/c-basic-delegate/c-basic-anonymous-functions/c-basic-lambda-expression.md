---
description: 'C# Basic Lambda Expression'
---

# C\# Basic Lambda Expression

## 무엇을 하려고 하는가?

* delegate를 구현하는 방법중 하나인 Lambda Expression에 대한 개념과 사용방법에 대해 기술합니다.

## Lambda Expression\(람다식\)이란?

* delegate를 생성하기 위해 Lambda Expression을 통해 구현하는 방법이 있는데, 이는 Anonymous Method\(무명 함수\)와 마찬가지로, 이름이 없는 함수를 간단하게 만드는 방법입니다.
* 그에 대한 방법은 아래와 같습니다.

```csharp
//                ↙ Lambda Expression
myDelegate my2 = () => { Console.WriteLine("Lambda Expression을 통한 함수 생성"); };

/*
    Output : Lambda Expression을 통한 함수 생성
*/
```

위의 예시는 delegate void myDelegate\(\); 라는 문장으로 delegate를 생성 후 Main\(\) 함수에서 예시의 코드를 작성한 결과 입니다.

Lambda식을 표현하는 방법은 보통 2가지로 나눠집니다.

1. \(매개변수\) =&gt; {실행 내용};
2. \(매개변수\) =&gt; 선언한 반환식;

## Lambda Expression의 이유

* delegate의 코드의 간결함을 위해 적용되었으며, 가독성이나 이런 부분에서는 개인차가 존재합니다.

