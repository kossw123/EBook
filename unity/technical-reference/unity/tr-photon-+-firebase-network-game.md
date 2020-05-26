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

* `firebaseApp` 
  * FirebaseApp Class변수입니다. 이를 통해 내부 함수에 접근합니다.
  * FirebaseApp Class는 Firebase의 서비스 사이의 통신을 위한 통로 역할을 합니다.
  * 기본 Instance가 자동으로 생성되며, Firebase API가 자동으로 연결됩니다.
    * 즉, Firebase Project에서 어떤 기능을 활성화 하면 자동으로 FirebaseApp Class를 통해 접근할 수 있습니다.
* `firebaseAuth` 
  * Firebase에서 authentication을 관리하는 Class 변수입니다.
  * Singleton pattern을 가졌기 때문에, 이 Object를 사용할 시 `FirebaseAuth.DefaultInstance`를 통해 먼저 호출해야합니다.
* `User` 
  * 해당 Class를 통해 사용자 프로필을 조정하고 Authentication에 연결/해제를 하며, Authentication\(인증\) Token을 관리합니다.
{% endtab %}

{% tab title="Method" %}
{% code title="AuthManager.cs" %}
```csharp
FirebaseApp.CheckAndFixDependenciesAsync().ContinueWith()
```
{% endcode %}

* `FirebaseApp.CheckAndFixDependenciesAsync().ContinueWith()`
  * 1. FirebaseApp에 먼저 접근합니다.
    2. FirebaseApp Class의 `CheckAndFixDependenciesAsync()` 함수에 접근합니다.
    3. `CheckAndFixDependenciesAsync()` 함수는 Task Class type이기 때문에 System.Threading.Tasks namespace에 접근하여 Task Class의 `ContinueWith()` 함수를 사용합니다. 
  * 해당 함수는 Firebase에 필요한 모든 종속성이 시스템에 존재하는지, 필요한 상태인지, 비동기적으로 확인하고 그렇지 않는 경우에는 수정하려고 시도합니다.
    * 종속성?
      * **데이터의 구조가 프로그램 데이터 저장방식을 결정하고, 반대로 프로그램의 데이터 저장방식에 따라 데이터의 저장방식이 바뀌는 것을 의미합니다.**
      * **데이터 구조가 변경되면, 프로그램까지 같이 바뀌는 비용이 들기 때문에 , 프로그램 개발과 유지보수가 어려워지는 문제점이 있습니다.**
      * 여기서는 ContinueWith\(\) 함수의 Chaining Callback Method의 내용에 따라, 비동기 로 해당 내용을 실행합니다.
* continuation
  * Lambda식을 사용하여 `ContinueWith()` 함수에 Callback Method를 넘깁니다.
  * 이때 사용된 continuation: 이라는 문장은 named parameter로써 일반적으로 parameter를 넘길 때 순차적으로 넘기지만, 위치와 상관없이 named parameter를 지정하여 전달할 수 있도록 합니다.
  * `ContinueWith()` 함수의 parameter는 아래의 그림과 같기 때문에 쓰던지 안쓰던지 상관은 없지만서도, named parameter를 사용하는 의미는 좀 더 모호성이 없는 전달을 위함이라고 생각됩니다.
  * 보기 쉽게 Code를 정렬하여 보자면 아래와 같습니다.

