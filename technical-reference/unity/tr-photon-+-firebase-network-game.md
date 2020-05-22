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
  * 해당 함수는 Firebase에 필요한 모든 종속성이 시스템에 존재하는지, 필요한 상태인지, 비동기적으로 확인하고 그렇지 않는 경우에는 수정하려고 시도합니다.
    * 종속성?
      * **데이터의 구조가 프로그램 데이터 저장방식을 결정하고, 반대로 프로그램의 데이터 저장방식에 따라 데이터의 저장방식이 바뀌는 것을 의미합니다.**
      * **데이터 구조가 변경되면, 프로그램까지 같이 바뀌는 비용이 들기 때문에 , 프로그램 개발과 유지보수가 어려워지는 문제점이 있습니다.**
      * 여기서는 ContinueWith\(\) 함수의 Chaining CallbackMethod의 내용에 따라, 비동기 로 해당 내용을 실행합니다.
  * 
{% endtab %}
{% endtabs %}

## Photon 관련 함수



## Thread

* Task Class를 알기 위해서 Threading Class에 관하여 먼저 정리하겠습니다.

{% tabs %}
{% tab title="개요" %}
* 보통 인터넷을 하면서, 노래를 듣고, 동시에 작업을 할 수 있습니다. 이러한 작업들을 Process라고 합니다.
* 이러한 Process는 하나 이상의 Thread로 이루어지고, 독립적인 실행 경로이며, 다른 Thread와 동시에 실행할 수 있는 기능입니다.
* 단일 Thread를 실행하고 거기서 다른 Thread를 생성하고, 실행하여 Multi Thread를 가능하게 합니다.

![Thread &#xACFC;&#xC815;](../../.gitbook/assets/image%20%28160%29.png)

{% embed url="https://wergia.tistory.com/187" caption="Thread에 대한 개념" %}
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

## Task Class

* * 
{% tabs %}
{% tab title="개요" %}
* Task Class를 알기 위해서 Threading Class에 관하여 
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

