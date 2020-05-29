---
description: 'C# Programing Language'
---

# C\# Programming Language - 작성중

## 무엇을 하려고 하는가?

* Programming Language와 그 속에 있는 C\#이라는 언어에 대한 고찰을 중점으로 적습니다.

## PL\(Programming Language\)이란?

* PL에 대해 정의하자면 아래와 같습니다.
  * 사람이 하려는 작업을 컴퓨터에게 전달하기 위한 표현법이라고 할 수 있습니다.
  * 기계, 사람 둘다 읽을 수 있는 형식으로 계산을 표현하기 위한 언어입니다.
  * **이를 사용하여 컴퓨터의 시스템을 구동시키는 소프트웨어\(프로그램\)을 작성하기 위한 형식언어 입니다.**
  * Programing Language는 많은 과정을 통해 성장해 왔기 때문에, 이를 정리하는 것은 쉬운일이 아니지고, 여기서 "A는 어떤 타입을 가진 언어이다"라고 하기에도 애매하기 때문에, 접근 하기 쉬울 정도의 설명을 가지고 문서를 작성하겠습니다.

{% hint style="info" %}
사람에게 전달하기 위한 언어?

* 사람이 번역하기 쉬울수록 고급언어에 속합니다. 
  * 컴퓨터 자체가 어떤 전자회로를 이용하여 빠르게 계산하기 위한 도구로써 정의 되었기 때문에, 시스템구조를 살펴보면 어떤 주소값과, 이진 데이터 값\(0, 1\)을 가진 형태를 띄고 있습니다.
  * 그렇기 때문에 **고급언어일수록 이진데이터를 가공하여 사람에게 접근하기 쉽게 하기 위해 컴파일러의 작업량이 굉장히 많아집니다.**
  * 컴파일러 작업량에 대한 비교는 Assembly Language와 우리가 사용하고 있는 C\#을 비교하면 쉽게 알 수 있습니다.

{% code title="NASM x86 Assembly Language를 사용한 Hello world 출력" %}
```text
adosseg
.model small
.stack 100h

.data
hello_message db 'Hello, World!',0dh,0ah,'$'

.code
main proc
      mov    ax, @data
      mov    ds, ax

      mov    ah, 9
      mov    dx, offset hello_message
      int    21h

      mov    ax, 4C00h
      int    21h
main endp
end main
```
{% endcode %}

{% code title="C\#을 이용한 Hello world 출력" %}
```csharp
using System;
class Helloworld {
    static void Main(string[] args) {
        Console.WriteLine("Hello, world!");
   }
}
```
{% endcode %}
{% endhint %}

## PL의 역할

* 궁극적으로 사람이 원하는 작업을 컴퓨터에게 시키기 위해 PL\(Programming Language\)이라는 표현 체계를 사용하여 원하는 작업을 하는 것입니다.
* 원하는 작업을 하기 위해서 기존의 기계어를 대채할 새로운 언어가 필요했고, 큰 숫자를 계산하기 위한 컴퓨터가, 점차 필요에 따라 발전해 나가면서, 다양한 분야에 쓰이게 됩니다.
  * 이에 대한 역할은 위의 설명 이외에 다양하게 생각할 수 있는데, 이렇게 따로 단락을 구분한 이유는 Programing Language에 따라 목적이 다르기 때문에 역할이 다르고, 스타일이 다르기 때문입니다.
  * 해당 내용은 Program paradigm의 도표를 통해 알 수 있습니다.

