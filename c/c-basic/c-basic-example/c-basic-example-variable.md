---
description: 'C# Basic Example Variable'
---

# C\# Basic Example Variable

## 무엇을 하려고 하는가?

* 변수에 대한 예시를 써보고 그에 대한 Code를 살펴봅니다.

## 변수 선언

* 다음과 같은 Code로 작성합니다.

```csharp
using System;

namespace ConsoleApp1 {
    class Program {
        public static void Main(string[] args) {
            Nullable<float> nullable_Var = null;        // 변수
            nullable_Var = float.MaxValue;              // 선언한 변수의 데이터 할당
            float float_Var = nullable_Var.Value;       // 선언과 동시에 할당
            Console.WriteLine(float_Var);

            float_Var = 1234.5f;                        // 변수 선언
            Console.WriteLine(float_Var);               // 출력

            int literal_integer = int.MaxValue;         // int형 변수 선언
            Console.WriteLine(literal_integer);
            uint literal_unsigned_integer = uint.MaxValue;      // Unsigned int형 변수 선언
            Console.WriteLine(literal_unsigned_integer);
        }
    }
}

```

* Value type은 따로 데이터를 선언하지 않는다면 최소값이 자동으로 할당됩니다.
* 그렇다면 Null을 할당할 수 없는 것인가? 에 대한 의문을 Nullable이라는 Generics struct를 사용하여 해결한 Code입니다.
* 접미어\(Suffix\)에 문자를 추가하여, Literal 데이터를 생성할 수 있습니다.
  * 한 변수를 통해 Literal Data를 선언했습니다.

위의 주석과 같은 설명을 통해, Value type에 Null을 할당할 수 있습니다.



## 상수 선언

* 다음과 같은 Code로 작성합니다.

```csharp
using System;

namespace ConsoleApp1 {
    class Program {
        public static void Main(string[] args) {
            const int integer_Var = int.MaxValue;

            Console.WriteLine(integer_Var);
            //integer_Var = 100;                    // 할당식의 왼쪽은 변수, 속성 또는
                                                    // 인덱서여야 합니다.
        }
    }
}

```

* 위에서 나온 결과와 같이 Complier상에서 오류가 발생하게 됩니다.
* const 키워드를 사용하여 상수로 선언한다면, Code상에서 변경할 수 없습니다.



