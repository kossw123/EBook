---
description: How-to-guide Photon + FireBase Network Game
---

# How-to-guide Photon + FireBase Network Game



{% tabs %}
{% tab title="Start\(\)" %}

{% endtab %}
{% endtabs %}

## 무엇을 하려고 하는가?

* Photon, Firebase를 사용하는 Code에 대한 설명을 합니다.
* Script에 사용된 Lambda expression, Task Class, Threading과 같은 C\# 문법에 대한 설명은 해설문서\(Explanation\)에 기재하도록 하겠습니다.
* 해당 문서에서는 tutorial에서의 정보를 좀 더 다듬는 데에 의의를 두고 있습니다.

## AuthManager.cs

{% tabs %}
{% tab title="Variable" %}
* AuthManager는 FireBase에 대한 연결과, 동작을 담당하는 Script입니다.
* 해당 부분에서는 연결과 동작을 담당하는 Object를 선언하는 부분입니다.

{% code title="AuthManager.cs" %}
```csharp
public bool IsFirebaseReady { get; private set; }
public bool IsSignInOnProgress { get; private set; }

public InputField emailField;
public InputField passwordField;
public Button signInButton;

public static FirebaseApp firebaseApp;
public static FirebaseAuth firebaseAuth;

public static FirebaseUser User;
```
{% endcode %}

* `IsFirebaseReady` : Firebase가 준비여부에 대한 Property 변수입니다.
* `IsSignInOnProgress` : 이미 직전에 로그인시도가 현재 진행중인 경우에 대
* `emailField`, `passwordField`, `signInButton` : SignIn Scene에서의 UI Component 입니다.
* `firebaseApp` : firebase 전체를 관리하는 Object입니다.
* `firebaseAuth` : firebase에서의 authentication을 관리하는 Object입니다.
* `User` : firebaseAuth를 통해 가져온 정보를 InputField의 정보와 비교하여 같은 정보를 담는 Object입니다.
{% endtab %}

{% tab title="Start\(\)" %}
* Script 시작과 동시에 실행되는 함수입니다.
* 해당 함수에는 Firebase에 대한 비동기 인증을 CheckAndFixDependenciesAsync\(\) 함수를 통해 검사하여 준비가 되었다면 FirebaseApp의 Instance를 가져와서 해당 Script의 공간에 할당합니다.

{% code title="AuthManager.cs" %}
```csharp
public void Start() {
    signInButton.interactable = false;

    // other : FirebaseApp.CheckAndFixDependenciesAsync().ContinueWithMainThread()
    FirebaseApp.CheckAndFixDependenciesAsync().ContinueWith(
        continuationAction: task => { var result = task.Result;
            if(result != DependencyStatus.Available) {
                Debug.Log(result.ToString());
                IsFirebaseReady = false;
            }
            else {
                IsFirebaseReady = true;
                firebaseApp = FirebaseApp.DefaultInstance;
                // other : firebaseAuth = FirebaseAuth.GetAuth(firebaseApp);
                firebaseAuth = FirebaseAuth.DefaultInstance;
            }
            signInButton.interactable = IsFirebaseReady;
        }
    );
}
```
{% endcode %}

* `FirebaseApp.CheckAndFixDependenciesAsync().ContinueWith()` 
  * `CheckAndFixDependenciesAsync()`
    * FireBase를 구동할 수 있는 환경인지 검사합니다.
    * Async\(비동기\)이기 때문에 확인과 동시에 다음 Code로 넘어갑니다.
  * `ContinueWith()`
    * CheckAndFixDependenciesAsync\(\) 함수가 끝나는 타이밍에 이어서 실행할 Chaining을 연결하는데 쓰이는 구문입니다.
    * Firebase가 아닌 System.Task Data Type을 가지고 있습니다.
    * 이에 대한 설명은 해설문서\(Explanantion\)에 기재하겠습니다.
* `DependencyStatus.Available` : Firebase의 동작 상태를 enum문에 담아 현재 동작상태를 반환합니다.
* firebaseApp = FirebaseApp.DefaultInstance
  * FirebaseApp의 Instance를 가져오는 부분입니다.
* firebaseAuth = FirebaseAuth.DefaultInstance
  * FirebaseAuth의 Instance를 가져옵니다.
{% endtab %}

{% tab title="SignIn\(\)" %}
* SignIn\(\) 함수가 실행되면 각 UI에 대한 반응과, FirebaseAuth의 SignInWithEmailAndPasswordAsync\(\) 함수를 이용하여 InputField에 입력한 ID, Password 비교합니다.

{% code title="AuthManager.cs" %}
```csharp
public void SignIn() {
    if (!IsFirebaseReady || IsSignInOnProgress || User != null) {
        return;
    }
    IsSignInOnProgress = true;
    signInButton.interactable = false;
    firebaseAuth.SignInWithEmailAndPasswordAsync(emailField.text, passwordField.text).ContinueWithOnMainThread(continuation : task => {
        Debug.Log(message : $ "Sign in Status : { task.Status}");
        IsSignInOnProgress = false;
        signInButton.interactable = true;
        if (task.IsFaulted) {
            Debug.LogError(task.Exception);
        }
            Debug.LogError(message : "Sign-in Canceled");
        } else {
            User = task.Result;
            Debug.Log(User.Email);
            SceneManager.LoadScene("Lobby");
        }
    });
}
```
{% endcode %}

