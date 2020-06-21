---
description: 'C# Basic Class'
---

# C\# Basic Class

## 무엇을 하려고 하는가?

* Multiple paradigms을 가진 C\#은 C++와 Java의 이점들을 가져와 혼합한 언어로써, 그 중 Class라는 기능이 있습니다.
* Class라는 기능에 대한 개념과, 간단한 예시 및 객체들간 통신에 대해 알아보려고 합니다.



## Class란?

* 값 형식 중에서 Reference type에 분류되어 있습니다. 물론, 작성자가 직접 작성한 Class와 기존에 사용하는 Exception이나, Object, String 등등과 같은 값 형식들을 구성하는 기능 중 하나가 Class입니다.
* Reference type으로 분류된 Class는 C\#에서는 동적으로 생성하여 사용합니다.
  * ex\) SampleClass sample = new SampleClass\(\);
* 제가 생각하는 이러한 Class의 구성요소는 아래와 같습니다.
  * \*\*\* MSDN에서는 상태\(Field\)와 작업\(Method\)를 하나의 단위로 결합한 데이터 구조라고 설명하고 있습니다.
  * 하지만 제가 생각한 구성요소는 "어떤것으로 이루어 졌는가"가 아닌 "데이터 구조 내부를 구성하는 요소 어떻게 배치 되어 있는가"에 더 중점이 되어 이야기 하고 있으니 참고해 주시면 감사하겠습니다.
  * MSDN에서의 클래스 정의에 대한 링크는 아래와 같습니다.

{% embed url="https://docs.microsoft.com/ko-kr/dotnet/csharp/tour-of-csharp/classes-and-objects" %}

{% hint style="info" %}
Class 구성요소 

1. Constructor
2. Destructor
3. Field\(Method, Variable, Constants....\)
{% endhint %}

![](../../../.gitbook/assets/image%20%28209%29.png)



* 구성요소 중 Constructor에 대해서 먼저 얘기해보겠습니다.
  * Constructor\(생성\)
    * Class를 선언하고 사용한다고 선언한 순간\(new\)부터 맨 처음으로 자동 호출됩니다.
    * 굳이 명시하지 않아도 자동으로 생성되고 호출되기 때문에, 따로 선언할 필요가 있다면 Class내부 Field에서 어떤 Field에 존재하는 변수들을 초기화 하는데 사용될 것입니다.
    * Constructor는 ClassName과 똑같은 함수를 Class 내부에 선언하면 명시적으로 사용할 수 있는데 여기서 작성자는 여러가지 변수들을 초기화합니다.
    * 아래와 같은 예시로 생성자를 선언하고 내부에서 Field에 선언된 변수를 초기화 한다면 Main함수에서는 동적으로 생성한다는 문장과 동시에 parameter로 넘기게 되면
    * Class의 동적 생성과 동시에 호출되는 Constructor로 인해 int a는 자동적으로 param에서 선언한 데이터가 들어가게 됩니다.

```csharp
class SampleClass
{
    int a;
    public SampleClass(int param)
    {
        a = param;
    }
}

class Program
{
    static void Main(string[] args)
    {
        SampleClass sample = new SampleClass(1);
    }
}
```



* Field
  * Field를 구성하는 데이터는 변수, 함수, 인덱서, 이벤트 등등 여러가지 데이터 형식이 들어갈 수 있습니다.
    * Class 내부의 Field의 변수를 "멤버변수"라고도 합니다.
    * 응용하여 Class 내부의 Field에 존재하는 함수를 "멤버함수"라고도 합니다.

```csharp
interface IVariable { void Write(); }

class SampleClass : IVariable {
    public SampleClass(int param) {
        a = param;
        Console.WriteLine("생성자");
    }
    #region Field 부분에서 쓸수 있는 데이터 
    int a; double b;
    float c; string d; Index f;
    
    public delegate void Publisher();
    public event Publisher MyEvent;      //이벤트 정의
    public void sampleMethod() {
        Console.WriteLine("Field에 존재하는 Method");
    }
    void IVariable.Write() {
        Console.WriteLine("IVariable interface의 Write() 함수");
    }
    #endregion

    ~SampleClass() {
        Console.WriteLine("소멸자")
    }
}
```



* Destructor\(소멸\)
  * GC\(garbage Collector\)의 판단에 의해 사용한 객체, 즉 동적으로 생성한 Class의 사용이 끝났다고 판단하면 자동으로 호출되는 함수입니다.
  * 생성자와 달리 따로 오버로딩 할수가 없고, 사용자가 원하는 때에 호출할 수 없습니다.



## Class간의 통신

* Class는 속과 상태가 결합된 하나의 데이터 구조입니다.
* Class를 사용하려면 동적으로 생성하여 Heap 영역에 할당하여 프로그램이 종료될 때 까지 살아있습니다.
* "그렇다면 이런 데이터 구조는 어떻게 사용하는 것이 가장 좋은가?"에 대한 이야기를 하려고 합니다.

C\#은 Java와 C++의 장점을 도입하여 Multiple paradigm을 가진 PL입니다. 하지만 객체를 생성하고 사용함에 있어서, 여러 장점들이 손상되는 경우가 생깁니다.

C\#과 Java에서 가장 크게 꼽는 장점중 하나는 OOP\(Object - Oriented - Programming\)이 있습니다.

* OOP\(Object - Oriented - Programming\)의 장점
  * 캡슐화
    * 객체의 속성과 상태를 하나로 묶고\(Class\), 원하는 데이터를 외부에서 접근하지 못하게 감싸는 기능\(접\)입니다.
    * 이 부분은 
  * 상속
  * 다향성




