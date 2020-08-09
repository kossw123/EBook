---
description: Algorithm
---

# Algorithm

## 정

{% tabs %}
{% tab title="선택정렬\(Selection Sort\)" %}
```csharp
for(int i=0; i<_stack.Length -1; i++)
{
    int temp = i;
    for(int j=i+1; j<_stack.Length; j++)
    {
        if (_stack  [temp] >= _stack[j])
            temp = j;
    }
    int tmp = _stack[i];
    _stack[i] = _stack[temp];
    _stack[temp] = tmp;
}
```
{% endtab %}

{% tab title="버블 정렬\(Bubble Sort\)" %}
```csharp
int tmp;
for (int i=0; i<_stack.Length - 1; i++)
{
    for(int j=0; j<_stack.Length - i - 1; j++)
    {
        if(_stack[j] > _stack[j+1])
        {
            tmp = _stack[j+1];
            _stack[j+1] = _stack[j];
            _stack[j] = tmp;
        }
    }
}
```
{% endtab %}
{% endtabs %}

## 트리

{% tabs %}
{% tab title="이진 탐색 트리" %}
```csharp
class Tree
{ 
        public class Node
    {
        public int data;
        public Node left;
        public Node right;
        public Node(int data)
        {
            this.data = data;
        }
    }
    public Node root;

    public void MakeTree(int[] arr)
    {
        root = MakeTreeR(arr, 0, arr.Length - 1);
    }
    
    Node MakeTreeR(int[] arr, int start, int last)
    {
        if (start > last) return null;

        int mid = (start + last) / 2;
        Node node = new Node(arr[mid]);
        node.left = MakeTreeR(arr, start, mid - 1);
        node.right = MakeTreeR(arr, mid + 1, last);

        return node;
    }
    
    public void searchTree(Node n, int find)
    {
        if (find < n.data)
        {
            Debug.Log("Data is smallar than" + n.data);
            searchTree(n.left, find);
        }
        else if (find > n.data)
        {
            Debug.Log("Data is bigger than" + n.data);
            searchTree(n.right, find);
        }
        else
            Debug.Log("Search!!");
        }
}

void Start()
{
    int[] Array = new int[10];
    for(int i=0; i<Array.Length; i++)
        Array[i] = Random.Range(1, 10);

    Tree t = new Tree();
    t.MakeTree(Array);
    t.searchTree(t.root, 3);
}
```
{% endtab %}
{% endtabs %}



## 스택, 큐

{% tabs %}
{% tab title="큐\(Queue\)" %}
```csharp
    public int[] _queue;
    private int capacity; 
    public int front;
    public int rear;
    public bool isFrontPosisRear = false;

    public RandomVariable _value;
    Text _pushText;
    Text _popText;

    private void Start()
    {
        capacity = 10;
        InitalQueue(capacity: capacity);
        FindComponent();
    }

    void InitalQueue(int capacity)
    {
        _queue = new int[capacity];
        front = 0;
        rear = -1;
    }

    void FindComponent()
    {
        _pushText = GameObject.Find("PushCountText").GetComponent<Text>();
        _popText = GameObject.Find("PopCountText").GetComponent<Text>();
    }

    public void Enqueue()
    {
        if(rear + 1 < capacity)
        {
            rear += 1;
            _queue[rear] = _value._randomVar;
            _pushText.text = "Count : " + _queue[rear].ToString();
        }
    }

    public void Dequeue()
    {
        _popText.text = "Count : " + _queue[front].ToString();
        _queue[front] = 0;
        front += 1;

        if (front - 1 == rear)
            isFrontPosisRear = true;
    }

    private void Update()
    {
        if(isFrontPosisRear)
        {
            rear = -1;
            front = 0;
            isFrontPosisRear = false;
        }
    }
```
{% endtab %}

{% tab title="스택\(Stack\)" %}
```csharp
    public int[] _stack;
    private int capacity;
    private int count;
    private int[] _tempArr;

    RandomVariable _value;
    Text _pushText;
    Text _popText;

    private void Start()
    {
        FindComponent();
        capacity = 10;
        InitialStack(capacity: capacity);
    }

    void InitialStack(int capacity)
    {
        _stack = new int[capacity];
        count = -1;
        _tempArr = new int[capacity];
    }

    void FindComponent()
    {
        _pushText = GameObject.Find("PushCountText").GetComponent<Text>();
        _popText = GameObject.Find("PopCountText").GetComponent<Text>();
        _value = GameObject.Find("StackCountText").GetComponent<RandomVariable>();
    }

    public void Push()
    {
        count++;
        _stack[count] = _value._randomVar;
        _pushText.text = "Count : " + _stack[count].ToString();
    }

    public void Pop()
    {
        _popText.text = "Count : " + _stack[count].ToString();
        _stack[count] = 0;
        count--;
    }
```
{% endtab %}
{% endtabs %}