![&#xB9CE;&#xC740; Program paradigm &#xB3C4;&#xD45C;&#xC911; &#xD558;&#xB098;](../../.gitbook/assets/image%20%28170%29.png)

* 작성자가 사용하는 C\#은 multi-paradigmed에 속하는 C\#이기 때문에, 여러 paradigm에 대처하기 위해 주로 OOP\(Object - Oriented Programming\)을 비롯한 여러가지 문법을 사용하여 대응할 수 있습니다.

{% hint style="warning" %}
Program paradigm을 나눈 목적과 분류에 대한 설은 추후 추가될 예정입니다.
{% endhint %}

## Multi - paradigmed에 속한 C\#은?

* C\#은 마이크로소프트에서 개발한 .NET Framework를 이용하여 작성하는 언어입니다.

{% hint style="info" %}
.NET Framework이란?

* .NET Framework는 마이크로소프트에서 개발한 윈도우 프로그램 개발 및 실행 환경입니다.
* 마이크로소프트에서 제작했기 때문에, OS가 다르다면 프로그램이 동작하지 않습니다.
  * Linux에서 동작하는 동일한 프로그램을 따로 제작해야 합니다.
* Network, Inferface등 많은 작업을 캡슐화\(Encapsulation\)했고, CLR\(Common - Language - Runtime\)이라는 가상머신 위에서 작동합니다.
* 하드웨어에서 사용자가 프로그램을 사용하기 까지의 컴퓨터가 거치는 과정은 다음과 같습니다.

**H/W -&gt; Bare machine -&gt; MacroInstruction Interpreter -&gt; OS -&gt; Virtual Complier -&gt; User**

자세한 내용은 아래의 링크에 있습니다.
{% endhint %}

{% embed url="https://www.radford.edu/~nokie/classes/380/Chap1-Lang-Impl.html" %}

* C\#은 Multi - paradigmed의 스타일을 가지고 있기 때문에, Window, Web, Game, Mobile Programming등 거의 모든 영역에서 사용되는 범용 언어입니다.
* **C, C++, Java의 장점을 모아서 제작했기 때문에 어느정도 유사성을 띄고 있습니다.**

## C\#에 대해서 공부하기 전에 살펴보는 메모리 구조

* 해당 단락에서는 Unity를 사용한 C\#의 메모리 구조에서 관해서 설명하려고 합니다.
* 그 전에 왜 메모리 구조를 왜 살펴봐야 하는지에 대해 설명하려고 합니다.

{% hint style="info" %}
메모리\(Memory\)란?

* 이진데이터로 이루어진 데이터를 저장하기 위해서 전자회로를 이용하여 내가 쓴 데이터를 임의의 영역에 저장하고 읽을 수 있는 장치입니다.

이런 메모리\(Memory\)구조를 왜 살펴야 하는가?

* 데이터의 성질에 따라 할당되는 메모리\(Memory\)의 영역이 다르기 때문입니다.

메모리\(Memory\) 구조를 살펴서 얻는 것은 무엇인가?

* C\#은 고급언어에 속하기 때문에 데이터의 성질을 변화 할 수 있습니다.
  * ex\) int, float, double, class, interface 등등...
* C\#에서는 모든 데이터 타입은 Object type에서 상속 받습니다.
* Object type은 값의 형식에 따라 아래와 같이 나눠집니다.
  * **Value : 해당 데이터가 직접 저장됩니다.**
    * int, unsigned, float, double, char, struct, bool, byte, enum, long, short
  * **Reference : 데이터에 대한 참조가 저장됩니다.**
    * class, object, string, interface, delegate
      * 여기서 모든 데이터 타입은 Object type을 상속받는데 따로 나눠진 이유는 아래와 같습니다.
        * 모든 값형식이 상속받은 Object type과 Reference type의 Object type은 같습니다.
        * 그럼에도 불구하고, 값의 형식에 따라 2가지로 나눈 이유는 Value type이라는 Class와, Reference type이라는 Class로 상속받아 쓰기 때문입니다.
        * 그에 대한 설명은 아래의 단락으로 내려가시면 볼 수 있습니다.
      * 참조\(Reference\)란?
        * 한 객체가 다른 객체를 연결하거나 연결하는 수단 이라는 사전적 정의입니다.
        * C\#에서는 어떤 변수에 대한 작업이 연결된 다른 변수에 대해 영향을 미친다는 것을 의미합니다.

결론?

* 최종적으로 이러한 데이터 타입의 변화에 따라 저장되는 메모리 영역이 다르기 때문에, 만약데이터 타입의 저장영역을 확실히 안다면, Runtime\(프로그램이 실행되고 있는 동안의 동작\)에서의 최적화는 물론, Runtime 이전에 메모리를 낭비를 막을 수 있습니다.

데이터 타입에 따른 분류는 아래의 그림을 보면 좀 더 편하게 이해하실 수 있습니다.
{% endhint %}

![Data type Hierarchy](../../.gitbook/assets/image%20%28168%29.png)

* 각 언어마다 메모리에 대한 구조가 약간 상이할 수 있지만, 대부분은 다음과 같습니다.
* C\#에서의 메모리 구조는 아래와 같습니다.

![&#xCEF4;&#xD4E8;&#xD130; &#xBA54;&#xBAA8;&#xB9AC; &#xAD6C;&#xC870;](../../.gitbook/assets/image%20%28169%29.png)

* 코드 영역 : 프로그래머가 쓴 Code에 대한 저장 영역입니다.
* 데이터 영역 : 프로그래머가 미리 선언한 전역, 정적변수에 대한 저장 영역입니다.
  * ex\) static, const
