# H I J K L M N

## H 

## I 

## J 

### Job System의 사용

Unity 내부에서 Job System을 사용하여 멀티스레드 프로그래밍을 하는데  
C\#에서는 Threading, Task Class를 사용하여 멀티스레드 프로그래밍을 작성해야 한다.

Unity는 C++ 기반의 엔진을 C\# 기반의 코드로 변환하여 제공하기 때문에 Threading, Task Class를 사용할 수 있지만, 여러가지 예외 처리를 하던지 후작업이 필요하기 때문에,  
Unity에서는 Burst Compiler를 제공하여 여러가지 후작업에 대한 처리를 한다.

Burst Compiler는 IJob Struct에 Attribute로 넣기만 한다면 자동으로 돌아가는데,

Job을 어떻게 쓰는가에서 살짝 혼란이 왔다.

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
    [BurstCompile]
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

{% tab title="구조체 Job 사용" %}
```csharp
using UnityEngine;
using Unity.Burst;
using Unity.Jobs;

public class JobSample : MonoBehaviour
{
    struct Paragraph
    {
        public char _value;
    }

    [BurstCompile]
    struct Fundamental : IJob
    {
        public Paragraph paragraph;

        public void Execute()
        {
            Debug.Log("Paragraph char Value : " + paragraph._value);
        }
    }

    // Start is called before the first frame update
    void Start()
    {
        var job = new Fundamental
        {
            paragraph = new Paragraph
            {
                _value = 'P'
            }
        }.Schedule();
        /// Console 실행 결과 : Paragraph char Value : P

    }
}

```
{% endtab %}
{% endtabs %}

몇가지 확실하게 된 것이 있다.

1. Execute를 호출하려면 Schedule을 호출한다.
2. Job의 Schedule\(\)을 호출하면 JobHandle struct 반환한다.
3. 정의된 Struct를 확인해보니 크게 4가지 용도로 나누어져 있었다.
   1. Dependencies Combine
   2. Dependencies Check
   3. Complete
   4. Job Batch
4. 그리고 Schedule\(\) 함수는 Extension Method로 선언되어 있다.



## K 

## L 

### List

HashMap을 구현하려고 했으나, 여러가지 햇갈리는 부분이 존재했고, 이는 구조에 대해 자세하게 모른다는 의미라는 것을 깨달아서 list Structure를 먼저 정리하고자 한다.

```csharp
using System.Collections;
using UnityEngine;
using System;

using Structure.List.SimpleList;

public class _List : MonoBehaviour
{
    public Fundamental _fundamental;

    private void Start()
    {
        _fundamental = new Fundamental();

        _fundamental.Add(1);
        _fundamental.Add(2);
        _fundamental.Add(3);
        _fundamental.Add(4);
        _fundamental.Add(5);
        _fundamental.Add(6);

        for (int i = 0; i < _fundamental.Count; i++)
        {
            Debug.Log(_fundamental[i]);
        }

    }
}


namespace Structure.List.SimpleList
{
    // 간단한 List구현
    [System.Serializable]
    public class Fundamental : IDisposable
    {
        public int[] array;
        public int position;
        int Length;

        #region initializer Callbacks
        public Fundamental()
        {
            array = new int[4];
            position = -1;
            Length = 4;
        }

        public int this[int index]
        {
            get
            {
                if (index < 0 || index > position)
                    throw new InvalidOperationException();
                else
                    return array[index];
            }
            set
            {
                if (!(index < 0 || index > position))
                    array[index] = value;
            }
        }

        ~Fundamental() => Dispose();
        #endregion
        #region Disposable Callbacks
        public void Dispose()
        {
            Dispose(true);
            GC.SuppressFinalize(this);
        }
        public void Dispose(bool disposing)
        {
            if (disposing)
            {
                Clear();
            }
        }
        #endregion
        #region IList Components

        public int Count
        {
            get
            {
                return array.Length;
            }
        }

        public int Add(int value)
        {
            if(value == int.MinValue && value == int.MaxValue)
            {
                return -1;
            }

            if (position + 1 >= array.Length) EnsureCapacity();

            position++;
            array[position] = value;

            return 1;
        }

        void EnsureCapacity()
        {
            Length *= 2;
            int[] tempArr = (int[])array.Clone();
            array = new int[Length];
            Array.Copy(tempArr, 0, array, 0, Length / 2);
            tempArr = null;
        }


        public void Clear()
        {
            position = -1;
            array = null;
            Length = 4;
        }

        public bool Contains(int value)
        {
            for(int i = 0; i < Count; i++)
            {
                if(array[i] == value)
                {
                    return true;
                }
            }
            return false;
        }

        public int IndexOf(int value)
        {
            for(int i = 0; i < Count; i++)
            {
                if(array[i] == value)
                {
                    return i;
                }
            }
            return -1;
        }

        public void Insert(int index, int value)
        {
            if ((position + 1 <= array.Length) && (index < Count) && (index >= 0))
            {
                position++;

                for(int i = Count - 1; i > index; i--)
                {
                    array[i] = array[i - 1];
                }
                array[index] = value;
            }
        }

        public void Remove(int value)
        {
            RemoveAt(IndexOf(value));
        }

        public void RemoveAt(int index)
        {
            if ((index >= 0) && (index < Count))
            {
                for (int i = index; i > Count - 1; i--)
                {
                    array[i] = array[i + 1];
                }
                position--;
            }
        }

        public void CopyTo(Array pArray, int index)
        {
            for (int i = 0; i < Count; i++)
            {
                pArray.SetValue(array[i], index++);
            }
        }
        #endregion
    }
}
```



