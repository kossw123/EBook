---
description: TR Photon + FireBase Network Game
---

# TR Photon + FireBase Network Game - 작성중

## 무엇을 하려고 하는가?

* Script에 사용된 Photon, Firebase에 관련한 함수 및 기능들을 보며, Code에 대한 이해를 돕습니다.
* 해설문서\(Explanation\)에 작성하려고 했지만 해당 문서와 해설문서의 성질이 맞지 않다고 생각했기에 기술문서\(Technical reference\)에 작성했습니다.

## Firebase 관련 함수

{% tabs %}
{% tab title="Variable" %}
{% code title="AuthManager.cs" %}
```csharp
public static FirebaseApp firebaseApp;
public static FirebaseAuth firebaseAuth;
public static FirebaseUser User;

DependencyStatus.Available
```
{% endcode %}

* firebaseApp 
  * FirebaseApp Class변수입니다. 이를 통해 내부 함수에 접근합니다.
  * FirebaseApp Class는 Firebase의 서비스 사이의 통신을 위한 통로 역할을 합니다.
  * 기본 Instance가 자동으로 생성되며, Firebase API가 자동으로 연결됩니다.
    * 즉, Firebase Project에서 어떤 기능을 활성화 하면 자동으로 FirebaseApp Class를 통해 접근할 수 있습니다.
* firebaseAuth 
  * Firebase에서 authentication을 관리하는 Class 변수입니다.
  * Singleton pattern을 가졌기 때문에, 이 Object를 사용할 시 FirebaseAuth.DefaultInstance를 통해 먼저 호출해야합니다.
* User 
  * 해당 Class를 통해 사용자 프로필을 조정하고 Authentication에 연결/해제를 하며, Authentication\(인증\) Token을 관리합니다.
{% endtab %}

{% tab title="Method" %}
{% code title="AuthManager.cs" %}
```csharp
FirebaseApp.CheckAndFixDependenciesAsync().ContinueWith()
```
{% endcode %}

* FirebaseApp.CheckAndFixDependenciesAsync\(\).ContinueWith\(\)
  * 1. FirebaseApp에 먼저 접근합니다.
    2. FirebaseApp Class의 CheckAndFixDependenciesAsync\(\) 함수에 접근합니다.
    3. CheckAndFixDependenciesAsync\(\) 함수는 Task Class type이기 때문에 System.Threading.Tasks namespace에 접근하여 Task Class의 ContinueWith\(\) 함수를 사용합니다.
  * 해당 함수는 Firebase에 필요한 모든 종속성이 시스템에 존재하는지, 필요한 상태인지, 비동기적으로 확인하고 그렇지 않는 경우에는 수정하려고 시도합니다.
    * 종속성?
      * **데이터의 구조가 프로그램 데이터 저장방식을 결정하고, 반대로 프로그램의 데이터 저장방식에 따라 데이터의 저장방식이 바뀌는 것을 의미합니다.**
      * **데이터 구조가 변경되면, 프로그램까지 같이 바뀌는 비용이 들기 때문에 , 프로그램 개발과 유지보수가 어려워지는 문제점이 있습니다.**
      * 여기서는 ContinueWith\(\) 함수의 Chaining CallbackMethod의 내용에 따라, 비동기 로 해당 내용을 실행합니다.
{% endtab %}
{% endtabs %}

## Photon 관련 함수

{% tabs %}
{% tab title="First Tab" %}

