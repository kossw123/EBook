---
description: 'C# Memory assignment'
---

# C\# Memory assignment

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

### Stack에서의 데이터 저장 과정 

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

### 

### 

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

![](../../.gitbook/assets/image%20%28183%29.png)

* 각각 Pointer 형식 x, y라는 주소가 같은 객체에 접근하기 때문에, 위의 Code를 출력하면 결국 값은 변하지 않고 4를 출력하게 됩니다.

### 

### 

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

![](../../.gitbook/assets/image%20%28193%29.png)

* 다음으로 AddFive함수와 parameter의 데이터가 메모리에 할당되고, **int x는 AddFive의 parameter에 복사됩니다.**

![](../../.gitbook/assets/image%20%28190%29.png)

* AddFive\(\) 함수의 실행이 종료된다면, 다시 Go\(\) 함수로 전달이 되고 사용이 끝난 데이터들은 제거됩니다.

![](../../.gitbook/assets/image%20%28194%29.png)

#### Stack과 Heap을 사용하는 코드에 대한 메모리 할당

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

![](../../.gitbook/assets/image%20%28184%29.png)

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

![](../../.gitbook/assets/image%20%28182%29.png)

### 

### 

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

![](../../.gitbook/assets/image%20%28191%29.png)

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

![](../../.gitbook/assets/image%20%28189%29.png)

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

![](../../.gitbook/assets/image%20%28187%29.png)

* 위의 그림은 Switcharoo\(\) 함수의 메모리 할당까지의 그림입니다.
* 함수가 실행된 이후 출력하면 아래와 같은 그림의 메모리 할당을 가지게 됩니다.

![](../../.gitbook/assets/image%20%28181%29.png)





### Value types, Reference types의 차이점으로 인해 발생하는 문제점

* Value type의 값은 함수에 의해 호출이 되면 실제 메모리에 할당된 주소의 값을 변경할 수 없습니다.
* Reference type의 값은 함수에 의해 호출이 되면 실제 메모리에 할당된 주소의 값을 변경합니다.

이 두가지 유형의 차이점으로 인해 발생하는 문제에 대하여 설명하겠습니다.

우리는 많은 데이터 변수들을 한데 묶어서 관리하는게 유용하다는 것을 알고 있습니다. 그럴때 마다 클래스, 구조체, 배열 등등의 데이터 타입을 통해 값을 묶어서 관리합니다.

이때, 메모리의 관리를 위해 Heap의 영역을 사용해야 할지 Stack의 영역을 사용하여 메모리를 할당할 지 고민하게 됩니다.

이 단락은 위의 문제에 대해서 도움이 되고자 참고문서에서 발췌 했습니다.

```csharp
public struct Shoe{ public string Color; }
public class Dude {
    public string Name;
    public Shoe RightShoe;
    public Shoe LeftShoe;
    public Dude CopyDude() {
        Dude newPerson = new Dude();
        newPerson.Name = Name;
        newPerson.LeftShoe = LeftShoe;
        newPerson.RightShoe = RightShoe;
        return newPerson;
    }
    public override string ToString() {
        return (Name + " : Dude!, I have a " + RightShoe.Color 
        + " shoe on my right foot, and a " + LeftShoe.Color 
        + " on my left foot.");
    }
}

public static void Main() {
    Dude Bill = new Dude();
    Bill.Name = "Bill";
    Bill.LeftShoe = new Shoe();
    Bill.RightShoe = new Shoe();
    Bill.LeftShoe.Color = Bill.RightShoe.Color = "Blue";
    Dude Ted = Bill.CopyDude();
    Ted.Name = "Ted";
    Ted.LeftShoe.Color = Ted.RightShoe.Color = "Red";
    Console.WriteLine(Bill.ToString());
    Console.WriteLine(Ted.ToString());
}
```

* 위의 예제 코드가 존재할 시 Dude Class는 아래와 같은 Heap 영역을 가지게 됩니다.