* 힙\(Heap\) : new를 통해 사용자가 직접 생성하기 때문에 Reference type의 데이터 타입이 저장됩니다.
* 스택\(Stack\) : 해당 영역에는 프로그래머가 선언한 Value type의 데이터 타입과,  사용자가 동적으로 할당된 Heap에 저장된 객체의 주소가 저장됩니다.

프로그래머가 Code를 작성하여 실행되면 주로 접근하는 두 곳의 메모리 영역\(Stack, Heap\)은 아래와 같은 유형으로 저장이 됩니다.

1. Value type
2. Reference type
3. Pointer
4. Instruction

## 메모리 구조에 따른 데이터의 전달 방식

* 메모리 구조에 대해 어느정도 인지했다면, 우리가 사용하는 Visual Studio를 통해 코드를 작성하면 어떻게 컴퓨터는 값을 저장하고 읽어오는가에 대한 의문이 들 수 있습니다.
* 이에 대한 내용은 해당 문서에서 발췌하여 정리한 글입니다.

{% embed url="https://www.c-sharpcorner.com/article/C-Sharp-heaping-vs-stacking-in-net-part-i/" %}

* C\#에서 **Code가 실행될 때** .NET Framework는 두 곳의 메모리 영역\(Heap, Stack\)에 저장합니다.
  * Code가 실행되기 전에 데이터\(코드 영역, 데이터 영역\)들은 실행전에 할당이 됩니다.
* 그렇다면 두 곳의 메모리 영역의 차이는 무엇인가? 하면 아래와 같습니다.
  * Stack
    * 자체적으로 메모리 관리가 되기 때문에 필요없는 데이터 영역을 알아서 처리합니다.
    * Code에서의 데이터 영역의 값들을 실행하면 알아서 추적합니다.
  * Heap
    * 메모리 관리를 GC\(Garbage Collector\)에 의해 관리됩니다. 그렇기 때문에 더이상 쓰지 않는 데이터를 처리하기 위해 비용을 많이 소모합니다.
    * 작성자가 생성한 객체들을 추적합니다.



* 이러한 메모리 영역에 어떻게 저장이 되는가?
* 일단 Stack 영역에서의 데이터 저장 과정을 설명하겠습니다.

### Stack에서의 데이터 저장 과

```csharp
public int AddFive(int pValue)  
{  
      int result;  
      result = pValue + 5;  
      return result;  
} 
```

* 위와 같은 Code가 있다고 한다면 맨 처음 Stack에는 아래의 그림과 같이 됩니다.

![](../../.gitbook/assets/image%20%28175%29.png)

* 아직 함수 속 Code가 실행되지 않았기에 위와 같은 그림이 됩니다.
* Code가 실행된다면 아래의 그림과 같습니다.

![](../../.gitbook/assets/image%20%28177%29.png)

* return result; 부분까지 실행한 그림입니다.
* 이제 함수가 실행을 완료하고 그 결과를 반환하는데 아래의 그림과 같습니다.

![](../../.gitbook/assets/image%20%28174%29.png)

* Stack은 자체적으로 메모리가 관리되기 때문에 실행을 완료한다면 아래의 그림과 같이 Stack에서 Delete합니다.

![](../../.gitbook/assets/image%20%28172%29.png)







### Reference type 유형의 Class를 사용하여 Value type을 선언한다면?

* C\#은 Multi - paradigmed의 스타일을 가졌기 때문에 여러 스타일에 대응이 되기 위해서 Reference type의 Class, 혹은 Array를 선언하여 하는 경우가 있습니다.
* 즉, Stack에 저장되어야 할 Value type이 때때로 Heap에 저장이 되는 경우를 가집니다.

```csharp
public class MyInt  
{            
   public int MyValue;  
}

public MyInt AddFive(int pValue)  
{  
      MyInt result = new MyInt();  
      result.MyValue = pValue + 5;  
      return result;  
} 
```

* 위와 같은 Code를 가진 경우 Reference type인 Class와 Value type의 함수가 선언되고, 함수 안에서 Class 객체를 생성하여 Field에 접근하는 Code입니다.

![](../../.gitbook/assets/image%20%28171%29.png)

* Stack에서는 AddFive\(\) 함수의 데이터와 parameter인 int type pValue 데이터가 생성됩니다.
* 이 함수가 실행되면 MyInt Class의 Instance가 생성되기 때문에 Heap 영역에서의 int type MyValue가 생성이 됩니다.
* 내용에 대한 그림은 아래와 같습니다.

![](../../.gitbook/assets/image%20%28178%29.png)

* 이제 사용이 끝났기 때문에 Stack에서의 메모리 관리에 의해 Stack의 내용을 사라지게 됩니다.
* 아래의 그림과 같습니다.