![ContinueWith\(\) &#xD568;&#xC218;&#xC758; parameter](../../../.gitbook/assets/image%20%28165%29.png)

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
  * 이 의미는 `ContinueWith()` 함수를 사용하여 해당 구문이 끝났을 때 실행될 내용을 등록하는 것입니다.

{% code title="AuthManager.cs" %}
```csharp
firebaseAuth.SignInWithEmailAndPasswordAsync().ContinueWithOnMainThread();
```
{% endcode %}

* `firebaseAuth.SignInWithEmailAndPasswordAsync().ContinueWithOnMainThread()`
  * 1. FirebaseAuth Class에 접근합니다.
    2. FirebaseAuth Class의 `SignInWithEmailAndPasswordAsync()`에 접근합니다.
    3. `SignInWithEmailAndPasswordAsync()` 함수는 Task Class로 구성되어 있기 때문에 Chaining할 Method를 등록해야 합니다.
  * 해당 함수는 Email이나, Password를 사용하여 Login을 가능하게 하는 함수입니다.
    * 똑같이 Task Class를 사용하기 때문에 Continue Task Method를 사용하여 작업이 완료되면 실행될 다음 Method를 추가합니다.
    * Google, Apple 계정을 이용하여 외부 서비스에서 Login이 가능하게 하는 함수인 `SignInWithCredentialAsync()` 함수도 있습니다.
  * `ContinueWithOnMainThread()`
    * 해당 함수는 Task Class의 `ContinueWith()` 함수와 동일하게 기능을 하지만, Main Thread에서 작동한다는 차이점을 가지고 있습니다.
    * Sample Project는 Unity Version에 따른 Error 때문에 사용했다고 합니다.
    * 하지만, 애초에 Task Class는 연속 작업의 비동기 처리를 위해 작성되있기 때문에, Thread를 이용하여 작업의 분산화를 시켰지만 처음에 시작하는 Thread인 MainThread에서 실행하는 것이 실행속도가 좀 더 빠르기 때문에, 해당 함수를 사용함에 따라 좀 더 신중하게 사용할 필요가 있을 듯 합니다.
      * 해당 부분은 OS에 관련된 영역이기 때문에 참고자료만 링크하겠습니다.

{% embed url="https://thecodingmachine.tistory.com/1" caption="Threading에 관련한 개념과 사례 및 구조를 설명한 개인적으로 추천하는 참고자료 입니다." %}
{% endtab %}
{% endtabs %}

## Photon 관련 함수

{% tabs %}
{% tab title="Photon은 어떤 역할인가?" %}
* Photon은 Server의 역할을 하기 때문에, 위에서 사용한 Firebase와는 다른 구조를 가지고 있습니다.
* Firebase에서는 Authentication\(인증\) 기능만을 사용했다면 여기서는 Photon에서 제공하는 API를 사용하여 플레이어 간의 동기화를 제공합니다.
{% endtab %}

{% tab title="LobbyManager Variable" %}
{% code title="LobbyManager.cs" %}
```csharp
MonoBehaviourPunCallbacks Class

PhotonNetwork.GameVersion = gameVersion;
```
{% endcode %}

* `MonoBehaviourPunCallbacks` : PUN2를 자동으로 감지할 수 있는 MonoBehaviour입니다.
  * 이를 사용하기 위해서는 기존의 MonoBehaviour를 상속받는 기본 Script의 Main Class를 MonoBehaviourPunCallbacks로 교체해야합니다.
  * 기존의 MonoBehaviour Class는 Unity가 기본적으로 상속받아야할 Class이지만, 이는 기본적으로 Unity 자체적으로 제공되기 때문에 Photon과는 맞지 않는 함수들이 많습니다.
  * 이러한 이유 때문에 Photon에서는 MonoBehaviourPunCallbacks이라는 Class를 제공함으로써 Photon을 사용할 수 있는 Class를 제공합니다.

-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

* `PhotonNetwork.GameVersion = gameVersion`
  * 우리가 접할 수 있는 Game에서는 Version이라는 것이 있습니다.
  * 이를 통해 새로운 Content들을 업데이트 하고, 이전 프로그램과 구분 짓기 위해서 만듭니다.
  * Photon에서는 사용자들을 호환되는 Version으로 그룹지을 수 있습니다.
  * 때문에, 이 GameVersion이 맞지 않는다면, 다른 그룹으로 간주하여, 멀티플레이가 안될 수 있습니다.
{% endtab %}

{% tab title="LobbyManager Method" %}
{% code title="LobbyManager.cs" %}
```csharp
1. PhotonNetwork.ConnectUsingSettings();
2. void OnConnectedToMaster()
3. void OnDisconnected(DisconnectCause cause)
4. PhotonNetwork.JoinRandomRoom();
5. void OnJoinRandomFailed(short returnCode, string message)
6. PhotonNetwork.CreateRoom(null, new RoomOptions { MaxPlayers = 2 });
7. void OnJoinedRoom()
8. PhotonNetwork.LoadLevel("Main");
```
{% endcode %}

* `PhotonNetwork.ConnectUsingSettings()` 
  * 이 함수는 Editor에서 설정한 PhotonServerSetting에 접근하여 Server/Cloud Settings의 항목을 검사하고 해당 함수가 끝나면 자동적으로 `OnConnectedToMaster()` 함수로 접근합니다.
* `void OnConnectedToMaster()`
  * Master Server에 접근할 수 있는 함수입니다.
  * Master Server에 연결이 되고, 인증이 되면 자동적으로 호출합니다.
    * 인증 부분은 `ConnectUsingSettings()` 함수로 PhotonSeverSetting 접근하여 검사합니다.
* `void OnDisconnected(DisconnectCause cause)`
  * 접속을 시도 했지만 실패한 경우 parameter를 통해 실패한 변수를 받습니다.
* `PhotonNetwork.JoinRandomRoom()`
  * Master Server에서의 인증 및 연결이 끝났기 때문에 GameServer에서 Room, Lobby를 생성합니다.
  * 이 함수는 GameServer에서 감지한 RandomRoom에 자동으로 접속하는 함수입니다.
  * 빈방이 없다면 자동적으로 실패하여 `OnJoinRandomFailed()` 함수를 실행합니다.
  * **맨 처음에는 방이 없기 때문에 의도적으로 이 함수에 접근하여 실패할 경우 방을 만들어 주는 행동을 작성합니다.**
* `void OnJoinRandomFailed(short returnCode, string message)`
  * 빈방이 없는 경우 자동적으로 실행하는 함수입니다.
  * `CPhotonNetwork.CreateRoom()` 함수를 통해 방을 생성합니다.
* `PhotonNetwork.CreateRoom()`
  * 이 함수는 주어진 이름으로 방을 생성하지만, 이미 있는 이름이라면 실패합니다.
  * 주어진 이름이 null이라면 Server가 대신 이름을 생성합니다.
  * Master Server에서만 호출할 수 있습니다.
  * **Lobby는 필요에 따라 자동적으로 생성됩니다.**
  * 필요에 따라 Option을 줄 수 있습니다.
* `void OnJoinedRoom()`
  * Room에 입장시 모든 Client에서 호출이 됩니다.
  * 입장을 했다면 Scene의 전환이 필요한데 이는 `PhotonNetwork.LoadLevel()` 함수를 이용하여 전환합니다.
* `PhotonNetwork.LoadLevel()`
  * Scene을 Load하는데 기존의 SceneManager을 사용하여 LoadLevel한다면 해당 Client에서만 전환되고, 다른 Client에서는 전환이 안되기 때문에, 해당 함수를 이용하여 Scene을 전환합니다.
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="GameManager Variable" %}
{% code title="GameManager.cs" %}
```csharp
1. PhotonNetwork.IsMasterClient
2. PhotonNetwork.LocalPlayer.ActorNumber
```
{% endcode %}

