---
description: 'C# Basic Exception'
---

# C\# Basic Exception

## 무엇을 하려고 하는가?

* .NET Framework의 Exception에 따른 C\#의 예외처리를 정리합니다.



## 예외 처리란?

* 예외 상황이 생겼을 때, 프로그램은 Error Message를 출력하고 프로그램을 종료시킵니다.
* 뒤에 존재하는 코드가 존재함에도 프로그램이 종료되어 실행이 안됩니다.
* 이러한 예외를 특정 문법을 사용하여 예외에 대한 상황을 만들어 줍니다.



## 예외 처리의 문법

* 3가지 키워드를 사용하여 예외를 컨트롤 합니다.
  * try
    * 실행하고 싶은 프로그램 명령문을 가지고 있습니다.
  * catch
    * try에서 Error가 발생하면 catch문에서 잡히게 됩니다.
    * 모든 예외를 일괄적으로 잡거나 특정 예외를 선별할 수 있습니다.
    * 하나 이상의 catch문을 사용하여 예외의 유형에 따라 서로 다른 상황을 컨트롤 할 수 있습니다.
  * finally
    * 예외가 발생 하던지 안하던지 가장 마지막에 실행됩니다.

```csharp
try {
   //실행 문장들
}
catch (ArgumentException ex) {
   // Argument 예외처리
}
catch (AccessViolationException ex) {
   // AccessViolation 예외처리
}
finally {
   // 마지막으로 실행하는 문장들
}
```

* 
