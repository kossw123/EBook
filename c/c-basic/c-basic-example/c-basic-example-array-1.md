---
description: 'C# Basic Example Grammar'
---

# C\# Basic Example Grammar

## 무엇을 하려고 하는가?

* C\# Basic Tutorial의 Grammar항목의 예시를 작성합니다.



## if, Switch 조건문

* 아래와 같은 조건문의 코드가 존재합니다.





## for, foreach, while, do - while 반복문

* 아래와 같은 반복문의 코드가 존재합니다.





## Iterator, yield, LINQ

* 아래와 같은 Iterator, yield의 코드가 존재합니다.

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

* Iterator
  * Iterator중, IEnumerator interface의 Current라는 Property를 통해 데이터 집합을 가져오게 됩니다.
    * 이를 통해 GetEnumerator\(\) 함수를 사용하여 구현한 IEnumerator를 반환합니다.
      * Main\(\) 함수에서 IEnumerator 변수를 사용하여 MyList의 GetEnumerator\(\) 함수를 통해 작성한 데이터들을 반환받습니다.
        * Current와 MoveNext\(\) 함수를 통해 현재값을 반환받고, 다음 데이터로 넘어갑니다.
* LINQ
  * C\#에서는 LINQ라는 쿼리\(Query\)라는 것이 존재합니다.
  * 여기서는 미리 선언된 string 배열에서 IEnumerable&lt;string&gt;을 통해 LINQ를 사용하여 데이터를 뽑아내고 있습니다.
    * \*\*\* foreach를 사용한다는 것은 IEnumerable, IEnumerable&lt;T&gt; interface를 상속 받는다는 것입니다.
      * LINQ의 from - in은 Foreach와 동일하기 때문에, 해당 interface를 상속받는 배열을 사용할 수 있습니다.
  * IEnumerable은 컬렉션 클래스에서 사용하는 키워드 이기 때문에, while문을 사용하기 위해서는 IEnumerator 변수로 데이터를 담는 작업이 필요합니다.
    * IEnumerable은 자체적으로 객체를 IEnumerator로 반환하는 GetEnumerator\(\) 함수를 가지고 있기 때문에, 이를 이용하여 while문의 조건식을 완성하고, 결과를 출력합니다.