* `PhotonNetwork.IsMasterClient`
  * Master Server인지 확인하기 위한 PhotonNetwork Class의 bool type 변수입니다.
* `PhotonNetwork.LocalPlayer.ActorNumber`
  * 현재 방에 있는 플레이어를 식별하는 변수입니다.
  * 0번 index부터 시작하여 -1의 값은 객실 외부를 가리키고 있습니다.
{% endtab %}

{% tab title="GameManager Method" %}
{% code title="GameManager.cs" %}
```csharp
1. PhotonNetwork.Instantiate()
2. void OnLeftRoom()
3. [PunRPC]
   void RPCUpdateScoreText()
4. photonView.RPC()
```
{% endcode %}

* `PhotonNetwork.Instantiate()`
  * 네트워크 상에서 prefab Instance를 생성합니다.
    * 이 함수는 prefab을 생성하기 위해서는 Unity상에서 prefab을 가져오기 위해서는 무조건 경로상의 "Resources" 라는 폴더가 필요합니다.
    * "Resources" 폴더는 대소문자 틀림없이 그대로의 이름을 가져야 합니다.
* `void OnLeftRoom()`
  * Master Server가 아닌 Game Server에서 동작하는 함수입니다.
  * 나가는 플레이어에게만 실행됩니다.
* `[PunRPC] void RPCUpdateScoreText()`
  * `[PunRPC]`
    * 원격으로 Client의 함수를 호출하기 위해 해당 속성을 적용해야합니다.
      * 이때 Client는 전체 혹은 다른 특정 Client를 특정할 수 있습니다.
      * 이를 사용하기 위해서는 `PhotonView.RPC()` 함수를 이용해야 합니다.
    * 적용하기 위해 Attribute를 적용과 PhotonView Component를 사용해야 합니다.
  * `void RPCUpdateScoreText()`
    * 해당 함수는 PUN2 Class가 아닌 사용자 함수지만, PunRPC Attribute를 사용하여 원격으로 호출한다는 것을 알리기 위해 정리 했습니다.
    * Logic은 간단하게 string parameter를 2개 받아서 하나의 scoreText를 통해 점수를 출력합니다.
