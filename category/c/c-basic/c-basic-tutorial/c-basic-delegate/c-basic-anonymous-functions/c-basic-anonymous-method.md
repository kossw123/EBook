---
description: 'C# Basic Anonymous Method'
---

# C\# Basic Anonymous Method

## 무엇을 하려고 하는가?

* Anonymous Functions와 비슷한 이름을 가진 Anonymous Method에 대한 개념과 사용방법 및 사례에 대해 기술합니다.

## Anonymous Method\(무명 함수\)란?

* 미리 정의하지 않아도 어떤 데이터 형식의 값으로써 선언할 수 있는 기능입니다.
* 일회용으로 사용하기에 적당하기 때문에, 일부러 함수를 만들어서 호출하지 않아도 선언한 내용이 실행됩니다.

```csharp
using System;

namespace ConsoleApp
{
    class Program
    {
        public delegate void myDelegate();
        static void Main(string[] args)
        {
            // Anonymous Method를 사용한 delegate 생
            myDelegate my = delegate() 
            {
                Console.WriteLine("Anonymous Method");
            };
            
            my();
        }
    }
}
```

## Anonymous Method의 

