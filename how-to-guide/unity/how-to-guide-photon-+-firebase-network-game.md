---
description: How-to-guide Photon + FireBase Network Game
---

# How-to-guide Photon + FireBase Network Game

## 무엇을 하려고 하는가?

* Photon, Firebase를 사용하는 Code에 대한 설명을 합니다.
* Script에 사용된 Lambda expression, Task Class, Threading과 같은 C\# 문법에 대한 설명은 해설문서\(Explanation\)에 기재하도록 하겠습니다.
* 해당 문서에서는 tutorial에서의 정보를 좀 더 다듬는 데에 의의를 두고 있습니다.

## AuthManager

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

