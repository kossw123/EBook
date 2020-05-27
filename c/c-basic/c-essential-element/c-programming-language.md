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

![&#xB9CE;&#xC740; Program paradigm &#xB3C4;&#xD45C;&#xC911; &#xD558;&#xB098;](../../../.gitbook/assets/image%20%28168%29.png)

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

H/W -&gt; Bare machine -&gt; MacroInstruction Interpreter -&gt; OS -&gt; Virtual Complier -&gt; User

자세한 내용은 아래의 링크에 있습니다.
{% endhint %}

{% embed url="https://www.radford.edu/~nokie/classes/380/Chap1-Lang-Impl.html" %}



* C\#은 Multi - paradigmed의 스타일을 가지고 있기 때문에, Window, Web, Game, Mobile Programming등 거의 모든 영역에서 사용되는 범용 언어입니다.
* 작성자는 Unity를 사용하기 때문에 객체지향의 개념을 사용합니다.
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
{% endhint %}

* C\#에서의 메모리 구조는 아래와 

