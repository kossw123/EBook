---
description: C# Basic delegate
---

# C# Basic delegate

## 무엇을 하려고 하는가?

* C,C++의 함수 포인터처럼 함수를 안전히 캡슐화하는 대리자(delegate)의 개념과 사용방법 및 사례를 소개합니다.



## 대리자(delegate)란?

* Delegate sealed Class에서 파생되었으며, instance화 된 delegate는 매개변수로 전달하거나, 속성에 할당할 수 있습니다.
  * **이로 인해 어떤 함수에서 나중에 delegate를 호출하여 비동기 콜백이라는 방식을 사용할 수 있습니다.**

{% hint style="info" %}
비동기 콜백(Asynchronous callback), 동기 콜백(Synchronous callback)

delegate를 사용하는 가장 큰 이유는 비동기적인 콜백함수 호출에 있기 때문에 동기, 비동기에 관한 개념부터 잡아보려고 합니다.

* 동기적 처리
  * 시간의 흐름에 따라 순서대로 코드가 처리되는 방식
* 비동기적 처리
  * 시간에 흐름에 따라 순서대로 코드가 실행되지만, 리턴을 기다리지 않는 처리 방식
  * 이로 인해 프로세스는 요청에 따라 실행되지만, 프로세스가 끝나는 시간에 따라 다르게 리턴을 받습니다.

위의 개념과 같이 작성자가 따로 비동기적인 처리를 하지 않는 이상 코드는 무조건 동기적 처리가 됩니다. 하지만 비동기적 처리를 이용해서 코드를 작성한다면,&#x20;

우리가 사용하고 있는 앱이나 프로그램에서 새로고침 버튼을 누른다면, ui는 그대로 있고 내용만 새로 고쳐지는 현상을 재현할 수 있게 됩니다.
{% endhint %}

* 안전하게 함수를 캡슐화 하는 문법입니다. 이런 캡슐화를 통해 얻을 수 있는 이점은 아래와 같습니다.
  * 개체 지향이므로 delegate를 통해 재사용성이 뛰어납니다.
  * 형식이 안전합니다.
  * 보안이 유지됩니다.

이런 delegate는 일반적으로 원형을 제공하거나, 익명 함수를 통해 사용하여 생성합니다.

{% content-ref url="c-basic-anonymous-functions/" %}
[c-basic-anonymous-functions](c-basic-anonymous-functions/)
{% endcontent-ref %}

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

## delegate의 여러가지 구

해당 단락에서는 delegate는 어떻게 구현하는 사례가 있는가에 대한 내용을 기술하려고 합니다.

### parameter로 넘기기

```csharp
using System;
namespace ConsoleApp {
    class Program { 
        // 함수를 등록할 delegate 선언
        public delegate void myDelegate(string str);
        static void Main(string[] args) {
            myDelegate del = TargetMethod;
            MethodWithCallback(1, 2, del);
            /*
                Output : Sum : 3
            */
        }
        // 등록할 함수
        void TargetMethod(string str) => Console.WriteLine(str);
        // 함수의 parameter로 myDelegate를 할당
        public static void MethodWithCallback
        // parameter에 등록한 myDelegate로 등록된 함수를 실행
        (int a, int b, myDelegate callback) => callback($ "Sum : {(a + b).ToString()}");
    }
}
```

위의 예시는 parameter로 delegate를 넘겨서 해당 함수를 호출하는 콜백함수의 형태를 띄고 있습니다. 이러한 형태로 MethodWithCallback() 함수안에서 myDelegate에 등록된 함수도 실행시키고, 따로 기능을 추가할 수 있게 됩니다.&#x20;

{% hint style="danger" %}
이러한 대리자를 통해 일정 형식만 유지하면서, 보안 및 재사용성을 챙길 수 있는 이점이 있습니다. 하지만 이러한 이점에 저는 아래와 같은 불편함을 느꼈습니다.

* 재사용성이 있기 때문에, 콜백함수의 오류에 빠져 데이터의 종속성이 생기는 점이 있습니다.

이 부분은 저의 설계부터 잘못한 탓이 있을 수 있지만서도, 콜백함수에 대한 흐름을 따라가기가 쉽지 않다는 점도 있었습니다.
{% endhint %}

###

### Wrapping

