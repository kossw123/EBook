---
description: TR Photon + FireBase Network Game
---

# TR Photon + FireBase Network Game - 작성중

## 무엇을 하려고 하는가?

* Script에 사용된 Photon, Firebase에 관련한 함수 및 기능들을 보며, Code에 대한 이해를 돕습니다.
* 해설문서\(Explanation\)에 작성하려고 했지만 해당 문서와 해설문서의 성질이 맞지 않다고 생각했기에 기술문서\(Technical reference\)에 작성했습니다.

## Firebase 관련 함수

{% tabs %}
{% tab title="Firebase는 어떤 역할인가?" %}
* Firebase는 외부 저장소 역할과, 인증 역할 등등을 담당하기 때문에, Photon Server간의 동기화가 필요없습니다.
* 그렇기 때문에, Firebase를 통해 Authentication\(인증\)기능만 사용하여 처음에 Login 기능을 제외한 모든 Logic이나 동기화는 Photon이 제공하는 API를 사용해야 합니다.
{% endtab %}

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
      * 여기서는 ContinueWith\(\) 함수의 Chaining Callback Method의 내용에 따라, 비동기 로 해당 내용을 실행합니다.
* continuation
  * Lambda식을 사용하여 ContinueWith\(\) 함수에 Callback Method를 넘깁니다.
  * 이때 사용된 continuation: 이라는 문장은 named parameter로써 일반적으로 parameter를 넘길 때 순차적으로 넘기지만, 위치와 상관없이 named parameter를 지정하여 전달할 수 있도록 합니다.
  * ContinueWith\(\) 함수의 parameter는 아래의 그림과 같기 때문에 쓰던지 안쓰던지 상관은 없지만서도, named parameter를 사용하는 의미는 좀 더 모호성이 없는 전달을 위함이라고 생각됩니다.
  * 보기 쉽게 Code를 정렬하여 보자면 아래와 같습니다.

![ContinueWith\(\) &#xD568;&#xC218;&#xC758; parameter](../../.gitbook/assets/image%20%28165%29.png)

```csharp
FirebaseApp.CheckAndFixDependenciesAsync().ContinueWith(
    continuationAction: task => { 
                                    var result = task.Result;
                                    if(result != DependencyStatus.Available)
                                    {
                                        Debug.Log(result.ToString());
                                        IsFirebaseReady = false;
                                    }
                                    else
                                    {
                                        IsFirebaseReady = true;
                                        firebaseApp = FirebaseApp.DefaultInstance;
                                        firebaseAuth = FirebaseAuth.DefaultInstance;
                                    }
                                    signInButton.interactable = IsFirebaseReady;
                                }
    );
```

_named parameter를 사용하여 넘길 parameter를 지정했는데, 뒤에 task 변수는 약간 생소합니다. Lambda식을 사용한 것은 맞지만, =&gt;의 좌항에는 parameter 변수가 들어가야 하는데, 딱히 괄호를 치고 parameter 변수를 넣지 않아도 성립합니다._

\_\_

*  그 후에 Task Class 변수인 task를 선언하고 Action Delegate의 내용으로 위의 내용을 넣습니다.
* 무명함수\(Anonymous Method\)를 사용하기 위해 C\#에서 미리 선언돼있는, Action Delegate이기 때문에 반환값을 필요없고, 내용만 넣습니다.
  * 이 의미는 ContinueWith\(\) 함수를 사용하여 해당 구문이 끝났을 때 실행될 내용을 등록하는 것입니다.

{% code title="AuthManager.cs" %}
```csharp
firebaseAuth.SignInWithEmailAndPasswordAsync().ContinueWithOnMainThread();
```
{% endcode %}

* firebaseAuth.SignInWithEmailAndPasswordAsync\(\).ContinueWithOnMainThread\(\)
  * 1. FirebaseAuth Class에 접근합니다.
    2. FirebaseAuth Class의 SignInWithEmailAndPasswordAsync\(\)에 접근합니다.
    3. SignInWithEmailAndPasswordAsync\(\) 함수는 Task Class로 구성되어 있기 때문에 Chaining할 Method를 등록해야 합니다.
  * 해당 함수는 Email이나, Password를 사용하여 Login을 가능하게 하는 함수입니다.
    * 똑같이 Task Class를 사용하기 때문에 Continue Task Method를 사용하여 작업이 완료되면 실행될 다음 Method를 추가합니다.
    * Google, Apple 계정을 이용하여 외부 서비스에서 Login이 가능하게 하는 함수인 SignInWithCredentialAsync\(\) 함수도 있습니다.
{% endtab %}
{% endtabs %}

## Photon 관련 함수

{% tabs %}
{% tab title="Photon은 어떤 역할인가?" %}
* Photon은 Server의 역할을 하기 때문에, 위에서 사용한 Firebase와는 다른 구조를 가지고 있습니다.
* Firebase에서는 Authentication\(인증\) 기능만을 사용했다면 여기서는 Photon에서 제공하는 API를 사용하여 플레이어 간의 동기화를 제공합니다.
{% endtab %}

{% tab title="Variable" %}
```text
MonoBehaviourPunCallbacks Class

PhotonNetwork.GameVersion = gameVersion;
```

* MonoBehaviourPunCallbacks : PUN2를 자동으로 감지할 수 있는 MonoBehaviour입니다.
  * 이를 사용하기 위해서는 기존의 MonoBehaviour를 상속받는 기본 Script의 Main Class를 MonoBehaviourPunCallbacks로 교체해야합니다.
  * 기존의 MonoBehaviour Class는 Unity가 기본적으로 상속받아야할 Class이지만, 이는 기본적으로 Unity 자체적으로 제공되기 때문에 Photon과는 맞지 않는 함수들이 많습니다.
  * 이러한 이유 때문에 Photon에서는 MonoBehaviourPunCallbacks이라는 Class를 제공함으로써 Photon을 사용할 수 있는 Class를 제공합니다.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

* PhotonNetwork.GameVersion = gameVersion
  * 우리가 접할 수 있는 Game에서는 Version이라는 것이 있습니다.
  * 이를 통해 새로운 Content들을 업데이트 하고, 이전 프로그램과 구분 짓기 위해서 만듭니다.
  * Photon에서는 사용자들을 호환되는 Version으로 그룹지을 수 있습니다.
  * 때문에, 이 GameVersion이 맞지 않는다면, 다른 그룹으로 간주하여, 멀티플레이가 안될 수 있습니다.
{% endtab %}

{% tab title="Method" %}


```csharp
1. PhotonNetwork.ConnectUsingSettings();
2. void OnConnectedToMaster()
3. void OnDisconnected(DisconnectCause cause)
4. PhotonNetwork.JoinRandomRoom();
5. void OnJoinRandomFailed(short returnCode, string message)
6. PhotonNetwork.CreateRoom(null, new RoomOptions { MaxPlayers = 2 });
7. void OnJoinedRoom()
```

* PhotonNetwork.ConnectUsingSettings\(\) 
  * PhotonNetwork Class를 사용하여 
{% endtab %}
{% endtabs %}

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