### Local Development Enviroment 구축 

저장, 불러오기 기능과 Collection을 작성하다 보니 쓰임새를 알거 같다.

* Save/Load는 어디에 메모리 주소와 값을 지정해서 어딘가에 저장하는데 "어딘가"를 설정하고 경로를 바꾸고 싶을 때 복잡하다. 그리고, 저장장치 어딘가에 기록 하려니, 하드웨어와 밀접하게 연관이 된다.
* Save/Load를 할 때 Binary library를 사용하여 기능을 작성하고 보니 변환 정보, 색, 모양, Object의 갯수, 저장된 Scene정도만 불러오는데 최소 500줄정도 되는듯 하다.

위와 같은 이유로 어떤 기능\(Save/Load\)을 작성할 때, OOP를 사용할 경우 중복되는 코드를 최소화하기 위해 어떤 데이터\(변환정보, 색, 모양, 갯수, Scene\)등을 하나의 Class로 만들어 재활용 하면

체감상 500줄 -&gt; 50줄 정도의 효율이 생긴다.

만약 Unity를 사용하지 않고 C\#으로 기능들을 작성했다면, 더 오래 걸렸을게 뻔하다.  
그렇다면 위와 같은 Save/Load 기능과 Collection 같은 기능을 컴퓨터에 미리 설치 혹은, 불러올 수 있다면  
어느정도의 효율을 불러올까?



그래서 기본적으로 APM\(Apache - PHP - MySQL\) 환경 구축을 하려고 한다.

APM에 대한 느낌을 간단히 설명하자면

1. 인터넷 상에 가상의 Container에다가 Data를 전송해주는 프로그램 \(Web Server - Apache\)
2. Data를 볼 수 있는 GUI \(Web Page\(GUI\) - PHP\)
3. Data를 간단하게 저장할 수 있는 프로그램\(DB - MySQL\)

위의 기능들이 멀쩡히 돌아가기 위해 엄청 많은 또다른 기능들이 존재 하지만, Unity는 게임엔진이기 때문에, 여기서 Save/Load와 Data를 저장하는 부분과 Client에서 Server를 거쳐 Web DB에 저장하기정도  
구현하면 될듯하다.

환경 구축 페이지를 만들어서 따로 작성하도록 한다.  
왜냐하면 환경이 여러개고, Docker등 환경을 일치 시킬 수 있는, 방법이 너무 많기 때문이다.

{% embed url="https://app.gitbook.com/@8iiow/s/t-h-e-t/~/drafts/-Mdcp0GcNGE2JZItINwU/unity-handling/undefined/apm/chapter-1." %}



  


## M 

## N

