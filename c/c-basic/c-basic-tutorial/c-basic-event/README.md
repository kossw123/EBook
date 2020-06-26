---
description: 'C# Basic Event'
---

# C\# Basic Event

## 무엇을 하려고 하는가?

* C\#에서 도입된 Event라는 Keyword에 대한 개념과 사용방법 및 사례에 대해 기술합니다.



## Event란?

* 어떠한 문제가 발생하면 Event는 관련된 모든 구성 요소에게 문제가 발생했음을 알리고, 상태가 변화했다면 상태가 변화했음을 알리는 기능입니다. 
* .NET의 Event는 Observer pattern을 따르는 keyword이기 때문에, Event를 발생시키는 Class\(Publisher\)와 알림을 받는 Class\(Subscriber\)로 구성됩니다.
* Publisher Class와 Subscriber Class 각각에 EventHandler라는 시스템에서 정의된 delegate를 가지고 Event에 대한 정보를 주고 받습니다.

![](../../../../.gitbook/assets/image%20%28217%29.png)

{% embed url="https://docs.microsoft.com/ko-kr/dotnet/csharp/events-overview" %}

{% embed url="https://www.tutorialsteacher.com/csharp/csharp-event" %}



## Event의 사용방법

1. delegate를 선언합니다.
2. Event Keyword를 사용하여 delegate의 변수를 선언합니다.
3. 등록할 함수를 delegate의 형식과 똑같이 구현합니다.
4. Publisher Class의 객체를 생성하여 Event변수에 등록할 함수를 등록합니다.
5. Event를 발생시킵니다.

```csharp
using System;

namespace ConsoleApp2
{
    // 1. delegate를 선언합니다.
    public delegate void Notify();  // delegate

    public class ProcessBusinessLogic
    {
        // 2. Event keyword를 이용하여 delegate 변수를 선언합니다.
        public event Notify ProcessCompleted; // event
        
        // 5. Event를 발생시킵니다.
        public void StartProcess() {
            Console.WriteLine("Process Started!");
            // some code here..
            OnProcessCompleted();
        }

        protected virtual void OnProcessCompleted() //protected virtual method
        {
            //if ProcessCompleted is not null then call delegate
            // 5. Event를 발생시킵니다.
            // 해당 내용은 ProcessCompleted가 Null이 아니면 .(Dot)연산자 다음 함수를 실행
            // Null이면 null을 반환
            ProcessCompleted?.Invoke();
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // 4. Publisher Class 객체를 생성
            ProcessBusinessLogic bl = new ProcessBusinessLogic();
            // 4. Event 변수에 함수를 등록
            bl.ProcessCompleted += bl_ProcessCompleted;
            bl.StartProcess();
        }
        // 3. 등록할 함수를 delegate의 형식과 똑같이 구현합니다.
        static void bl_ProcessCompleted()
        {
            Console.WriteLine("ProcessCompleted");
        }
    }
}

```



## 내장된 EventHandler delegate 사용

* .NET에서는 표준 Event 작성방법을 제시합니다.

{% embed url="https://docs.microsoft.com/ko-kr/dotnet/csharp/event-pattern" %}

* 
