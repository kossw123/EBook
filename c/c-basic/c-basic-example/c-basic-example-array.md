---
description: 'C# Basic Example Array'
---

# C\# Basic Example Array

## 무엇을 하려고 하는가?

* 변수에 대한 예시를 써보고 그에 대한 Code를 살펴봅니다.

## 배열 선언

```csharp
using System;

namespace ConsoleApp1 {
    class Program {
        public static void Main(string[] args) {
            int[] arr = { 1, 2, 3, 4 };

            for(int i = 0; i < arr.Length; i++) {
                Console.WriteLine(arr[i]);
            }
        }
    }
}

```

* 배열의 인덱스는 명시적으로 선언하지 않아도, 배열의 갯수에 따라 자동적으로 메모리가 할당됩니다.
* System.Array Class의 Length 속성을 통해 배열의 길이를 받아와 하나씩 출력하고 있습니다.

## 배열 전달

* Reference type이기 때문에 배열의 이름을 가지고 있는 Pointer를 넘겨서 함수로 전달할 수 있습니다.

```csharp
using System;

namespace ConsoleApp1 {
    class Program {
        public static void Main(string[] args) {
            int[] arr = { 1, 2, 3, 4 };                    // 1. 배열 선언
            Calculate(arr);                                // 3. 함수를 실

            void Calculate(int[] arr) {                    // 2. 선언한 배열을 함수의 parameter로 전
                for (int i = 0; i < arr.Length; i++) {
                    Console.WriteLine(arr[i]);
                }
            }
        }
    }
}

```

