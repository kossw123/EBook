---
description: 'C# Basic delegate'
---

# C\# Basic delegate

## 무엇을 하려고 하는가?

* C,C++의 함수 포인터처럼 함수를 안전히 캡슐화하는 대리자\(delegate\)의 개념과 사용방법 및 사례를 소개합니다.



## 대리자\(delegate\)란?

* 위에서 얘기한대로 안전하게 함수를 캡슐화 하는 문법입니다.
* 이런 캡슐화를 통해 얻을 수 있는 이점은 아래와 같습니다.
  * 개체 지향이므로 delegate를 통해 재사용성이 뛰어납니다.
  * 형식이 안전합니다.
  * 보안이 유지됩니다.

이런 delegate는 일반적으로 원형을 제공하거나, 익명 함수를 통해 사용하여 생성합니다.

{% page-ref page="c-basic-anonymous-functions/" %}

해당 delegate는 아래와 같은 방법으로 구현합니다.

1. 등록할 함수의 형식대로 delegate를 선언합니다.
2. delegate에 등록할 함수를 생성합니다.
3. 생성한 delegate의 객체를 선언하고 함수를 등합니다.
4. 생성한 delegate 객체를 호출합니다.

해당 구현 과정을 코드로 작성하면 아래와 같습니다.

```csharp
using System;

namespace ConsoleApp {
    class Program {
        // 2. delegate에 등록할 함수(여기선 클래스 멤버 함수)를 생성합니다.
        class DelClass { 
            public void Method() => Console.WriteLine("DelClass.Method()");
        }
        
        // 1. 등록할 함수의 형식대로 delegate를 선언합니다.
        public delegate void myDelegate();

        static void Main(string[] args) {
            var sample = new MethodClass();
            
            // 3. 생성한 delegate의 객체를 선언하고 / 함수를 등록합니다.
            Del del = sample.Method1;
            
            // 4. 생성한 delegate 객체를 호출합니다.
            del();
        }
    }
}
```



이러한 대리자를 통해 일정 형식만 유지하면서, 보안 및 재사용성을 챙길 수 있는 이점이 있습니다. 하지만 이러한 이점에 저는 아래와 같은 불편함을 느꼈습니다.

* 재사용성이 있기 때문에, 콜백함수의 오류에 빠져 데이터의 종속성이 생기는 점이 있습니다.

이 부분은 설계부터 잘못한 탓이 있을 수 있지만서도, 콜백함수에 대한 흐름을 따라가기가 쉽지 않다는 점도 있었습니다.