![](../../.gitbook/assets/image%20%28179%29.png)

* 해당 코드의 출력은 아래와 같습니다.
  * Bill : Dude!, I have a Blue shoe on my right foot, and a Blue on my left foot.
  * Ted : Dude!, I have a Red shoe on my right foot, and a Red on my left foot.

하지만 어떤 사정에 의해 Shoe 구조체를 Class로 바꿔서 실행 한다면 아래의 결과와 그림과 같은 영역을 가지게 됩니다.

* Bill : Dude!, I have a Red shoe on my right foot, and a Red on my left foot
* Ted : Dude!, I have a Red shoe on my right foot, and a Red on my left foot

![](../../.gitbook/assets/image%20%28180%29.png)

* 구조체를 클래스로 변경한 것 뿐인데, 결과는 Bill과 Ted가 동일한 출력문을 가지게 됩니다.
* 이는 Class라는 Pointer가 존재하는 Reference type으로 변경했기 때문에 발생하는 문제점 입니다.
  * 정확히 말하자면, Value type인 구조체는 Pointer값을 가지지 않습니다.
  * 하지만 Reference type은 실제 메모리에 주소값을 보내서 해당 주소의 데이터를 변경하는 타입이기 때문에, 나중에 Ted 객체에서 변경한 Shoe.Color 값이 변경된 상태로 유지하게 되는 것입니다.

이런 데이터 간의 종속성 때문에 C\#에서는 ICloneable이라는 interface를 제공합니다.

* ICloneable
  * 하나의 함수로 구성된 interface입니다.
  * 최근의 Instance를 새로운 Object type에 할당하는 interface입니다.
  * interface안에서 선언한 Clone\(\) 함수를 구체적인 내용을 넣어 새로운 객체에 할당합니다.

```csharp
public interface ICloneable {
    object Clone();
}
```



ICloneable이라는 C\#에서 제공되는 interface를 사용하여 나중에 생성한 Dude Class의 객체인 Ted를 새로운 복사본을 가리키게 합니다.

```csharp
public class Shoe : ICloneable {
    public string Color;
    #region ICloneable Members
    public object Clone() {
        Shoe newShoe = new Shoe();
        newShoe.Color = Color.Clone() as string;
        return newShoe;
    }
    #endregion
}
```

그리고 CopyDude 함수를 통해 Ted라는 객체를 생성했기 때문에, 이에 대한 조정이 필요합니다.

왜냐하면, 처음에 생성한 Dude Class는 Bill이고, 이에 대한 정보를 가리키게 했기 때문에, 이 부분을 바꾸지 않는다면 여전히 Bill의 Shoe.Color를 가리키기 때문입니다.

다음과 같이 CopyDude\(\) 함수를 변경합니다.

```csharp
public Dude CopyDude() {
    Dude newPerson = new Dude();
    newPerson.Name = Name;
    newPerson.LeftShoe = LeftShoe.Clone() as Shoe;
    newPerson.RightShoe = RightShoe.Clone() as Shoe;
    return newPerson;
}
```

* 새로운 Dude 객체를 생성합니다.
* 해당 객체는 Shoe Class변수인 Left, RightShoe를 가지고 있는 객체로써 해당 멤버변수도 복제하여 새롭게 할당하기 위해 as 연산자를 통해 형변환을 합니다.
* 최종적으로 newPerson 이라는 객체를 반환하여 복사본을 생성합니다.

이제 Main\(\) 함수를 실행한다면 다음과 같은 출력과 메모리 할당을 가지게 됩니다.

* 출력
  * Bill : Dude!, I have a Blue shoe on my right foot, and a Blue on my left foot
  * Ted : Dude!, I have a Red shoe on my right foot, and a Red on my left foot
* 메모리 할당

![](../../.gitbook/assets/image%20%28192%29.png)

## 마치며



