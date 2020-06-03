---
description: 'C# Basic Grammar'
---

# C\# Basic Grammar

## 무엇을 하려고 하는가?

* PL\(Programming Language\)을 사용하기 위해서는 해당 언어에 대한 문\(Grammar\)에 대해서 서술합니다.
* 명령문을 사용하기 위한 연산자\(Operator\)와 구문\(Grammar\)에 대해서 알아봅니다.

## Syntax vs Semantics

* 언어에 대한 구문, 의미, 문장에 대해서 구별하는 것이 필요할 것 같습니다.
* 보통 PL에서는 아래와 같이 분류합니다.



* 문법\(Grammar\)
  * 이미 정의되고 형식화된 의미입니다.
    * 보통 우리가 쓰는 반복문\(for, while, do-while\)이나 조건문\(if, switch\)등과 같은 이미 형식화된 단어들의 집합을 문법이라고 니다.
* 구문\(Syntax\)
  * 프로그램의 모습, 형태, 구조가 어떻게 보이는 것인가에 대한 정의입니다.
    * 문법\(Grammar\)에 따라 유효한지 타당한지 확인하는 것과 관련이 있습니다.
      * 예시로, 반복문을 의미하기 위해 작성해야 하는 문법이 있습니다.
      * 반복문을 사용하기 위해서는\(ex : for Loop\)
        * for라는 키워드
        * for를 사용하기 위한 괄호\(\)
        * 괄호\(\) 안에 들어가야할 문법들\(ex : 초기 변수, 반복 범위 변수, 증가 변수\)
    * 즉, 문법\(Grammar\) 규칙을 다루는 분야입니다.
* 의미\(Semantic\)
  * 문장\(Syntax\)이 타당한 의미를 지니고 있는지 아닌지를 판별합니다.
    * 보통 Compiler - Error가 일어나는 이유는 Compiler 자체의 의미 분석기\(Semantic Analytics\)를 통해 Error를 표시합니다.
      * 반복문을 사용하기 위한 Syntax 중에 하나라도 타당하지 않은 값을 넣는다면, 의미\(Semantic\)가 맞지 않기 때문에 발생하는 일입니다.



* 하지만 C\#에 대한 Basic을 위해 문법\(Grammar\)을 통해 어떤 프로그램을 작성하기 위한 키워드와 그에 맞는 구문\(Syntax\)에 대해 설명합니다.





## C\# Basic Grammar

### 조건문

* if 조건문
  * 조건식이 참, 거짓에 따라 서로 다른 문장을 실행합니다.

```csharp
using System;

namespace ConsoleApp1 {
    class Program {
        public static void Main(string[] args) {
            int Variable1 = 10;
            int Variable2 = 10;

            if(Variable1 > Variable2) {
                Console.WriteLine("Variable1 is big");
            }
            else {
                Console.WriteLine("Variable2 is big");
            }
        }
    }
}
```

* Switch 조건문
  * 조건값이 여러 값을 가질 경우 경우의 수를 작성하여 각 경우마다 서로 다른 문장을 실행합니다.

```csharp
switch (category)
{
   case "사과":
      price = 1000;
      break;
   case "딸기":
      price = 1100;
      break;
   case "포도":
      price = 900;
      break;
   default:
      price = 0;
      break;
}
```



### 반복문

* for 반복문
  * Counter 변수를 이용하여 일정 범위 동안 블럭의 내용을 실행합니다.

```csharp
class Program {
    static void Main(string[] args) {
        // for 루프
        for (int i = 0; i < 10; i++) {        // i : Counter 변수, i < 10 : 범
           Console.WriteLine("Loop {0}", i);
        }
    }
}
```

* while 반복문
  * 조건식이 만족할 때 까지 반복합니다.

```csharp
class Program {
    static void Main(string[] args) {
        int i;
        // while 루프
        while(i <= 10) {
           Console.WriteLine("Loop {0}", i);
           i++;
        }
    }
}
```

* do - while 반복문
  * do와 while 키워드 사이 블럭에 내용을 실행합니다.
  * 최소 한번은 실행됩니다.

```csharp
class Program {
    static void Main(string[] args) {
        int i = 0;
        // do - while 루프
        do 
        {
           Console.WriteLine("Loop {0}", i);
           i++;
        } while(i <= 10)
    }
}
```

* foreach 반복문
  * 배열\(Array\)이나 컬렉션\(Collection\)에 주로 사용합니다.
  * 각 요소를 하나씩 꺼내와서 블럭의 내용을 실행합니다.
* for 반복문 vs foreach 반복문
  * 성능적 차이는 그렇게 크지 않습니다.
  * 배열을 사용하는 반복문 경우 foreach 내부의 최적를 거쳐서 for 반복문 보다 효율적으로 사용할 수 있고, 다차원 배열\(Jagged Array\)의 경우 코드의 가독성이 떨어지기 때문에 foreach 반복문을 사용하면 가독성이 훨씬 좋아집니다.

