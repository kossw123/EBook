# H I J K L M N

## H 

## I 

## J 

### Job System의 사용

Unity 내부에서 Job System을 사용하여 멀티스레드 프로그래밍을 하는데  
C\#에서는 Threading, Task Class를 사용하여 멀티스레드 프로그래밍을 작성해야 한다.

Unity는 C++ 기반의 엔진을 C\# 기반의 코드로 변환하여 제공하기 때문에 Threading, Task Class를 사용할 수 있지만, 여러가지 예외 처리를 하던지 후작업이 필요하기 때문에,  
Unity에서는 Burst Compiler를 제공하여 여러가지 후작업에 대한 처리를 한다.

여러 상황에 대해서 처리를 대신 해주는 대신에 사용법이 정해져 있다.

1. Execute에서 실행할 동작을 작성한다.
2. Start\(\)나 Update\(\)같은 익숙하게 Unity엔진에 명령을 내릴 수 있는 Script Cycle에서 Job을 instance화 한다.
3. 생성한 Job Instance 뒤에 .\(Dot\)으로 Schedule 함수를 호출하면 미리 작성한 Execute가 실행된다.

{% tabs %}
{% tab title="간단 사용" %}
```csharp
using Unity.Burst;
using Unity.Collections;
using Unity.Jobs;

public class JobSample: MonoBehaviour
{
    struct Fundamental : IJob
    {
        public float _value;
        public void Execute()
        {
            Debug.Log("Execute");
        }
    }

    void Start()
    {
        var job = new Fundamental().Schedule();
    }
    /// Console 실행 결과 : Excute
}
```
{% endtab %}

{% tab title="" %}
```csharp
using Unity.Burst;
using Unity.Collections;
using Unity.Jobs;

public class JobSample: MonoBehaviour
{
    struct Fundamental : IJob
    {
        public void Execute()
        {
            Debug.Log("Execute");
        }
    }

    void Start()
    {
        var job = new Fundamental().Schedule();
    }
    /// Console 실행 결과 : Excute
}
```
{% endtab %}
{% endtabs %}

몇가지 확실하게 된 것이 있다.

1. Execute를 호출하려면 Schedule을 호출한다.
2. Job의 Schedule\(\)을 호출하면 JobHandle type을 반환한다.
3. 
## K 

## L 

## M 

## N

