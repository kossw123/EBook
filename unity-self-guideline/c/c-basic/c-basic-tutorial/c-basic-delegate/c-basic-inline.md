---
description: 'C# Basic inline'
---

# C\# Basic inline

## 무엇을 하려고 하는가?

* 익명 함수에서는 대리자\(delegate\)의 사용이 예상되는 지점에 inline문 또는 expression을 사용합니다.
* 그 중 inline이라는 문법에 대해 알아보도록 합니다.

## inline이란?

**함수의 처리를 하기 위해 들어간 간접적인 처리시간 및 메모리\(Overhead\)**를 줄이기 위한 **"함수"**입니다. 보통 함수를 호출하는데는 아래와 같은 과정을 거쳐서 호출이 됩니다.

![&#xCD9C;&#xCC98; - https://hyeonbell.tistory.com/79](../../../../../.gitbook/assets/image%20%28214%29.png)

```cpp
#include <iostream>
using namespace std;

int increseNumber(int a)
{
    return a++;
}

int main()
{
    int i = 0;
    for(int i = 0; i < 10000; i++)
    {
        // 함수를 반복문에서 일정 횟수만큼 호출할 때
        cout << "increseNumber : " << increseNumber(i) << endl;
    }    
    return 0;
}
```

어떤 함수를 통해 반복문에서 10000번 호출한다면, 이 과정에서 발생하는 비용은 무시할수 없을 정도로 고비용이 발생하게 됩니다. 

10000이라는 횟수에서 함수를 호출하는 경우가 고비용을 발생하는 원인이 됩니다.

이러한 비용을 어떻게 하면 줄일 수 있는가?에 대해서 C++에서는 inline이라는 개념을 도입하게 됩니다.

사용 방법은 아래와 같습니다.



```cpp
#include <iostream>
using namespace std;

// inline문 도
inline int increseNumber(int a) { return a++; }

int main() {
    int i = 0;
    for(int i = 0; i < 10000; i++) {
        // inline문을 통해 increseNumber(i) 대신 inline이 사용된 문장이 삽입됩니다.
        cout << "increseNumber : " << increseNumber(i) << endl;
    }    
    return 0;
}
```

C++에서는 객체 지향언어로 비교적 작은 규모의 함수를 사용하게 됩니다. 이런 작은 함수에 inline문을 통해 **함수 호출과정을 줄여서 프로그램의 속도를 향상 시킬 수 있습니다.**

이러한 inline문은 아래와 같은 단점이 있습니다.

* 호출하는 곳이 많다면 그만큼 전체 코드의 길이가 증가한다는 단점이 있습니다.
* 컴파일러는 크기나, 효율에 따라 불필요한 경우 무시하는 판단을 합니다.
* 컴파일러에 따라 재귀함수, static, for, switch, goto문 등을 가진 함수는 inline 함수로 적용되지 않습니다.