```csharp
static void Main(string[] args) {
    // 3차배열 선언
    string[,,] arr = new string[,,] { 
            { {"1", "2"}, {"11","22"} }, 
            { {"3", "4"}, {"33", "44"} }
    };

    //for 루프 : 3번 루프를 만들어 돌림
    for (int i = 0; i < arr.GetLength(0); i++) {
        for (int j = 0; j < arr.GetLength(1); j++) {
            for (int k = 0; k < arr.GetLength(2); k++) {
                Debug.WriteLine(arr[i, j, k]);
            }
        }
    }

    //foreach 루프 : 한번에 3차배열 모두 처리
    foreach (var s in arr) {
        Debug.WriteLine(s);
    }
}
```

### Iterator\(반복기\)란?

* Iterator\(반복기\)란?
  * 컬렉션, 배열, 목록의 요소를 반복하는데 사용합니다.
    * yield return을 사용하 한번에 각 요소를 반환합니다.
    * 반복을 종료하고 싶을 때는 yield break를 사용합니다.
  * 사용하기 위해서는 IEnumerator라는 interface를 구현해야 합니다.
    * Current\(속성\), MoveNext\(\), Reset\(\)의 기능을 가지고 있고, Current, MoveNext\(\)함수를 꼭 구현해야 합니다.
  * 동일하게 IEnumerable이라는 interface가 존재하는데, 이는 컬렉션\(Collection\) Class와 같이 열거형으로 전환 가능한 클래스의 반복기 역할을 합니다.

### 

### yield문\(양도\)

* Iterator\(반복기\)에서 데이터의 집합으로부터 데이터를 하나씩 호출자에게 보낼 때 사용합니다.
  * 즉, 아래와 같은 경우에 사용하면 유리합니다.
    * 데이터의 양이 커서 한번에 반환이 어려울 때, 조금씩 반환한다면 효율적입니다.
    * 어떤 함수가 무제한의 데이터를 반환할 때, 데이터 집합을 한번에 반환할 수 없기에 yield문을 사용하여 구현합니다.
    * 모든 데이터를 미리 계산하는데 비용이 많이 소모될 때, 원할 때 계산하는 형태인 On Demand로 처리할 때 유리합니다.
* yield문의 실행순
  1. 어떤 호출자가 Iterator를 가진 함수를 호출합니다.
  2. yield return문에서 하나의 값을 반환하고, 해당 함수의 위치를 기억합니다.
  3. 호출자가 다시 Iterator를 가진 함수를 호출하면, 기억한 위치의 다음 문장을 실행하 다음 yield문까지 실행합니다.

{% hint style="info" %}
정리

* 컬렉션, 배열, 목록을 반환하는데, 데이터의 반환을 한번에 하면 안되는 경우 Iterator\(반복기\)라는 interface를 사용합니다.
* Iterator\(반복기\)를 사용할 때 yield문을 통해 데이터를 반환, 종료합니다.
* Iterator를 사용하려면 interface에 필요한 기능을 작성해야 합니다.
  * Current // 필요
  * MoveNext\(\) // 필요
  * Reset\(\)
* Iterator\(반복기\)는 기본적으로 IEnumerator interface를 기반으로 Collection Class에서도 사용할 수 있는 IEnumberable interface가 있습니다.
  * IEnumerable에는 IEnumerator를 반환하는 단일 함수인 GetEnumerator\(\) 함수가 있습니다.
{% endhint %}

```csharp
using System;
using System.Linq;
using System.Collections;
using System.Collections.Generic;

namespace ConsoleApp1 {
    public class MyList {
        private int[] data = { 1, 2, 3, 4, 5 };
        public IEnumerator GetEnumerator() {
            int i = 0;
            while (i < data.Length)
            {
                yield return data[i];
                i++;
            }
        }
    }
    class Program {
        public static void Main(string[] args) {
            var list = new MyList();

            foreach(var item in list) {
                Console.WriteLine(item);
            }

            IEnumerator it = list.GetEnumerator();
            Console.WriteLine("\n GetEnumerator를 통한 배열 반환 \n");
            it.MoveNext();
            Console.WriteLine("GetEnumerator : " + it.Current);
            it.MoveNext();
            Console.WriteLine("GetEnumerator : " + it.Current);


            Console.WriteLine("\n IEnumerable를 통한 string 배열 반환 \n");
            string[] strList = { "jeong", "kang", "won", "lee", "hee" };

            IEnumerable<string> enumerable = from str 
                                             in strList
                                             where str.Length <= 3 
                                             select str;

            IEnumerator<string> enumerator = enumerable.GetEnumerator();
            
            while(enumerator.MoveNext()) {
                Console.WriteLine("Enumerable을 이용한 Enumerator :" + enumerator.Current);
            }
        }
    }
}

```



