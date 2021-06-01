# A B C D E F G

## A

## B

### Burst Compiler의 사용

Burst Compiler은 Unity에서 LLVM을 사용하여 .NET Byte Code를 고도로 최적화된 Native Code로   
변환해주는 Compiler로써, Unity가 추구하는 DOTS\(Data - Oriented - Tech - Stack\)에 맞게 설계되어 있다.

DOTS은 다음과 같이 설명되어 있다.  
"데이터 지향 방식으로 프로그램을 짜는 코드 작성의 다른 패러다임"

그렇다면 데이터 지향 방식은 어떤 방식인가?

* Unity가 조금 익숙해지다 보면 GameObject를 제외하면서 짠다는게 너무 어렵다
* 그래서 줄인다고 해도 Player이외의 Data에 접근하기 위해 다양한 Component Class에 대한 접근이 필요했다.
* 이러한 접근 자체는 Compile Error가 주로 일어나서 상관없지만, Runtime에서의 Error가 문제가 되는 경우가 많다.
* 그래서 Profiler를 돌려보면 특별히 memory leak이 나거나 하는 부분을 찾을 수 없지만, 문제가 분명 존재한다.
* 이러한 문제의 대부분은  Random Memory Access Pattern\(무작위 메모리 접근 패턴 / RAM\),과  Cache Miss\(CPU에서 요청한 데이터가 Cache에 없는 경우\) 두 가지 경우가 많다고 한다.
* 그래서 데이터 지향 방식은 관점을 Data로 옮겨 다음과 같은 항목을 중심으로 짠다. Data의 타입, Data가 Memory에 어떻게 배치 되는가, 게임에서 어떻게 읽어서 처리할 것인가

{% embed url="https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=yilice&logNo=221548434909" %}





즉, 새로운 작성 패러다임으로 짜면, 기본 OOP에서 발생하는 문제점들을 해결하고 Memory에 대한 최적화를 이룰 수 있다는 이야기 같다.

물론 Unity에서 자신하는 기능인 만큼 속도가 무진장 빠르다.

Job System과 같이 사용되는 Burst Compiler에서는 다음과 같은 기능들을 제공하고 있다.

1. Safety Checks
2. Synchronous Compilation
3. Native Debug Mode Compilation
4. Show Timing

#### Safety Checks

사용하고 있는 Job을 감지하여 모든 잠정적인 Race Condition을 감지하고 그로 인해 발생하는 버그를 차단한다.

* Race Condition : 2개 이상의 경쟁 상태인 쓰레드들이 공유된 자원에 접근하려고 할 때,  동기화 메커니즘 없어 실행 순서를 동기화 하지 못해 결과가 달라지는 상

#### Synchronous Compilation

멀티쓰레드 프로그래밍의 대표적인 동기화 문제를 해결한다.

#### Native Debug Mode Compilation

* IDE에 포함된 Native Debugger를 사용하여 다음과 같은 작업을 할 수 있다.
  * 조건부 중단점을 비롯한 중단점 적중
  * 프레임 콜 스택 검사
  * 로컬 변수 검사
  * 구조체 및 데이터 검사, 포인터 추적
  * 디버거 감시 사용
* 그러나 아직은 시험적인 성격이 강해 다음과 같은 항목은 제한되어 있다고 한다.
  * 쓰레드 디버깅
  * 변수가 선언되어 사용될 수 있는 영역을 벗어나면 추적 불가
  * Foreach문에서 Lambda식을 쓰면 값이 제대로 추적이 안된다.
  * 함수의 인자를 넘길 때 값이 제대로 
  * 몇가지 명시적인 구조체가 지원이 안되서 값이 제대로 추적이 안된다.

#### Show Timing

z

## C

### Coroutine Exception Handling\(Nested Coroutine\)

#### 발단

1. Coroutine 내부에서 while문을 사용하는 중 자꾸 잘못된 조건식과 값을 넣는 느낌이 강하게 들었음
2. 생기는 오류가 대부분 Runtime에서 발생했기 때문에 Debuging이 힘들었음
3. 그래서 Debuging에 관한 문제를 해결하고자 구글링 시작

#### 전개

