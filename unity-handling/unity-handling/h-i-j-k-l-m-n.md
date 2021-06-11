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
5. 
## K 

## L 

HashMap을 구현하려고 했으나, 여러가지 햇갈리는 부분이 존재했고, 이는 구조에 대해 자세하게 모른다는 의미라는 것을 깨달아서 list Structure를 먼저 정리하고자 한다.

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System;

using Structure.List.CollectionList;

public class _List : MonoBehaviour
{
    _CollectionList list;

    private void Start()
    {
        list = new _CollectionList();

        for(int i = 0; i < 10; i++ )
        {
            list.Add(i);
        }

        foreach(var item in list)
        {
            Debug.Log(item);
        }

    }

    private void Update()
    {
        
    }
}


namespace Structure.List.SimpleList
{
    // 간단한 List구현
    [System.Serializable]
    public class Fundamental : System.IDisposable
    {
        public int[] array = new int[4];
        public int position = -1;
        int Length = 4;

        #region Funtion Callbacks
        public void Add(int value)
        {
            if (position + 1 >= array.Length)
            {
                EnsureCapacity();
            }

            position++;
            array[position] = value;
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
            for(int i = 0; i < array.Length; i++)
            {
                if(array[i] == value)
                {
                    return true;
                }
            }
            return false;
        }
        #endregion

        #region initializer Callbacks
        ~Fundamental() => Dispose();
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

        public int Add(object value)
        {
            throw new NotImplementedException();
        }

        public bool Contains(object value)
        {
            throw new NotImplementedException();
        }

        public int IndexOf(object value)
        {
            throw new NotImplementedException();
        }

        public void Insert(int index, object value)
        {
            throw new NotImplementedException();
        }

        public void Remove(object value)
        {
            throw new NotImplementedException();
        }

        public void RemoveAt(int index)
        {
            throw new NotImplementedException();
        }

        public void CopyTo(Array array, int index)
        {
            throw new NotImplementedException();
        }

        public IEnumerator GetEnumerator()
        {
            throw new NotImplementedException();
        }
        #endregion

    }
}


namespace Structure.List.CollectionList
{
    [System.Serializable]
    public class _CollectionList : IList
    {
        public object[] data;
        int position;

        public _CollectionList()
        {
            data = new object[8];
            position = -1;
        }


        #region ILIst
        public object this[int index]
        {
            get
            {
                if (index < 0 || index > position)
                    throw new InvalidOperationException();
                else
                    return data[index];
            }
            set
            {
                if (!(index < 0 || index > position))
                    data[index] = value;
            }
        }

        public bool IsFixedSize
        {
            get
            {
                return true;
            }
        }

        public bool IsReadOnly
        {
            get
            {
                return false;
            }
        }

        public int Count
        {
            get
            {
                return data.Length;
            }
        }

        public bool IsSynchronized
        {
            get
            {
                return false;
            }
        }

        public object SyncRoot
        {
            get
            {
                return this;
            }
        }

        public int Add(object value)
        {
            EnsureCapacity();

            if (position < data.Length)
            {
                position++;
                data[position] = value;
                return position;
            }

            return -1;
        }

        void EnsureCapacity()
        {
            if (position + 1 == data.Length)
            {
                object[] temp = (object[])data.Clone();
                data = new object[data.Length * 2];
                Array.Copy(temp, 0, data, 0, data.Length / 2);
                temp = null;
            }
        }

        public void Clear()
        {
            position = -1;
        }

        public bool Contains(object value)
        {
            for (int i = 0; i < data.Length; i++)
            {
                if (data[i] == value)
                    return true;
            }

            return false;
        }

        public void CopyTo(Array array, int index)
        {
            for (int i = 0; i < Count; i++)
            {
                array.SetValue(data[i], index++);
            }
        }

        public int IndexOf(object value)
        {
            for (int i = 0; i < data.Length; i++)
            {
                if (data[i] == value)
                    return i;
            }

            return -1;
        }

        public void Insert(int index, object value)
        {
            if ((position + 1 <= data.Length) && (index < Count) && (index >= 0))
            {
                position++;

                for (int i = Count - 1; i > index; i--)
                {
                    data[i] = data[i - 1];
                }
                data[index] = value;
            }
        }

        public void Remove(object value)
        {
            RemoveAt(IndexOf(value));
        }

        public void RemoveAt(int index)
        {
            if ((index >= 0) && (index < Count))
            {
                for (int i = index; i < Count - 1; i++)
                {
                    data[i] = data[i + 1];
                }
                position--;
            }
        }
        #endregion

        #region IEnumerator
        public IEnumerator GetEnumerator()
        {
            return new Enumerator(data);
        }

        public struct Enumerator : IEnumerator, IDisposable
        {
            private object[] data;
            int position;
            public object Current
            {
                get
                {
                    try
                    {
                        return data[position];
                    }
                    catch(IndexOutOfRangeException)
                    {
                        throw new InvalidOperationException();
                    }
                }
            }

            public Enumerator(object[] pArray)
            {
                position = -1;
                data = pArray;
                for(int i = 0; i < pArray.Length; i++)
                {
                    data[i] = pArray[i];
                }
            }

            public void Dispose()
            {
                Dispose(true);
                GC.SuppressFinalize(this);
            }

            public void Dispose(bool disposing)
            {
                if(disposing)
                {
                    Reset();
                }
            }

            public bool MoveNext()
            {
                position++;
                return (position < data.Length);
            }

            public void Reset()
            {
                position = -1;
            }
        }
        #endregion
    }
}

```

## M 

## N

