---
description: 'C# Programing Language'
---

# C\# Programming Language

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

![&#xB9CE;&#xC740; Program paradigm &#xB3C4;&#xD45C;&#xC911; &#xD558;&#xB098;](../../../.gitbook/assets/image%20%28170%29.png)

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
  * **Reference : 데이터에 대한 참조\(주소\)가 저장됩니다.**
    * class, object, string, interface, delegate
      * 여기서 모든 데이터 타입은 Object type을 상속받는데 따로 나눠진 이유는 아래와 같습니다.
        * 모든 값형식이 상속받은 Object type과 Reference type의 Object type은 같습니다.
        * 그럼에도 불구하고, 값의 형식에 따라 2가지로 나눈 이유는 Value type이라는 Class와, Reference type이라는 Class로 상속받아 쓰기 때문입니다.
        * 그에 대한 설명은 아래의 단락으로 내려가시면 볼 수 있습니다.
      * 참조\(Reference\)란?
        * 한 객체가 다른 객체를 연결하거나 연결하는 수단 이라는 사전적 정의입니다.
        * C\#에서는 어떤 변수에 대한 작업이 연결된 다른 변수에 대해 영향을 미친다는 것을 의미하는데, 영향을 끼치기 위해서는 주소값을 보내야 합니다.
        * 즉, 참조한다는 뜻은 실제 원본 데이터가 저장된 메모리의 주소를 보내서 해당 주소의 값을 바꾼다는 것을 의미합니다.

결론?

* 최종적으로 이러한 데이터 타입의 변화에 따라 저장되는 메모리 영역이 다르기 때문에, 만약데이터 타입의 저장영역을 확실히 안다면, Runtime\(프로그램이 실행되고 있는 동안의 동작\)에서의 최적화는 물론, Runtime 이전에 메모리를 낭비를 막을 수 있습니다.

데이터 타입에 따른 분류는 아래의 그림을 보면 좀 더 편하게 이해하실 수 있습니다.
{% endhint %}

![Data type Hierarchy](../../../.gitbook/assets/image%20%28168%29.png)

* 각 언어마다 메모리에 대한 구조가 약간 상이할 수 있지만, 대부분은 다음과 같습니다.
* C\#에서의 메모리 구조는 아래와 같습니다.

![&#xCEF4;&#xD4E8;&#xD130; &#xBA54;&#xBAA8;&#xB9AC; &#xAD6C;&#xC870;](../../../.gitbook/assets/image%20%28169%29.png)

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

## 마치며

* 기본적으로 꼭 필요한 PL\(Programming Language\)에 개념과 메모리 구조에 관하여 살펴봤습니다.
* 아무리 써도 부족함을 느끼지만 다음 과정으로 넘어가기 위해 부족한 부분은 나중에 채우도록 하겠습니다.
* 다음 항목은 실제적인 C\#에서의 메모리 할당에 관하여 서술하겠습니다.

{% page-ref page="c-memory-assignment.md" %}



