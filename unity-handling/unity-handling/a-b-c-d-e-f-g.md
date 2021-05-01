# A B C D E F G

## A

## B

## C

### Coroutine Exception Handling

#### 발단

1. Coroutine 내부에서 while문을 사용하는 중 자꾸 잘못된 조건식과 값을 넣는 느낌이 강하게 들었음
2. 생기는 오류가 대부분 Runtime에서 발생했기 때문에 Debuging이 힘들었음

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
            /// CLR에서 관리되는 모든 managed Resources들을 해제
            /// ...
            /// Field value is Null
            /// ...
        }
        
        lockble = true;
    }
}

```

#### 결말 

애초에 Exception부분에서 검사식만을 넣고 yield는 Exception 밖에서 사용하면 끝이지만, Coroutine Exception Handling이라는 워드와 Internal Coroutine과 같이 Class로 Data를 넘기는 방식을 알게됨

정리하니 쓸데가 없어 보이지만 Coroutine Class에서 Field로 뭘 넘길지에 대한 고민과 이유를 찾아낸다면 중요한 워드중 하나라고 생각함



## E

## F

## G



