---
description: TR Photon + FireBase Network Game
---

# TR Photon + FireBase Network Game - 작성중

## 무엇을 하려고 하는가?

* Script에 사용된 Photon, Firebase에 관련한 함수 및 기능들을 보며, Code에 대한 이해를 돕습니다.

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
{% endtab %}
{% endtabs %}

## Photon 관련 함수



## 

