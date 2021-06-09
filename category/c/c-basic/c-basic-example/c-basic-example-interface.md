---
description: 'C# Basic Example interface'
---

# C\# Basic Example interface

## 무엇을 하려고 하는가?

* interface 관련 코드를 작성하고, 그에 따른 Review를 합니다.

```csharp
interface IStore
{
    public void Read();
    void Write();
}

interface ICompress
{
    void Compress();
    void Decompress();
}

public class Document : IStore, ICompress
{
    #region IStore Explicit Implementation

    void IStore.Read()
    {
        Console.WriteLine("Executing Document's Read Method for IStore");
    }

    void IStore.Write()
    {
        Console.WriteLine("Executing Document's Write Method for IStore");
    }

    #endregion // IStore
    #region ICompress Implicit Implementation

    public void Compress()
    {
        Console.WriteLine("Executing Document's Compress Method for ICompress");
    }
    public void Decompress()
    {
        Console.WriteLine("Executing Document's Decompress Method for ICompress");
    }
    #endregion // ICompress
}
```

* interface의 사용과 더불어 명시적으로 사용하여 Signiture간 충돌을 방지합니다.

## interface 설계시 일반 가이드 라인

* 딱 정해졌다기 보다 이런식으로 설계를 한다는 점에 대한 가이드라인의 의미입니다.

1. 해결하려는 문제에 인터페이스를 집중시키고 관련 작업 \(방법\)을 인터페이스에 유지하십시오. 
   1. 여러 관련없는 작업이있는 인터페이스는 클래스에서 구현하기가 매우 어려운 경향이 있습니다. 
   2. 관련없는 기능이 포함 된 인터페이스를 분할하십시오.
2. 인터페이스에 너무 많은 메소드가 포함되어 있지 않은지 확인하십시오.
   1.  구현 클래스가 인터페이스의 모든 메소드를 구현해야하므로 메소드가 너무 많으면 인터페이스 구현이 어려워집니다.
3. 특정 기능을위한 인터페이스를 만들지 마십시오. 
   1. 인터페이스는 다른 모듈 또는 서브 시스템의 클래스로 구현할 수있는 공통 기능을 정의해야합니다.

{% embed url="https://www.dotnettricks.com/learn/csharp/a-deep-dive-into-csharp-interface" %}

