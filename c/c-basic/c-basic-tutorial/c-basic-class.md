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
    * 아래와 같은 예시로 생성자를 선언하고 내부에서 Field에 선언된 변수를 초기화 한다면 Main함수에서는 동적으로 생성한다는 문장과 동시에 parameter로 넘기게 되면 Class의 동적 생성과 동시에 호출되는 Constructor로 인해 int a는 자동적으로 param에서 선언한 데이터가 들어가게 됩니다.

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

### Class의 사용 장점

* Class는 속과 상태가 결합된 하나의 데이터 구조입니다.
* Class를 사용하려면 동적으로 생성하여 Heap 영역에 할당하여 프로그램이 종료될 때 까지 살아있습니다.
* "그렇다면 이런 데이터 구조는 어떻게 사용하는 것이 가장 좋은가?"에 대한 이야기를 하려고 합니다.

C\#은 Java와 C++의 장점을 도입하여 Multiple paradigm을 가진 PL입니다. 하지만 객체를 생성하고 사용함에 있어서, 여러 장점들이 손상되는 경우가 생깁니다.

C\#과 Java에서 가장 크게 꼽는 장점중 하나는 OOP\(Object - Oriented - Programming\)이 있습니다.

* OOP\(Object - Oriented - Programming\)의 장점
  * 캡슐화
    * 객체의 속성과 상태를 하나로 묶고\(Class\), 원하는 데이터를 외부에서 접근하지 못하게 감싸는 기능\(접근 제한자\)입니다.
  * 상속
    * 객체들 간의 관계를 구축하는 방법입니다.
    * 부모, 자식관계로 구축이 되며 부모의 자산\(데이터\)을 자식이 쓸 수 있다는 개념입니다. 하지만 자식의 자산\(데이터\)를 부모가 쓰려면 부모 클래스의 변환이 필요합니다.
      * 여기서 데이터는 Field에 선언된 모든 데이터 형식을 의미합니다.
    * 즉, 일방적\(부모 -&gt; 자식\)으로 데이터의 사용은 가능하지만 그 반대\(자식 -&gt; 부모\)의 경우에는 별도의 업캐스팅\(UpCasting\)이 필요합니다.
  * 다형성
    * 다형성이란  _Polymorphism_이라는 "여러 형태"를 의미하는 단어입니다.
    * 오버로딩과 비슷한 단어인 오버라이딩\(Overriding\)을 통해 작동됩니다.
    * **기존 부모 클래스의 동작에서 파생된 자식 클래스의 동작을 그대로 실행하는 것이 아닌 다른, 혹은 추가동작을 실행하는 것을 의미합니다.**
    * 아래의 예시를 통해 쉽게 알아 볼 수 있습니다.

```csharp
public class Shape {
    // A few example members
    public int X { get; private set; }
    public int Y { get; private set; }
    public int Height { get; set; }
    public int Width { get; set; }

    // Virtual method
    public virtual void Draw()
    {
        Console.WriteLine();
        Console.WriteLine();
        Console.WriteLine("///////////////////// Shape Class /////////////////////");
        Console.WriteLine("Performing base class drawing tasks");
    }
}
public class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine();
        Console.WriteLine();
        // Code to draw a circle...
        Console.WriteLine("///////////////////// Circle Class /////////////////////");
        Console.WriteLine("Drawing a circle");
        base.Draw();
    }
}
public class Rectangle : Shape
{
    public override void Draw()
    {
        Console.WriteLine();
        Console.WriteLine();
        Console.WriteLine("///////////////////// Rectangle Class /////////////////////");
        // Code to draw a rectangle...
        Console.WriteLine("Drawing a rectangle");
        base.Draw();
    }
}
public class Triangle : Shape
{
    public override void Draw()
    {
        Console.WriteLine();
        Console.WriteLine();
        Console.WriteLine("///////////////////// Triangle Class /////////////////////");
        // Code to draw a triangle...
        Console.WriteLine("Drawing a triangle");
        base.Draw();
    }
}

static void Main(string[] args)
{
    var shapes = new List<Shape> { new Rectangle(), new Circle(), new Triangle() };
    foreach(var item in shapes) { item.Draw(); }
    
        /* Output:
            ///////////////////// Rectangle Class /////////////////////
            Drawing a rectangle
            ///////////////////// Shape Class /////////////////////
            Performing base class drawing tasks


            ///////////////////// Circle Class /////////////////////
            Drawing a triangle
            ///////////////////// Shape Class /////////////////////
            Performing base class drawing tasks


            ///////////////////// Triangle Class /////////////////////
            Drawing a circle
            ///////////////////// Shape Class /////////////////////
            Performing base class drawing tasks
        */
}
```

{% hint style="info" %}
오버로딩\(Overloading\), 오버라이딩\(Overriding\)

다른 개념이지만, 비슷한 이름으로 인해 혼동하는 경우가 있습니다.

* Overloading
  * 함수 이름을 같지만 매개변수가 다르게 하여 다양한 방법으로 호출할 수 있도록 합니다.
* Overriding
  * Overloading과는 다르게 Class에서 부모 클래스가 가지고 있는 Field를 상속받아 자식 클래스에서 부모 클래스의 동작에 추가하거나, 다른 방식으로 동작 하도록합니다.
{% endhint %}



### Class의 메모리 할당

* 보통 동적으로 생성하기 때문에, 객체는 Heap영역에 할당됩니다.
* 하지만 Stack부분에서 Heap영역에 저장한 부분의 주소를 할당하기 때문에, 일반적으로 동적 생성이란 Stack의 일부분과 Heap의 일정 영역을 사용한다고 생각하시면 될것 같습니다.
* 
