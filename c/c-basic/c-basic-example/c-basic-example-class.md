---
description: 'C# Basic Example Class'
---

# C\# Basic Example Class

## 무엇을 하려고 하는가?

* Class 관련 코드를 작성하고, 그에 따른 Review를 합니다.

```csharp

// Element Class
class Element {  public object obj { get; set; } }
class Color { public string _Color { get; set; } }


// Object IEnumerable을 이용한 Iterator Class
class AllObject : IEnumerable
{
    object[] _objects =
        {
            new Element() { obj = "string"},
            new Element() { obj = 123 },
            new Element() { obj = 1234.12 },
            new Element() { obj = "string two" }
        };

    public IEnumerator GetEnumerator() 
    { return new ObjectEnumerator(_objects); }
    private class ObjectEnumerator : System.Collections.IEnumerator 
    {
        private Object[] tempArr;
        private int _position = -1;
        public ObjectEnumerator(Object[] obj) 
        { this.tempArr = obj; }
        public object Current 
        { 
            get 
            { 
                return tempArr[_position]; 
            } 
        }
        public bool MoveNext() {
            _position++;
            return (_position < tempArr.Length); 
        }
        public void Reset() { _position = -1; }
    }
}

class AllColor : AllObject
{
    Color[] _colors =
        {
            new Color() { _Color = "Red" },
            new Color() { _Color = "Green" },
            new Color() { _Color = "Blue" }
        };
    public override IEnumerator GetEnumerator() { return new ObjectEnumerator(_colors); }
}


static void Main(string[] args)
{
    AllColor all = new AllColor();
    foreach(Color item in all) { Console.WriteLine(item._Color); }

    AllObject Obj = new AllObject();
    foreach (Element item in Obj) { Console.WriteLine(item.obj); }
}
```

* 두가지 가장 기본적인 요소를 이루는 Element, Color는 AllElement, AllColor Class를 통해 배열로 생성되고 자리잡습니다.
* 그 후 IEnumerator라는 반복기를 통해 배열의 Index값을 증가시켜서 끝까지 이동하며, 탐색 합니다.
* Main함수에서 Foreach 반복문을 통해 Element, Color의 정보를 출력하고 있습니다.

{% hint style="info" %}
Foreach 반복문은 for-loop보다 좀 더 가다듬었지만, 몇몇 경우에는 for-loop를 사용하는 것이 나을 수 있습니다.

보통은 Collection에 포함된 자료구조들에 대해 사용하는 것이 효율, 코드면에서 깔끔한 것 같습니다.

\(여기서 Collection은 Stack, Queue, List, SortedList 등등의 System.Collection namespace에 포함된 자료구조들입니다.\)
{% endhint %}



