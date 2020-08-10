---
description: Algorithm
---

# Algorithm

## 정렬 

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

/// <summary>
/// 선택 정렬은 첫번째 자료를 두번째 자료부터 마지막까지 비교하여 가장 적은 값을 첫번째로 놓는다.
/// 이를 반복하면 가장 작은값의 자료가 맨 앞으로 정렬

/// 시간복잡도는 O(n^2)
/// </summary>
```

* 장점 
  * 자료 이동 횟수가 미리 결정된다. 
* 단점 
  * 안정성을 만족하지 않는다. 
  * 즉, 값이 같은 레코드가 있는 경우에 상대적인 위치가 변경될 수 있다.
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

/// <summary>
/// 첫번째 자료와 두번째 자료, 두번째 자료와 세번째 자료, 이런식으로 n-1번째와 n번째를 비교정렬
/// 가장 큰 자료가 맨 뒤로 이동

/// 시간복잡도는 O(n^2)
/// </summary>
```

* 장점 
  * 구현이 매우 간단하다. 
* 단점 
  * 순서에 맞지 않은 요소를 인접한 요소와 교환한다. 
  * 하나의 요소가 가장 왼쪽에서 가장 오른쪽으로 이동하기 위해서는 배열에서 모든 다른 요소들과 교환되어야 한다. 
  * 특히 특정 요소가 최종 정렬 위치에 이미 있는 경우라도 교환되는 일이 일어난다. 
  * 일반적으로 자료의 교환 작업\(SWAP\)이 자료의 이동 작업\(MOVE\)보다 더 복잡하기 때문에 버블 정렬은 단순성에도 불구하고 거의 쓰이지 않는다. 
{% endtab %}

{% tab title="삽입정렬\(insert Sort\)" %}
```csharp
public static void insertSorting(int[] arr)
{
    int key;
    int i, j;
    for (i = 1; i < arr.Length; i++)
    {
        key = arr[i];

        for (j = i - 1; j >= 0; j--)
        {
            if (arr[j] > key)
                arr[j + 1] = arr[j];
            else
                break;
        }

        arr[j + 1] = key;
    }
}

/// <summary>
/// 삽입 정렬은 두 번째 자료부터 시작하여 그 앞(왼쪽)의 자료들과 비교하여 삽입할 위치를 
/// 지정한 후 자료를 뒤로 옮기고 지정한 자리에 자료를 삽입하여 정렬하는 알고리즘이다.

/// 즉, 두 번째 자료는 첫 번째 자료, 세 번째 자료는 두 번째와 첫 번째 자료, 
/// 네 번째 자료는 세 번째, 두 번째, 첫 번째 자료와 비교한 후 자료가 삽입될 위치를 찾는다. 
/// 자료가 삽입될 위치를 찾았다면 그 위치에 자료를 삽입하기 위해 자료를 한 칸씩 뒤로 이동시킨다.

/// 처음 Key 값은 두 번째 자료부터 시작한다.
/// </summary>
```

* 장점 
  * 안정한 정렬 방법 레코드의 수가 적을 경우 알고리즘 자체가 매우 간단하므로 다른 복잡한 정렬 방법보다 유리할 수 있다. 
  * 대부분위 레코드가 이미 정렬되어 있는 경우에 매우 효율적일 수 있다. 
* 단점 
  * 비교적 많은 레코드들의 이동을 포함한다. 
  * 레코드 수가 많고 레코드 크기가 클 경우에 적합하지 않다.
{% endtab %}

{% tab title="" %}

{% endtab %}
{% endtabs %}

![](.gitbook/assets/image%20%28232%29.png)

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

/// <summary>
/// 어떤 자료구조에서 중간값을 설정 후 root 노드 부터 Leaf노드까지 계속 반으로 나눠서
/// 작으면 왼쪽, 크면 오른쪽에 위치 시킨다.
/// 이를 반복하여 Tree 생성

/// 시간 복잡도는 O(n)
/// </summary>
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
    
/// <summary>
/// 선입선출의 구조, front(맨앞 노드 위치), rear(추가 후 노드 위치)를 통해 En, Dequeue시
/// rear를 이동시키고 데이터를 삽입한다.
/// </summary>
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
    
/// <summary>
/// 선입후출, Stack 자료구조의 count index를 0부터 시작하여 삽입 후 증가
/// Push, Pop이 있는데, 이를 통해 데이터를 삽입하고, 추출한다.
/// </summary>
```
{% endtab %}
{% endtabs %}

