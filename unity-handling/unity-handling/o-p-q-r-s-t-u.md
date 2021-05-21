# O P Q R S T U

## O 

## P 

## Q 

## R 

## S 

### Struct Instancing

#### 발단

1. Job System을 적용하여 GPU instancing 작업을 할 때, 새로운 struct instancing 방식을 봤다.
2. 기존에 struct의 instancing 방식이 굉장히 구리다는 것을 알게됨

#### 전개

두 가지 방법을 봤는데 JobSystem을 적용한 구조체 선언문으로 예시 적용

1. new와 동시에 대괄호 안에서 struct Field를 초기화
2. 함수의 자료형을 구조체 타입으로 하고 return new와 동시에 대괄호 안에서 struct Field를 초기화

```csharp
using Unity.Burst;
using Unity.Collections;
using Unity.Jobs;

public class Main : MonoBehaviour
{
    struct Paragraph
    {
        public Vector3 direction;
        .... Field 작성
    }

    struct JobHandle : IJobFor
    {
        public float delta;
        public float scale;
        
        public NativeArray<Paragraph> parts
        
        public void Excute(int index)
        {
            
        }
    }


    void Start()
    {
    
}
```

## T 

### Tracing Message

#### 발단

1. Exception Handling을 할 때 여러가지 시도를 하는 도중 Debuging을 통해 Error Message와 호출되는 Stack이 모두다 호출되길 원했다.



#### 전개 

1. 그래서 다른 것을 찾다가 System.Runtime.CompilerServices의 Caller Attribute를 발견하게 
2. 처음에는 System.Diagnostics.StackFrame을 사용하여 해당 Stack의 정보중 LineNumber를 호출했다
3. 그러나 StackFrame의 동적 생성과 StackFrame.GetFileLineNumber\(\)가 굉장히 사용하기 어려웠다.
4. 그래서 다른 것이 있나 구글링 하던 와중 System.Runtime.CompilerServices Class의 Caller Attribute를 발견

System.Runtime.CompilerServices Class Caller Attribute의 사용

```csharp
public void TraceMessage(string message,
        [System.Runtime.CompilerServices.CallerMemberName] string memberName = "",
        [System.Runtime.CompilerServices.CallerFilePath] string sourceFilePath = "",
        [System.Runtime.CompilerServices.CallerLineNumber] int sourceLineNumber = 0)
        {
            UnityEngine.Debug.Log("message: " + message);
            UnityEngine.Debug.Log("member name: " + memberName);
            UnityEngine.Debug.Log("source file path: " + sourceFilePath);
            UnityEngine.Debug.Log("source line number: " + sourceLineNumber);
        }
```

* 간단하게 parameter에 Attribute를 붙여서 호출하면 자동으로 Complier가 지정한 내용을 붙여준다.



#### 결말

Debuging을 통해 한층 더 간편해짐을 체감 해버렸다.

## U

### UI Action Order

#### 발단

1. UI를 작성하는데 Menu Panel을 모두 검색해서 UI Manager에서 처음에 열려야 할 Menu Panel을 빼고 닫는 코드를 작성
2. 하지만 A Menu Panel의 GameObject가 비활성화면 A가 검색이 되지 않는 상황이 발생
3. Resources.FindObjectsOfTypeAll\(\)을 통해 비활성화된 Object까지 모두 검색
4. 하지만 코드가 기능 단위로 쪼개지면서 UI에서 생성된 Method가 많아짐
5. 관리는 되지만, 코드 위치를 찾아야 하는 번거로움이 존재

#### 전개 

1. 그래서 함수명에 순서를 정하고 관리하기로 함
2. 그러나 함수 이름을 통해 관리 한다는게 너무 터무니 없이 찜찜했기에 Event 형식으로 Action을 등록하여 Event를 호출해서 관리하기로 경로를 바꿈

Coroutine으로 Event에 등록된 Action을 호출

{% tabs %}
{% tab title="Main" %}
```csharp
using System;

public class Main : MonoBehaviour
{
    public static Main instance;
    public static event RegisterAction Events;
    public delegate void RegisterAction();

    void Awake()
    {
        instance = this;
    }    
    
    void Start()
    {
        Helper helper = new Helper();
        StartCoroutine(helper.MainEvent());
    }
}
```
{% endtab %}

{% tab title="Helper" %}
```csharp
public class Helper
{
    public IEnumerator MainEvent(Main.instance.Events events)
    {
        events += Bahviour_First;
        events += Bahviour_Second;
        events += Bahviour_Third;
        
        yield return null;
        
        if(events != null)
        {
            events.Invoke();
        }
        else
        {
            Debug.LogError("events is null");
        }
        
    }
    
    public Behaviour_First()
    {
        // 첫번째 동작
    }
    
    public Behaviour_Second()
    {
        // 두번째 동작
    }
    
    public Behaviour_Third()
    {
        // 세번째 동작
    }
}
```
{% endtab %}
{% endtabs %}

* Main Class는 Singleton으로 만들어서 Helper에서 parameter로 등록할 수 있게끔 하고
* Helper Class의 IEnumerator를 Main Class에서 Coroutine을 시작한다.
* 이렇게 하는 이유는 한 프레임 정도 띄워 놓고, 혹시 모를 Script Excution Order를 확보하기 위함이다.

#### 결말 

원래는 Event Pooling을 만드려고 했지만, 도저히 Unity Architecture로 하는 방법을 찾을 수 없었기 때문에, C\# Architecture로 만들기로 결정한 다음 작업한 내용이다.

위의 내용처럼 지식이 부족한 상태에서 함수의 순서를 정해놓고 실행시킨다는 것이 될거 같다는 느낌이지만, 그야 말로 지식이 부족한 상태기 때문에 판단을 내리기 어려운 상태.

혹시나 하는 마음에 올린 포스팅

