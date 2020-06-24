---
description: 'C# Basic Example interface'
---

# C\# Basic Example interface

## 무엇을 하려고 하는가?

* interface 관련 코드를 작성하고, 그에 따른 Review를 합니다.

```text
    public interface IControl
    {
        void Paint();
        int P { get; }
    }
    public interface ISurface
    {
        void Paint();
        int P();
    }
    public class SampleClass : IControl, ISurface
    {
        void IControl.Paint()
        {
            System.Console.WriteLine("IControl.Paint");
        }
        int IControl.P { get { return 0; } }
        void ISurface.Paint()
        {
            System.Console.WriteLine("ISurface.Paint");
        }
        int ISurface.P() { return 0; }
    }
```

