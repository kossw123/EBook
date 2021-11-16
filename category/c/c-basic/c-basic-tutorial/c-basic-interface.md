---
description: C# Basic interface
---

# C# Basic interface

## 무엇을 하려고 하는가?

* C#의 주요기능중 하나인 interface에 대한 개념정리를 기술했습니다.



## interface란?

* Class와 비슷하게 함수, 속성, 이벤트, 인덱서를 갖지만 "직접 구현하진 않고" 정의만 선언한 부분을 이야기 합니다.
* 공통적인 기능에 대해 단일 구현을 제공하기 위해, interface 내부에 아래와 같이 static을 사용할수 있습니다.
  * ex) static string Name { get;  set; }
* 기본적으로 interface 내부에는 정의만 쓰기 때문에, Syntax를 사용할 수 없지만 C# 8.0부터는 사용이 가능하게 됐습니다.
  * 위의 예시와 같이 int Num _**{ get;  set; }**_** **
* **그리고 재정의를 구현하지 않는 몇몇의 Class 및 Struct에 대한 기본적인 구현을 허용하게 됐습니다.**

## interface 사

* **interface keyword를 통해 선언**하고 그 안에 **내용을 정의 후** 사용하고 싶은 **class 및 객체에 상속하여 사용**합니다.

```csharp
interface ISampleInterface
{
    void SampleMethod();
}

class ImplementationClass : ISampleInterface
{
    // Explicit interface member implementation:
    void ISampleInterface.SampleMethod()
    {
        // Method implementation.
        Console.WriteLine("ISampleInterface.SampleMethod()");    
    }

    static void Main()
    {
        // Declare an interface instance.
        ISampleInterface obj = new ImplementationClass();

        // Call the member.
        obj.SampleMethod();
    }
}
```

* 대형 프로젝트를 할 때, 꼭 구현해야 할 값이 필요하면 interface를 사용하여 구현하도록 합니다.

## 명시적 interface 사용

* 어떤 interface를 상속받는 Class가 있다고 존재한다면, 기존 interface의 목적과는 다른 interface를 사용할 수 있습니다.
* 이러한 interface의 다중상속을 할 때, **interface에 구현된 함수를 재정의를 통해 내용을 추가하는 경우** 명시적 interface를 사용합니다.

아래의 예시를 통해 명시적 인터페이스 구현에 대해 설명하겠습니다.

### Signature가 동일한 함수를 가진 interface를 상속받은 Class가 해당 interface를 구현 할 때

```csharp
public interface IControl
{
    void Paint();
}
public interface ISurface
{
    void Paint();
}
public class SampleClass : IControl, ISurface
{
    // Both ISurface.Paint and IControl.Paint call this method.
    public void Paint()
    {
        Console.WriteLine("Paint method in SampleClass");
    }
}

static void Main(string[] args)
{
    SampleClass sample = new SampleClass();
    IControl control = sample;
    ISurface surface = sample;
    
    // The following lines all call the same method.
    sample.Paint();
    control.Paint();
    surface.Paint();
    // Output:
    // Paint method in SampleClass
    // Paint method in SampleClass
    // Paint method in SampleClass
}
```

* 처음에는 interface를 생성하고 하나의 Class에 모두 상속한 뒤, Main() 함수에서 Class에 대한 객체를 동적으로 생성합니다.
* 다음으로 **"각 인터페이스에 대한 객체를 선언"**하고 Class를 가리키게 하여, interface에서 중복된 이름으로 생성한 함수를 사용할 수 있습니다.
  * \*\*\* 동적 생성이 아닌 선언입니다.

### 상속받은 interface가 동일한 기능이 아닐 때

* 상속받은 interface가 동일한 기능이 아닐 때, interface 둘 중 하나 혹은 둘 다 잘못 구현할 수 있습니다.
* 이를 방지하기 위해 아래의 예시처럼 **Class 내부에서 해당 interface의 멤버를 명시적으로 표시합니다.**

```csharp
public interface IControl
{
    void Paint();
    int P { get; }
}
public interface ISurface
{
    void Paint();
    int P();
}
public class SampleClass : IControl, ISurface
{
    // 명시적 인터페이스 선언
    void IControl.Paint()
    {
        System.Console.WriteLine("IControl.Paint");
    }
    // 명시적 인터페이스 선언
    int IControl.P { get { return 0; } }
    
    // 명시적 인터페이스 선언
    void ISurface.Paint()
    {
        System.Console.WriteLine("ISurface.Paint");
    }
    // 명시적 인터페이스 선언
    int ISurface.P() { return 0; }
}

static void Main(string[] args)
{
    // Call the Paint methods from Main.
    SampleClass obj = new SampleClass();
    //obj.Paint();  // Compiler error.
    
    IControl c = obj;
    c.Paint();  // Calls IControl.Paint on SampleClass.
    
    ISurface s = obj;
    s.Paint(); // Calls ISurface.Paint on SampleClass.
    
    // Output:
    // IControl.Paint
    // ISurface.Paint
}
```

* 위의 예시와 같은 경우 Class 내부에서 명시적 인터페이스를 통해 접근하지만, Main() 함수에서의 사용은 interface 객체를 선언하고 Class를 가리키게 해서 Paint() 함수에 접근하고 있습니다.
* 그리고 Class 내부에서 interface에 대한 같은 Signiture를 가진 멤버에 대해 명시적으로 선언하여 확실하게 구현하고 있습니다.

### 선언한 interface의 멤버에 대한 구현

* C# 8.0부터 interface에 선언한 멤버를 구현할 수 있게 되었습니다.
* 해당 경우에 대한 예시는 아래와 같습니다.

```csharp
public interface IControl
{
    void Paint() => Console.WriteLine("Default Paint method");
}
public class SampleClass : IControl
{
    // Paint() is inherited from IControl.
}

static void Main(string[] args)
{
    var sample = new SampleClass();
    //sample.Paint();// "Paint" isn't accessible.
    var control = sample as IControl;
    control.Paint();
}
```

* 위의 예시처럼 interface에 대한 짧은 내용은 lambda expression을 통해 선언이 가능합니다.