* `firebaseAuth.SignInWithEmailAndPasswordAsync(emailField.text, passwordField.text).ContinueWithOnMainThread()`
  * `SignInWithEmailAndPasswordAsync()` 
    * email이나 패스워드를 사용해 로그인 가능하게 하는 함수입니다.
    * 이 또한 Asyn이기 때문에 확인과 동시에 다음 구문으로 넘어가기 때문에 Chaining할 함수를 등록해야합니다.
  * `ContinueWithOnMainThread()`
    * 앞에서 썼던 `ContinueWith()` 와는 비슷한 기능이지만 MainThread에서 동작합니다.
* `task.IsFaulted` : `ContinueWithOnMainThread()` 함수에서 나온 결과\(task\)가 제대로 동작하지 않는 경우입니다.
* `task.IsCanceled` : `ContinueWithOnMainThread()` 함수에서 나온 결과\(task\)가 도중에 Cancel되는 경우입니다.
{% endtab %}
{% endtabs %}

## LobbyManager.cs

{% tabs %}
{% tab title="Variable" %}
{% code title="LobbyManager.cs" %}
```csharp
private readonly string gameVersion = "1";

public Text connectionInfoText;
public Button joinButton;
```
{% endcode %}

* `gameVersion` : Photon Server의 GameVersion을 설정하기 위한 변수입니다.
{% endtab %}

{% tab title="Start\(\)" %}
* Start\(\) 함수에서는 PhotonNetwork Class를 사용하여, Lobby에서 PUN2에 대한 설정을 합니다.

```csharp
private void Start()
{
    PhotonNetwork.GameVersion = gameVersion;
    PhotonNetwork.ConnectUsingSettings();

    joinButton.interactable = false;
    connectionInfoText.text = "Connecting to Master Server...";
}
```

* `PhotonNetwork.GameVersion` : Photon Unity Network에서 돌아가기 위해 GameVersion이 필요한데 이를 설정하기 위한 Code입니다.
* PhotonNetwork.ConnectUsingSettings\(\) : PhotonNetwork에 대한 연결 설정 함수를 불러옵니다.
{% endtab %}

{% tab title="OnConnectedToMaster\(\)" %}
* Master Server\(Photon Cloud Server이자 Match Making을 위한 Server\)에 접근할 수 있는 함수입니다. 
* 기본적으로 PUN2 Package의 Class이며, 해당함수만 쓴다면 자동으로 Master Server에 접근합니다.

{% code title="LobbyManager.cs" %}
```csharp
public override void OnConnectedToMaster() {
    joinButton.interactable = true;
    connectionInfoText.text = "Online : Connected to Master Server";
}
```
{% endcode %}
{% endtab %}

{% tab title="OnDisconnected\(\)" %}
* OnDisconnected\(\) 함수는 PUN2를 Import하면 기본적으로 설치되는 패키지의 Class 함수입니다.
* PUN2는 Server에 대한 접속을 PhotonNetwork.ConnectUsingSettings\(\) 함수를 통하여 실행합니다.

{% code title="LobbyManager.cs" %}
```csharp
public override void OnDisconnected(DisconnectCause cause) {
    joinButton.interactable = false;
    connectionInfoText.text = $"Offline : Conntected Disabled {cause.ToString()}";

    PhotonNetwork.ConnectUsingSettings();
}
```
{% endcode %}
{% endtab %}

{% tab title="Connect\(\)" %}
* 해당 함수는 자체 함수이며, Join Button을 눌렀을 때 실행되는 함수입니다.
* PhotonNetwork Class 함수를 이용해 상황에 따른 Code를 실행합니다.

{% code title="LobbyManager.cs" %}
```csharp
public void Connect() {
    joinButton.interactable = false;
    if (PhotonNetwork.IsConnected) {
        connectionInfoText.text = "Connecting to Random Room...";
        PhotonNetwork.JoinRandomRoom();
    } else {
        connectionInfoText.text = "Offline : Conntected Disabled - Try ReConnecting..";
        PhotonNetwork.ConnectUsingSettings();
    }
}
```
{% endcode %}

* PhotonNetwork.IsConnected : Network에 연결 되었을 경우의 bool 변수를 반환합니다.
* PhotonNetwork.JoinRandomRoom\(\)
  * Match making Server에서 감지한 RandomRoom에 자동으로 접속하는 함수입니다.
  * 빈방이 없다면 자동적으로 실패하여 OnJoinRandomFailed\(\) 함수를 실행합니다.
{% endtab %}

{% tab title="OnJoinRandomFailed\(\)" %}
* 함수 자체는 Room Join에 실패할 경우 자동으로 실행되는 함수입니다.
* 그 안에 상황에 따른 Room 생성 Code를 넣습니다.

{% code title="LobbyManager.cs" %}
```csharp
public override void OnJoinRandomFailed(short returnCode, string message) {
    connectionInfoText.text = "There is no empty Room, Creating new Room";
    PhotonNetwork.CreateRoom(null, new RoomOptions { MaxPlayers = 2 });
}
```
{% endcode %}

* PhotonNetwork.CreateRoom\(\) : PhotonNetwork Class가 제공하는 Room 생성 함수입니다.
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="OnJoinedRoom\(\)" %}
* Player가 방에 들어온 경우 실행되는 함수입니다.
* 방에 들어온 경우 다음 Scene인 Main으로 넘어가야 하기 위한 함수입니다.

{% code title="LobbyManager.cs" %}
```csharp
public override void OnJoinedRoom()
{
    connectionInfoText.text = "Connected with Room";
    PhotonNetwork.LoadLevel("Main");
}
```
{% endcode %}

* `PhotonNetwork.LoadLevel()` : 보통 Scene을 Load할때는 SceneManager를 통해 하지만, Multiplay의 경우 다른 Client와 동기화를 위해 해당 함수를 사용하여 Scene을 Load합니다.
{% endtab %}
{% endtabs %}