![](../../.gitbook/assets/image%20%28173%29.png)

* 최종적으로 프로그램이 완전히 종료 될 때까지 메모리 영역에 다음과 같은 데이터가 잔류하게 됩니다.
* 아래의 그림과 같습니다.

![](../../.gitbook/assets/image%20%28176%29.png)

### 같은 Class를 다른 객체로 선언했을 때

* 곧바로 아래와 같은 Code를 살펴보겠습니다.

```csharp
public class MyInt  
{  
       public int MyValue;  
}  

public int ReturnValue2()  
{  
      MyInt x = new MyInt();  
      x.MyValue = 3;  
      MyInt y = new MyInt();  
      y = x;                   
      y.MyValue = 4;                
      return x.MyValue;  
}  
```

* MyInt x = new MyInt\(\); / MyInt y = new MyInt\(\);
* 두 문장은 얼핏보면 다른 객체를 생성하는 것 같지만 실상은, 아래의 그림과 같습니다.

![](../../.gitbook/assets/image%20%28181%29.png)

* 각각 Pointer 형식 x, y라는 주소가 같은 객체에 접근하기 때문에, 위의 Code를 출력하면 결국 값은 변하지 않고 4를 출력하게 됩니다.





### Passing Value types

#### Stack에서의 메모리 할당

* 기본적인 함수 호출시 발생하는 변수와 Class의 메모리 할당에 관하여 알아봤다면 좀 더 자세히 살펴보는 단원입니다.

{% hint style="info" %}
작성한 Code는 데이터로 환산되어 메모리에 할당합니다. 메모리에 할당한 데이터들의 중복을 막기 위해 우리는 함수를 사용하여 중복을 회피할 수 있습니다.

이때 함수에서 parameter를 전달하는 과정에서 대용량의 데이터를 효율적으로 사용하기 위해 값의 유형에 따라 2가지로 분류되었습니다.

* Call by Value or Passing Value type
  * 함수 호출을 통해 실제 메모리에 할당된 주소의 값을 변경할 수 없습니다.
* Call by Reference or Passing Reference type
  * 함수 호출을 통해 실제 메모리에 할당된 주소의 값을 변경합니다.
{% endhint %}

```csharp
class Class1 {
    public void Go() {
        int x = 5;
        AddFive(x);
        Console.WriteLine(x.ToString());
    }
    public int AddFive(int pValue) {
        pValue += 5;
        return pValue;
    }
}
```

* 위와 같은 Code가 존재할 때 Stack은 다음 그림과 같이 존재하게 됩니다.

![](../../.gitbook/assets/image%20%28189%29.png)

* 다음으로 AddFive함수와 parameter의 데이터가 메모리에 할당되고, **int x는 AddFive의 parameter에 복사됩니다.**

![](../../.gitbook/assets/image%20%28187%29.png)

* AddFive\(\) 함수의 실행이 종료된다면, 다시 Go\(\) 함수로 전달이 되고 사용이 끝난 데이터들은 제거됩니다.

![](../../.gitbook/assets/image%20%28190%29.png)

#### Stack과 Heap을 사용하는 코드에 대한 메모리 할

* 여기서 가장 중요한 부분은 parameter의 데이터가 메모리에 할당되고 다른 함수에서 호출이 된다면, 원본과 복사본이 존재한다는 것입니다.
* 만약 대용량의 데이터를 Passing Value type으로 parameter를 전달했다면, 매번 복사하고 공간을 할당하는 면에서 굉장히 비용이 비싸집니다.
* 구조체에 관한 Stack의 할당 과정에서 위의 내용을 증명할 수 있습니다.

```csharp
public struct MyStruct   {  
    long a, b, c, d, e, f, g, h, i, j, k, l, m;  
}  
public void Go() {
    MyStruct x = new MyStruct();
    DoSomething(x);
}
public void DoSomething(MyStruct pValue) { 
    // DO SOMETHING HERE....
}
```

* 위와 같은 Code가 존재할 때 Stack에서는 메모리를 아래와 같이 할당합니다.

![](../../.gitbook/assets/image%20%28182%29.png)

* 실제로 위의 그림과 같이 복사본 자체가 용량이 커지기 때문에 특정 상황에서는 굉장히 비효율적일 수 있습니다.
* 해당 문제를 해결하기 위해 Passing Reference type or Call by Reference와 같은 방법으로 parameter를 전달합니다.

```csharp
public void Go() {
    MyStruct x = new MyStruct();
    DoSomething(ref x);
}
 public struct MyStruct  {  
     long a, b, c, d, e, f, g, h, i, j, k, l, m;  
 }  
public void DoSomething(ref MyStruct pValue) { 
    // DO SOMETHING HERE....
}
```

