---
description: 'C# Basic Tutorial Variable'
---

# C\# Basic Tutorial Variable

## 무엇을 하려고 하는가?

* C\#에서의 가장 기초적인 변수에 대해서 알아보려고 합니다.
* 개념과 메모리 할당과 앞서 Essential element에서 설명한 데이터 유형에 대해 정리하는 문서입니다.
* 아래의 링크에서 참고 자료를 가져왔습니다.

{% embed url="https://www.csharpstudy.com/CSharp/CSharp-datatype.aspx" %}

{% embed url="https://docs.microsoft.com/ko-kr/dotnet/csharp/" %}





## 변수란?

* 데이터를 담는 공간입니다.
* 데이터 유형에 따라 각각 다른 데이터가 할당되고 .NET Framework에서는 System.Object에서 상속된 Value type / Reference type의 유형으로 나눠집니다.
* C\#에서는 기본적으로 제공하는 형식은 다음과 같습니다.

Value type

| C\# 형식 키워드 | .NET 형식 |
| :--- | :--- |
| [`bool`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/bool) | [System.Boolean](https://docs.microsoft.com/ko-kr/dotnet/api/system.boolean) |
| [`byte`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/integral-numeric-types) | [System.Byte](https://docs.microsoft.com/ko-kr/dotnet/api/system.byte) |
| [`sbyte`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/integral-numeric-types) | [System.SByte](https://docs.microsoft.com/ko-kr/dotnet/api/system.sbyte) |
| [`char`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/char) | [System.Char](https://docs.microsoft.com/ko-kr/dotnet/api/system.char) |
| [`decimal`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types) | [System.Decimal](https://docs.microsoft.com/ko-kr/dotnet/api/system.decimal) |
| [`double`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types) | [System.Double](https://docs.microsoft.com/ko-kr/dotnet/api/system.double) |
| [`float`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/floating-point-numeric-types) | [System.Single](https://docs.microsoft.com/ko-kr/dotnet/api/system.single) |
| [`int`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/integral-numeric-types) | [System.Int32](https://docs.microsoft.com/ko-kr/dotnet/api/system.int32) |
| [`uint`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/integral-numeric-types) | [System.UInt32](https://docs.microsoft.com/ko-kr/dotnet/api/system.uint32) |
| [`long`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/integral-numeric-types) | [System.Int64](https://docs.microsoft.com/ko-kr/dotnet/api/system.int64) |
| [`ulong`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/integral-numeric-types) | [System.UInt64](https://docs.microsoft.com/ko-kr/dotnet/api/system.uint64) |
| [`short`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/integral-numeric-types) | [System.Int16](https://docs.microsoft.com/ko-kr/dotnet/api/system.int16) |
| [`ushort`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/integral-numeric-types) | [System.UInt16](https://docs.microsoft.com/ko-kr/dotnet/api/system.uint16) |

Reference type

| C\# 형식 키워드 | .NET 형식 |
| :--- | :--- |
| [`object`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/reference-types#the-object-type) | [System.Object](https://docs.microsoft.com/ko-kr/dotnet/api/system.object) |
| [`string`](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/reference-types#the-string-type) | [System.String](https://docs.microsoft.com/ko-kr/dotnet/api/system.string) |
|  [class](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/class) |  |
|  [interface](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/keywords/interface) |  |
|  [delegate](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/reference-types) |  |
|  [dynamic](https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/builtin-types/reference-types) |  |





## 변수의 메모리 할당

* 데이터를 담는 공간인 변수는 데이터 타입에 따라 메모리 상에서 저장되는 영역이 다릅니다.
  * Value type = Stack
  * Reference type = Heap

![](../../../.gitbook/assets/image%20%28195%29.png)

* 위의 그림과 같이 영역에 대한 메모리가 할당됩니다.
* 보통 C\# Project를 작성하면 하나의 Calss안에 Main\(\)함수가 아래와 같이 자동으로 생성됩니다.

```csharp
using System;

namespace ConsoleApp1 {
    class Program {
        public static void Main(string[] args) {
            
        }
    }
}
```

* 보통 변수를 선언한다면 Class안에 선언을 합니다.
  * 이렇게 Class안에 선언한 변수를 멤버 변수\(Member Variable\) 혹은 필드\(Field\)라고 합니다.
  * Class 안에 선언한 **함수**를 멤버 함수\(Member Method\)라고 합니다.





## Literal

* C\#에서는 Code에 값을 직접 쓸 수 있습니다.
* 이때, 별도의 접미어\(Suffix\) 표시를 해야합니다.

## 최대, 최소값

* 숫자형 데이터 타입에는 .NET Framework가 제공하는 최대, 최소값을 제공합니다.

```csharp
int i = int.MaxValue;
float f = float.MinValue;
```

## NULL

* 어떤 변수가 메모리에 어떤 데이터도 가지지 않다는 것을 표시하기 위한 키워드입니다.
* Reference type만 가질 수 있습니다.

## Nullable type

* 보통 Value type은 NULL값을 가질 수 없습니다.
* 하지만 C\# 2.0부터는 Value type도 Null을 가질 수 있게 했습니다.
* 타입명 뒤에 ?를 붙이면 null값을 쓸 수 있습니다.

```csharp
// Nullable 타입
int? i = null;
i = 101;
            
bool? b = null;

//int? 를 int로 할당
Nullable<int> j = null;
j = 10;
int k = j.Value;
```

## 변수명 작성시 주의 사항

* 대소문자를 구별합니다.
* 
## 마치며

{% page-ref page="c-basic-variable.md" %}

