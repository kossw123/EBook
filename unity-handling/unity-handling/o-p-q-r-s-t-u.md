# O P Q R S T U

## O 

## P 

## Q 

## R 

## S 

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