* delegate는 instance Method를 Wrapping하기 위한 방법으로도 사용됩니다.
* 이러한 Wrapping을 거친다면 delegate는 등록한 함수에 추가로 다른 함수도 등록할 수 있게 됩니다.
  * \-= 연산자를 통해 등록을 해제 할 수도 있습니다.
* 아래와 같은 예시를 보면 수월하게 이해하실 수 있습니다.

```csharp
using System;

public class MethodClass
{ 
    public void Method1(string message)
    {
        Console.WriteLine(message);
    }
    public void Method2(string message)
    {
        Console.WriteLine(message);
    }
}

static void Main(string[] args)
{
    var obj = new MethodClass();
    Del d1 = obj.Method1;
    Del d2 = obj.Method2;
    Del d3 = DelegateMethod;

    // Del delegate를 allDelegate로 Wrapping
    Del allDelegate = d1 + d2;
    allDelegate += d3;
    DelegateMethodCallback("hello", allDelegate);
}

public static void DelegateMethodCallback(string str, Del callback)
{
    callback(str);
}
```

allDelegate를 통해 Del delegate의 객체를 하나로 Wrapping하여 콜백함수를 통해 호출하는 코드입니다. allDelegate() 함수를 호출하는 순간 순서대로 등록한 d1, d2, d3가 그대로 호출됩니다.

* 함수 중 하나라도 catch를 통해 예외처리한 이외의 Exception이 throw된다면, 해당 Exception이 대리자의 호출자에게 해당 예외가 전달되어 호출목록의 후속 함수는 실행되지 않습니다.

{% hint style="info" %}
!! instance Method

static method와 달리 객체를 생성해야만 사용가능한 함수입니다.

```csharp
class instance {
    void Method() { }
}

static void Main(string[] args)
{
    instance ins = new instance();
    ins.Method();
}
```

!!Wrapping

데이터를 감싸는 방법, 또는 디자인패턴이라고도 하는 것 같습니다.

이렇게 애매하게 설명한 이유는 기본적으로 데이터를 감싼다는 객체 지향의 개념과 유사하기 때문에 따로 책에서도 설명하지 않고, 굳이 찾으려고도 하지 않는 것 같습니다.

중요한 것을 생각한대로 풀자면 **"원본 데이터를 훼손하지 않기 위해, 또 다른 데이터를 가지고 접근, 수정하는 방법" **정도로 해설하면 될 것 같습니다.
{% endhint %}



### delegate의 비교

* 컴파일 시간에 할당된 delegate를 비교한다면 오류가 발생합니다.
* 하지만 static delegate형식이면 비교는 허용되나, 런타임에서 Exception이 발생합니다.

```csharp
void method(Delegate1 d, Delegate2 e, System.Delegate f)
{
    Console.WriteLine(d == e);            // CS0019 : 오류 
    Console.WriteLine(d == f);            // CS0252 : 경
}
```



### delegate의 instance

* C# 2.0부터는 delegate를 인스턴스화 시킬 수 있습니다.
* 하지만 이러한 방법은 무명함수(Anonymous Method)를 이용하는 방법 이전에 나왔기 때문에, 잘 쓰이진 않습니다.

```csharp
// Declare a delegate.
delegate void Del(string str);

// Declare a method with the same signature as the delegate.
static void Notify(string name)
{
    Console.WriteLine($"Notification received for: {name}");
}
static void Main(string[] args)
{
    // Create an instance of the delegate.
    Del del1 = new Del(Notify);
}

```



### Multicast deletgate

* multicast delegate는 차후 서술할 Event처리에 광범위하게 사용됩니다.
* 간단하게 Event에서 delegate가 어떻게 사용되는가를 설명하자면 아래와 같습니다.

1. Event 객체는 해당 Event를 받도록 등록한 개체에 이벤트가 발생하였음을 알립니다.
2. 알림을 받은 개체는 Event가 처리되도록 설계된 함수를 작성하고 delegate를 통해 Event 객체에게 전달합니다.
3. delegate에 등록한 함수는 Event가 발생한다면 delegate를 통해 호출됩니다.
4. delegate가 Event를 등록한 개체에게 데이터를 전달합니다.

해당 단락은 Event page를 통해 연계하여 기술하겠습니다.



