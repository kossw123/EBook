# A B C D E F G

## A

## B

## C

## D

###  - Dispose Pattern

Coroutine에 Exception Handling을 할 때 생긴 issue이다.

1. Coroutine에서 Exception을 사용하여 예외 처리를 시도하려고 함
2. 그러나 Exception 내부에서 yield문의 선언이 문법적으로 불가
   1. CS1626 : catch문이 선언된 try 블록 선언문에서 값 생성 불가
3. 구글링을 통해 Coroutine Exception Handling 워드 발견
4. 해당 워드의 내용을 Interanl Coroutine을 사용하여 내부 Coroutine을 한번 더 돌리는 방식
5. 하지만 new\(\)를 사용하여 동적 할당을 해야하는 불편함이 존재
6. using Statement를 통해 객체를 일시적으로 생성하고 해제하는 방식\(Dispose\)을 발견
7. 
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
        
        /// using문은 다음과 같이 변환 가능
        /// DisposeClass d = new 
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



## E

## F

## G



