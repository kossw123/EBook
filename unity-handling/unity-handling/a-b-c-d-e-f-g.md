# A B C D E F G

## A

## B

## C

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

#### 결

애초에 Exception부분에서 검사식만을 넣고 yield는 Exception 밖에서 사용하면 끝이지만, Coroutine Exception Handling이라는 워드와 Internal Coroutine과 같이 Class로 Data를 넘기는 방식을 알게됨

정리하니 쓸데가 없어 보이지만 Coroutine Class에서 Field로 뭘 넘길지에 대한 고민과 이유를 찾아낸다면 중요한 워드중 하나라고 생각함



## E

## F

## G



