---
description: 'C# Basic Variable'
---

# C\# Basic Variable

## 무엇을 하려고 하는가?

* C\#에서의 가장 기초적인 변수에 대해서 알아보려고 합니다.
* 개념과 메모리 할당과 앞서 Essential element에서 설명한 데이터 유형에 대해 정리하는 문서입니다.

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

![](../../../.gitbook/assets/image%20%28188%29.png)