{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

## Thread

* Task Class를 알기 위해서 Threading Class에 관하여 먼저 정리하겠습니다.

{% tabs %}
{% tab title="개요" %}
* 보통 인터넷을 하면서, 노래를 듣고, 동시에 작업을 할 수 있습니다. 이러한 작업들을 Process라고 합니다.
* 이러한 Process는 하나 이상의 Thread로 이루어지고, 독립적인 실행 경로이며, 다른 Thread와 동시에 실행할 수 있는 기능입니다.
* 단일 Thread를 실행하고 거기서 다른 Thread를 생성하고, 실행하여 Multi Thread를 가능하게 합니다.
* 즉, Thread는 Process를 여러개의 조각으로 나눈 것으로, Thread 변수를 통해 작업을 동시에 실행하는 것과 같은 효과를 가지게 합니다.

![Thread &#xACFC;&#xC815;](../../.gitbook/assets/image%20%28160%29.png)
{% endtab %}

{% tab title="Thread 기본 예제" %}
{% embed url="https://wergia.tistory.com/187" %}

* 위 사이트 중 하나의 예제를 살펴 보겠습니다.

```csharp
using System;
using System.Threading;

namespace ConsoleApp1 {
    class Program {
        public static void Main(string[] args) {
            Thread t1 = new Thread(() => Run(0));
            t1.Start();
            Thread t2 = new Thread(() => Run(1));
            t2.Start();
        }
        public static void Run(int idx) {
            Console.WriteLine(string.Format("Run {0} : Start", idx));
            for (int i = 0; i < 10; i++) {
                Console.WriteLine(string.Format("Run {0} : {1}", idx, i));
                Thread.Sleep(10);
            }
            Console.WriteLine(string.Format("Run {0} : End", idx));
        }
    }
}

```

* 아래의 그림을 가지고 정리를 하겠습니다.

![Thread Example](../../.gitbook/assets/image%20%28162%29.png)

* Run\(\) 함수의 작업을 Thread 변수를 가지고 Lambda식을 사용하여 Start\(\) 하면 위의 그림과 같이 출력이 됩니다.
* 출력 결과를 보면 Run 0, Run 1이 실행되는데 번갈아 가면서 실행하는데, 일정하게 반복해서 출력되는 것이 아니라 나눠서 실행되는 것을 확인할 수 있습니다.
{% endtab %}
{% endtabs %}

## Task Class

* Firebase의 Authentication\(인증\) 기능을 사용하기 위해 Firebase가 정상적으로 돌아가는지를 확인하기 위한 ContinueWith\(\) 함수를 알기 위해서는 Task Class에 대해 알아야 합니다.
* Task Class에 대한 개요, 개념과 간단한 예제를 통해 문서를 작성하겠습니다.

{% tabs %}
{% tab title="개요" %}
* Task라는 것은 작성자가 하려고 하는 작업, 작업의 단위를 뜻합니다.
* Task Class를 사용한다는 것은 Thread를 가져와서 작업을 비동기 처리한다는 것을 의미합니다.
* Task Class을 사용하기 위해서는 주로  Action Delegate를 사용하여 Instance를 생성해야 합니다.
* 값을 반환하지 않기 때문에 Return 값이 필요하지 않습니다.
  * **이는 Instance를 생성할 때 반환값이 필요없는 Action Delegate를 넘겨 받기 때문입니다.**
  * 만약 값을 반환하는 Task Class를 사용하려면 Task&lt;TResult&gt; Class를 사용하면 됩니다.
* 가장 일반적으로 Lambda식을 사용하기 때문에 처음 접할시 파악에 어려움을 겪을 수 있습니다.
* 비동기 처리를 하기 때문에 Code의 실행 흐름이 파악이 안될 수 있습니다.
* 아래의 그림은 Task를 사용한 한 프로그램의 흐름도입니다.

![](../../.gitbook/assets/image%20%28163%29.png)
{% endtab %}

{% tab title="Task 기본 예제" %}
* 아래의 Code는 Task Class를 사용한 Action Delegate 실행합니다.

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Program {
    public static void Main(string[] args) {
        Action Someaction = () => {
            Thread.Sleep(1000);
            Console.WriteLine(string.Format("Printed asynchronously!"));
        };

        Task task = new Task(Someaction);
        task.Start();
        Console.WriteLine("Printed synchronously");

        task.Wait();
    }
}
```

* 출력 결과 task.Start\(\)를 통해 Task Class 변수에서 생성한 Instance는 Someaction이라는 Action Delegate를 가지고 실행합니다. 
* Thread.Sleep\(1000\) 문장을 통해 1초 뒤 실행하도록 구성되어 있습니다.
* task.Wait\(\) 함수를 통해 작업이 완료 될 때 까지 대기하고 작업이 끝났다면 출력 하도록 합니다.
{% endtab %}
{% endtabs %}