* `photonView.RPC()`
  * Room내의 모든 Client가 특정 Method를 호출하도록 합니다.
  * 해당 함수는 아래의 parameter를 가지고 지정한 target에게 전송합니다.
    * methodName : RPC Attribute를 가지고 있는 Method name 입니다.
    * target : target들의 그룹과 RPC가 전송되는 방법입니다.
    * parameters : RPC Attribute가 적용된 Method의 parameter들 입니다.
{% endtab %}

{% tab title="Ball Variable" %}
```text
1. PhotonNetwork.IsMasterClient
2. photonView.IsMine
3. PhotonNetwork.PlayerList
```

* `PhotonNetwork.IsMasterClient`
  * Master Client는 일반적으로 다른 Client와 동일하지만, 일반적으로 게임을 시작하는 Host의 역할을 가지고 있습니다.
  * 이러한 역할을 가진 MasterClient인지 아닌지를 확인하는 변수입니다.
* `photonView.IsMine`
  * bool type의 변수이며, true라면 Client를 제어할 수 있다는 것을 의미합니다.
  * PUN2에는 PhotonView를 제어 하는가에 대한 소유권 개념이 있기 때문에 존재하는 변수입니다.
* `PhotonNetwork.PlayerList`
  * 현재 Room에 있는 플레이어들의 목록 List 변수입니다.
{% endtab %}
{% endtabs %}

## Thread Class

* Task Class를 알기 위해서 Threading Class에 관하여 먼저 정리하겠습니다.

{% tabs %}
{% tab title="개요" %}
* 보통 인터넷을 하면서, 노래를 듣고, 동시에 작업을 할 수 있습니다. 이러한 작업들을 Process라고 합니다.
* 이러한 Process는 하나 이상의 Thread로 이루어지고, 독립적인 실행 경로이며, 다른 Thread와 동시에 실행할 수 있는 기능입니다.
* 단일 Thread를 실행하고 거기서 다른 Thread를 생성하고, 실행하여 Multi Thread를 가능하게 합니다.
* 즉, Thread는 Process를 여러개의 조각으로 나눈 것으로, Thread 변수를 통해 작업을 동시에 실행하는 것과 같은 효과를 가지게 합니다.

![Thread &#xACFC;&#xC815;](../../../.gitbook/assets/image%20%28160%29.png)
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

![Thread Example](../../../.gitbook/assets/image%20%28162%29.png)

* Run\(\) 함수의 작업을 Thread 변수를 가지고 Lambda식을 사용하여 Start\(\) 하면 위의 그림과 같이 출력이 됩니다.
* 출력 결과를 보면 Run 0, Run 1이 실행되는데 번갈아 가면서 실행하는데, 일정하게 반복해서 출력되는 것이 아니라 나눠서 실행되는 것을 확인할 수 있습니다.
{% endtab %}
{% endtabs %}

## Task Class

* Firebase의 Authentication\(인증\) 기능을 사용하기 위해 Firebase가 정상적으로 돌아가는지를 확인하기 위한 `ContinueWith()` 함수를 알기 위해서는 Task Class에 대해 알아야 합니다.
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

![Task Code &#xD750;&#xB984;&#xB3C4;](../../../.gitbook/assets/image%20%28163%29.png)
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

## 마치며

* Photon, Firebase, Task, Thread등을 사용하면서 할수 있는 영역이 넓어졌다고 느낀 Project입니다.
* DB나 Server를 사용한다는게 예전서부터 어려움을 느껴왔지만, 이번 Project를 정리하면서 내가 생각했던 SQL이나, Server를 작성하는 Code를 사용한다는 것에 대해 좀 더 접근성이 좋아졌다는 것을 느꼈습니다.