* C\#에서는 C, C++에서 사용하는 Pointer type을 사용하지 않습니다.
* 대신에 ref, out이라는 키워드를 통하여 pointer의 역할을 대체합니다.
* 해당 Code는 위에서 사용한 구조체 관련 Code와 유사하지만, new, ref 키워드를 사용하여 동적 할당 및 참조를 통해 프로그램을 실행하고 있습니다.
  * Pointer를 통해 이미 할당되어 있는 메모리의 주소를 가지게 하여 구조체에 접근합니다.
  * 해당 Code에 관한 메모리 할당은 아래의 그림과 같습니다.

![](../../.gitbook/assets/image%20%28180%29.png)







### Passing Reference types

위 단락에서는 Stack에서의 Value types의 메모리 할당을 살펴봤다면 이번 단락에서는 Reference types의 메모리 할당에 대하여 살펴보겠습니다.

우선 간단한 예제부터 살펴보자면 아래와 같습니다.

```csharp
public class MyInt {  
    public int MyValue;  
}  

public void Go() {  
   MyInt x = new MyInt();  
}
```

* 위와 같은 코드는 아래의 그림과 같은 메모리 할당을 가지고 있습니다.

![](../../.gitbook/assets/image%20%28188%29.png)

* 위의 코드에서 Go\(\) 함수의 동작을 약간 변형하면 아래와 같습니다.

```csharp
public class MyInt {  
    public int MyValue;  
}  
public void Go() {  
   MyInt x = new MyInt();  
   x.MyValue = 2;  
   DoSomething(x);  
   Console.WriteLine(x.MyValue.ToString());  
}  
public void DoSomething(MyInt pValue) {  
     pValue.MyValue = 12345;  
}  
```

* 위의 코드에서 실행되는 메모리 할당은 아래와 같습니다.

![](../../.gitbook/assets/image%20%28186%29.png)

* 그리고 메모리 할당의 순서는 다음과 같습니다.

1. Go\(\) 함수의 메모리 할당이 일어납니다.
2. 함수 내부에서 MyInt Class의 동적할당이 일어났기 때문에 Heap에서의 MyInt Class 메모리 할당이 일어납니다.
3. Stack 내부적으로도 Heap에서 할당된 MyInt Class 변수인 x의 메모리를 할당합니다.
4. DoSomething\(x\) 함수를 실행함과 동시에 메모리에 할당합니다.
5. DoSomething은 MyInt Class parameter를 가지고 있기 때문에, 이 parameter에 대한 메모리를 할당합니다.
6. MyInt Class의 멤버 변수인 MyValue에 접근하여 값을 할당하고 할당한 값은 Heap에서의 MyInt 메모리의 멤버변수에 할당합니다.
7. 마지막으로 Stack의 MyInt Class를 동적 할당한 메모리를 가리키고 있는 x라는 변수의 멤버변수를 ToString\(\)을 통해 string으로 변하여 출력합니다.

이번엔 상속된 Class 메모리 할당과 할당된 메모리의 값을 변경하는 과정에서의 Stack, Heap 동작을 살펴 보겠습니다.

```csharp
public class Thing  {  }  
public class Animal:Thing  {  public int Weight;  }  
public class Vegetable:Thing  {  public int Length;  }  

public void Go()   {  
   Thing x = new Animal();  
   Switcharoo(ref x);  
    Console.WriteLine("x is Animal    :   "  + (x is Animal).ToString());  
    Console.WriteLine("x is Vegetable :   "  + (x is Vegetable).ToString());  
}  

public void Switcharoo(ref Thing pValue)  {  
     pValue = new Vegetable();  
}  
```

* 출력된 결과는 아래와 같습니다.
  * x is Animal : False
  * x is Vegetable : True
* Thing의 자식 Class인 Animal Class를 동적할당하여 Thing Class 변수인 x에 할당하고 있습니다.
* 다음으로 Switcharoo\(\) 함수를 통하여 parameter인 Thing Class 변수 pValue를 ref 키워드를 사용하여 실제 메모리에 할당된 값을 변경합니다.
* 위 코드에서의 메모리는 아래와 같은 그림을 가지게 됩니다.

![](../../.gitbook/assets/image%20%28185%29.png)

* 위의 그림은 Switcharoo\(\) 함수의 메모리 할당까지의 그림입니다.
* 함수가 실행된 이후 출력하면 아래와 같은 그림의 메모리 할당을 가지게 됩니다.

![](../../.gitbook/assets/image%20%28179%29.png)



