---
description: 'C# Basic Example Exception'
---

# C\# Basic Example Exception - 작성중

## 무엇을 하려고 하는가?

* 예외 처리 관련 코드를 작성하고, 그에 따른 Review를 합니다.

```csharp
using System;
using static System.Console;
namespace Exception {
    class Program {
        private static double SafeDivision(double x, double y) {
            if (y == 0) 
                throw new System.DivideByZeroException();
            
            return x / y;
        }
        public static void Main(string[] args) {
            double a = 98,
            b = 0;
            double result = 0;
            try {
                result = SafeDivision(a, b);
                WriteLine("{0} divided by {1} = {2}", a, b, result);
            }
            // catch (DivideByZeroException e) {
            //    Console.WriteLine("Attempted divide by zero.");
            // } 
            finally {
                WriteLine("End");
            }
        }
    }
}
```

* try문에서의 SafeDivision\(\) 함수가 실행될 때 런타임 오류가 발생하도록 만든 프로그램입니다.
* 주석을 해제한다면, "Attempted divide by zero"가 출력되며, finally를 사용하여 오류 발생시 프로그램을 종료하지 않고 마지막까지 출력 하도록 작성했습니다.
* 
