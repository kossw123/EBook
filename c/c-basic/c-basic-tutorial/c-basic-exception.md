---
description: 'C# Basic Exception'
---

# C\# Basic Exception - 작성중

## 무엇을 하려고 하는가?

* .NET Framework의 Exception에 따른 C\#의 예외처리를 정리합니다.



## 예외 처리란?

* 예외 상황이 생겼을 때, 프로그램은 Error Message를 출력하고 프로그램을 종료시킵니다.
* 뒤에 존재하는 코드가 존재함에도 프로그램이 종료되어 실행이 안됩니다.
* 이러한 예외를 특정 문법을 사용하여 예외에 대한 상황을 만들어 줍니다.



## 예외 처리의 Syntax

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

**try문에서 예외가 발생해서 catch문에서 잡았다면, 예외는 이미 처리된 것으로 간주됩니다.**

즉, Console로 표시되는 Error에 관한 Message들은 이미 처리된 문장이고, 프로그램을 작성하다보면 이를 다시 상위 호출자로 보내고 싶을 때에 throw 키워드를 사용하여 보냅니다.

* throw
  * 프로그램 실행중 예외를 발생시키는 신호를 보냅니다.
  * throw는 3가지 방식으로 사용할 수 있습니다.
    * throw ex;
      * catch에서 전달받은 예외 parameter 변수를 사용하는 경우입니다.
      * 위 코드에서 catch문에서 사용한 ex라는 parameter를 사용합니다.
        * 이러한 방식은 catch문에서 발생한 예외정보를 보존하지만 Stack Trace 정보를 초기화 시키기 때문에 이전 Call Stack의 정보를 유실하게 됩니다.
          * \*\*\* Stack Trace??
            * 특정한 시점에서의 Stack에 관한 정보입니다.
          * \*\*\* Call Stack\(Runtime Stack, Stack, Machine Stack...\)
            * 여러 이름이 있지만 그냥 메모리 할당 항목에서 기재한 Stack과 동일합니다.
    * throw new ExceptionName\(\);
      * new 키워드를 사용하여 새로운 예외 객체를 만드는 방법입니다.
      * catch문에서 잡은 예외를 한번 랩핑하여 새로운 예외를 전달할 때 사용되는데, 잘못하면 이전 예외정보를 잃을 수 있습니다.
        * \*\*\* 랩핑?
          * 기본기능을 감싸는 새로운 기능을 만드는 
    * throw;
      * throw 키워드 뒤에 아무것도 사용하지 않는 경우입니다.
      * 이 경우 catch문에서 잡힌 예외를 상위 호출자로 전달하는 일을 하게 됩니다.

```csharp
using System;
using System.Linq;
using System.Collections;
using System.Collections.Generic;
namespace ConsoleApp1 {
    public class NumberGenerator {
        int [] numbers = {
            2,
            4,
            6,
            8,
            10,
            12,
            14,
            16,
            18,
            20,
            22
        };
        public int GetNumber(int index) {
            if (index < 0 || index >= numbers.Length) {
                throw new IndexOutOfRangeException();
            }
            return numbers[index];
        }
    }
    class Program {
        public static void Main(string[] args) {
            var gen = new NumberGenerator();
            int index = 10;
            try {
                int value = gen.GetNumber(index);
                Console.WriteLine($ "Retrieved {value}");
            } catch (IndexOutOfRangeException e) {
                Console.WriteLine($ "{e.GetType().Name}: {index} is outside the bounds of the array");
            }
        }
    }
}
```