1. Coroutine + Exception이라는 워딩을 통해 구글링 중 제목과 같은 이름의 포스팅을 발견
   1. [http://www.zingweb.com/blog/2013/02/05/unity-coroutine-wrapper](http://www.zingweb.com/blog/2013/02/05/unity-coroutine-wrapper)



Coroutine Exception Handling의 개요

* Exception문에서 IEnumerator를 통한 yield 명령어가 삽입되지 않는걸 해결하고 이상적인 Coroutine을 만들고 싶다는 내용을 서술하고 있다.
* 여기서 말하는 이상적인 Coroutine이란 다음과 같은 상황을 이야기 하는 것 같다.

```csharp
void Start()
{
    try { StartCoroutine(ParentCoroutine()); }
    //can try...catch if there's no yielding
    catch (Exception e) {
        Debug.LogError("oh noes we had a problem: " + e.Message);
    }
}

IEnumerator ParentCoroutine()
{
    yield return StartCoroutine(NestedCoroutineDoSomeStuff());
    //can't try...catch these
    yield return StartCoroutine(NestedCoroutineFoo());
    //if this has an exception nothing below executes
    if (stuff)
        yield return StartCoroutine(NestedCoroutineBar());
}
```

* ParentCoroutine에서 NestedCoroutine들을 호출하고 있는데, 만약 NestedCoroutine중 하나가 예외를 발생 시킨다면, Parent는 동작을 멈춘다.
* NestedCoroutine에서 발생한 예외가 ParentCoroutine에 영향이 간다는 것은 불합리 하다.
* 이를 방지하기 위해 NestedCoroutine 내부에서 예외를 처리하게끔 작성한다.
* 이때 Coroutine들은 동시성\(Concurrency\)이라는 문제를 발생 시키기 때문에 Locking을 따로 처리해야한다.



#### 결말 

해당 포스팅의 내용은 "중첩된 Coroutine을 하나의 IEnumerator에서 제어할 경우" 생기는 문제점과 그에 대한 해결 방법을 포스팅했는데, 아직까지 중첩된 Coroutine을 사용하는 단계까진 아니더라도, 이로 인해 Dispose Pattern에 접근하고, System.Diagnostics, WeakReference 등과 같은 Class를 접하는 계기가 되었다.

하지만 중첩된 Coroutine을 사용하는 경우와 Generic Coroutine을 사용하여 반환할 정도로 가면 꽤나 커질 프로젝트의 규모를 생각하면 아찔하긴 하다.



### Coroutine Exception Handling\(Nested Coroutine 2\)

#### 발단

1. 위의 Nested Coroutine의 전체 코드가 있는 파일을 보다가 Single Coroutine Exception Handling과는 다른 방식으로 작성된 코드를 발견

#### 전개

1. 해당 포스팅에서는 Coroutine Generic Class를 가지고 객체를 생성 후 
2. Internal Coroutine으로 따로 실행하는 방법을 채택함

Coroutine Exception Handling\(Nested Coroutine 2\) 개요

* Generic Coroutine Class가 따로 존재
* static Class로 새로운 Coroutine 객체를 생성하여 Internal Coroutine을 돌림
* 실행되는 Coroutine은 다시 MonoBehaviour.StartCoroutine에서 실행
  * 실행되는 이유는 Generic Class에 MonoBehaviour.Coroutine이 존재하기 때문

{% tabs %}
{% tab title="Main" %}
```csharp
public class GenericCoroutine : MonoBehaviour
{
    private void Start()
    {
        StartCoroutine(Begin());
    }
    
    IEnumerator Begin()
    {
        var routine = MonoBehaviorExt.StartCoroutine<TypeICareAbout>(
                                        this, 
                                        TestNewRoutineGivesException());
        yield return routine.coroutine;

        /// try문에서는 반드시 예외를 발생 시키는 코드를 넣어야 한다.
        /// yield return문을 Exception 아래로 넣어서 실행시킨다면 Exception이 되지만
        /// 포스팅의 내용상으로는 yield return문이 Exception보다 위에 위치
        try
        {
            MessageHelper.ProcessClass(routine.Value);
        }
        catch (Exception e)
        {
            Debug.Log(e.Message);
            // do something
            Debug.Break();
        }
    }

    IEnumerator TestNewRoutineGivesException()
    {
        yield return null;
        yield return new WaitForSeconds(2f);
        throw new Exception("Bad thing!");
    }
}


public class Coroutine<T>
{
    public T Value
    {
        get
        {
            if (e != null)
                throw e;

            return returnVal;
        }
    }
    private T returnVal;
    private Exception e;
    public Coroutine coroutine;

    public IEnumerator InternalRoutine(IEnumerator coroutine)
    {
        while (true)
        {
            if (!coroutine.MoveNext())
            {
                yield break;
            }
            object yielded = coroutine.Current;

            if (yielded != null && yielded.GetType() == typeof(T))
            {
                returnVal = (T)yielded;
                yield break;
            }
            else
            {
                yield return coroutine.Current;
            }
        }
    }
}
```
{% endtab %}

{% tab title="Helper" %}
```csharp
public static class MessageHelper
{
    public static void ProcessClass(object target)
    {
        if(target == null)
        {
            throw new ArgumentNullException();
        }
        else
        {
            throw new ArgumentNullException();
        }
    }
}
```
{% endtab %}

{% tab title="static" %}
```csharp
public static class MonoBehaviorExt
{
    public static Coroutine<T> StartCoroutine<T>(this MonoBehaviour obj, 
                                                IEnumerator coroutine)
    {
        Coroutine<T> coroutineObject = new Coroutine<T>();
        coroutineObject.coroutine = obj.StartCoroutine(
                                        coroutineObject.InternalRoutine(coroutine));
        return coroutineObject;
    }
}
```
{% endtab %}
{% endtabs %}

#### 결말

아직까진 Generic까지 써서 Coroutine의 returnVal값을 뭘 넘길지는 모르겠지만, 구조 자체는 Generic을 사용할 때 유용하게 사용될 듯하다.

현재 static Class에서 object parameter를 가지고, T type이 Value type이 아닌 Reference type일 때 접근하는 방식과 또 다르게 적용할 수 있는 방식을 찾는 중  




### Coroutine Exception Handling\(Single\)

#### 발단

1. 위의 Coroutine Exception Handling을 처리하는 와중에 다른 포스팅을 통해 발견한 자료와 여러가지 Coroutine 제작 방식을 조합

#### 전개

1. 위의 Coroutine Exception Handling은 "Nested Coroutine에서 Child Coroutine에서의 예외 처리 및 Locking 처리"라고 한다면, 이 문단은 그냥 단순하게 하나의 Corotuine에서 Exception 및 Dispose를 하는 과정이다.
2. Class로 Data들을 묶기 때문에 동적 할당해야한다는 단점이 존재하지만, 나름 보기 깔끔한거 같아서 정리하고 개선중이다.

Single Coroutine Exception Handling

{% tabs %}
{% tab title="Main" %}
```csharp
public class Main : MonoBehaviour
{
    void Start()
    {
        using(SampleCoroutine s = new SampleCoroutine())
        {
            Debug.Log("대괄호 벗어나면 Dispose");
        }
    }
    
    IEnumerator target()
    {
        Debug.Log("Target Call!!");
        yield return null;
    }
}

public class SampleCoroutine : IDisposable
{
    public object result;
    public Coroutine coroutine;
    
    private IEnumerator target;
    Exception e;
    bool lockable = false;

    public SampleCoroutine(MonoBehaviour owner, IEnumerator target)
    {
        this.target = target;
        this.coroutine = owner.StartCoroutine(InternalCoroutine());
    }
    
    ~SampleCoroutine () => Dispose(false);
    
    public IEnumerator InternalCoroutine()
    {
        lockable = true;
        while(lockable)
        {
            try
            {
                if(!target.MoveNext())
                {
                    lockable = false;
                    Dispose(false);
                    yield break;
                }
            }
            catch(Exception e)
            {
                this.e = e;
                HelperClass.TraceMessage("Finalizer!!");
                Dispose(false);
                yield break;
            }
            
            result = target.Current;
            yield return result;
        }
    }
    
    
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
    
    public void Dispose(bool isDisposing)
    {
        if(lockable) { return; }

        if(isDisposing)
        {
            result = null;
            coroutine = null;
            target = null;
            e = null;
            lockable = false;
        }

        lockable = true;
    }
}
```
{% endtab %}

{% tab title="Helper" %}
```csharp
public class HelperClass
{
    public static void TraceMessage(
    string message,
    [CallerMemberName]string memberName = "",
    [CallerFilePath]string sourceFilePath = "",
    [CallerLineNumber]int sourceLineNumber = 0
    )
    {
        UnityEngine.Debug.Log("message: " + message);
        UnityEngine.Debug.Log("member name: " + memberName);
        UnityEngine.Debug.Log("source file path: " + sourceFilePath);
        UnityEngine.Debug.Log("source line number: " + sourceLineNumber);
    }
}
```
{% endtab %}
{% endtabs %}



#### 결말 

아래의 방법들이 사용됨

* Coroutine Exception Handling\(Nested Coroutine\)에서 포스팅된 내용을 바탕으로 Dispose와 Exception Message의 내용
* 호출 스택을 출력하는 TraceMessage\(\)
* using statement
* Dispose Pattern 구

위와 같은 방법들로 나름대로 코드를 리팩토링함과 동시에 최적화를 해봤지만, 여러가지 논리적 허점이 보인다.

1. using statement를 사용하여 객체를 잠깐 생성했다가 회수하는데, 이게 효율적인가?
2. Field가 너무 비효율적인 Value를 가진다고 생각된다. 코드에서는 Field를 다 사용하지만 감소 시킬 여력이 있다고 느낌이 들지만, 해결방법은 딱히 생각나지 않는다.
3. Single Coroutine인데 Nested Coroutine에서 사용할 lockable 변수는 딱히 필요없지 않을까?

## D

### Dispose Pattern

#### 발단

1. Coroutine에서 Exception을 사용하여 예외 처리를 시도하려고 함
2. 그러나 Exception 내부에서 yield문의 선언이 문법적으로 불가
   1. CS1626 : catch문이 선언된 try 블록 선언문에서 값 생성 불가
      1. Exception 자체가 yield return이라는 개체 생성\(return\) 후 MoveNext\(\)의 과정이 부적

#### 전개

1. 구글링을 통해 Coroutine Exception Handling 워드 발견
2. 해당 워드의 내용을 Interanl Coroutine을 사용하여 내부 Coroutine을 한번 더 돌리는 방식
3. 하지만 new\(\)를 사용하여 동적 할당을 해야하는 불편함이 존재
4. using Statement를 통해 객체를 일시적으로 생성하고 해제하는 방식\(Dispose\)을 발견



Dispose Pattern의 개요

* GC는 Heap에 할당된 리소스를 회수하는데 이때 시점이 MS에서 조차 장담하지 못한다고 한다.
* 그렇기 때문에 C\#에서는 Dispose라는 interface로 해제 할 수 있는 방식이 존재

```csharp
public class Main
{
    void Start()
    {
        using(DisposeClass d = new DisposeClass())
        {
            // 대괄호를 벗어나면 자동으로 객체의 Dispose를 호출하여 메모리 관
        }
        
        /// using문은 다음과 같이 치 가능
        /// DisposeClass d = new Dispose();
        /// try
        /// {
        ///     
        /// }
        /// finally
        /// {
        ///    if(d != null)
        ///         Dispose();
        /// }
    }

}



public class DisposeClass : IDisposable
{
    ...
    Field
    ...
    
    bool lockable = false;

    ~DisposeClass() => Dispose(false);
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
    
    public void Dispose(bool isDisposing)
    {
        if(lockable) { return; }
        
        if(isDisposing)
        {
            /// *** CLR에서 관리되는 모든 managed Resources들을 해제
            /// ...
            /// Field value is Null
            /// ...
        }
        
        lockble = true;
    }
}

```

#### 결말 

Exception Handling과는 별개로 Dispose를 통해 객체의 Resources 해제를 한다는 점이 자료를 찾으면서 굉장히 감탄했다.

이와 별개로 Microsoft.Win32.SafeHandles Class의 SafeHandle을 사용하여 구현하는 방법도 있다.

Dispose 자료를 찾으면서 알아낸 주의점

* Dispose는 프로그래머가 하나하나 호출하고 CLR에서 관리되는 managed Resources들을 명시적으로 해제하기 때문에 굉장히 귀찮을 수도 있지만, 배치만 잘하면 TestCase 이상으로 편해질 수 있는 양날의 검과 같이 느껴졌음
* Dispose를 한다고 해서 프로그래머가 모든 managed Resources들을 해제 하려고 하는 것은 하지 말기.
* 행여나 Finalizer를 구현하지 않는다면, 리소스 낭비가 되는 경우도 있다고 함

Advanced Dispose Pattern이 있다고 하는데, Unmanaged Resources들 까지 Dispose가 된다고 한다.



## E

## F

## G







