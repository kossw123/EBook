---
description: TR Celeste's Movement
---

# TR Celeste's Movement\(작업중\)

## 무엇을 하려고 하는가?

* 해설문서\(Explanation\)에서 설명하지 못한 함수의 상세 내용과 사용법을 명시합니다.
* 외부 API에 관한 상세 설명을 명시합니다.
* 해당자료는 상업적인 용도로 이용하지 않습니다.

## Scripting

* 해설문서\(Explanation\)의 흐름에 따라 목차 및 함수를 게시합니다

{% tabs %}
{% tab title="Movement.cs" %}
다음과 같은 목차를 가지고 있습니다. 이번 문서에서는 불편하시겠지만 GitBook 양식의 한계로 인해  목차를 가지고 Ctrl+F로 검색하시기 바랍니다.

1. Input.GetAxis\(string axisName\)
2. Input.GetAxisRaw\(string axisName\)
3. Vector2.Lerp\( [Vector2 ](https://docs.unity3d.com/kr/530/ScriptReference/Vector2.html)a, [Vector2 ](https://docs.unity3d.com/kr/530/ScriptReference/Vector2.html)b, float t\)
4. 삼항연산자
5. FindObjectOfType\(Type type\)
6. Coroutine



1.Input.GetAxis\(string axisName\)

{% embed url="https://docs.unity3d.com/kr/530/ScriptReference/Input.GetAxis.html" caption="Input.GetAxis\(\) Document" %}

* 키보드, 조이스틱에 입력값에 대해서 -1~1의 값을 가집니다.
* 독립적인 프레임 속도로 작동하기 때문에 가변적인 프레임 변경을 신경쓰지 
* 아래의 링크를 본다면 좀 더 확실하게 이해하실 수 있습니다.

{% embed url="https://icancodeclub.mvcode.com/lessons/unity-physics-workshop" caption="Input.GetAxis\(\) 참고자료" %}



2.Input.GetAxisRaw\(string axisName\)

{% embed url="https://docs.unity3d.com/kr/530/ScriptReference/Input.GetAxisRaw.html" caption="Input.GetAxisRaw\(\) Document" %}

* Input.GetAxis\(\) 함수와 같은 기능을 하지만 다듬어 지지 않았기에 오직 -1, 0 ,1의 값을 반환합니다.



3.Vector2.Lerp\( [Vector2 ](https://docs.unity3d.com/kr/530/ScriptReference/Vector2.html)a, [Vector2 ](https://docs.unity3d.com/kr/530/ScriptReference/Vector2.html)b, float t\)

{% embed url="https://docs.unity3d.com/kr/530/ScriptReference/Vector2.Lerp.html" caption="Vector2.Lerp\(\) Document" %}

* 두 Vector사이를 선형 보간합니다. 즉, 시작점과 끝점 사이에서 선형적\(선처럼 길게 일렬로 나아가는 것\)으로 그 사이에 위치한 값을 t값의 비율에 따라 반환합니다.
*  `t` 는 \[0...1\] 사이에 고정됩니다.
* 아래의 링크를 보시면 예시와 함께 좀 더 자세하게 알아보실 수 있습니다.

{% embed url="http://developug.blogspot.com/2014/09/unity-vector-lerp.html" caption="Vector.Lerp\(\) Example" %}

\*\*\* 보통 Vector변수를 가지고 Rigidbody.velocity에 새로운 Vector를 생성하여 할당합니다.                    Walk\(\)함수에서 Vector변수를 parameter로 받아 parameter Vector의 x축을 가지고 speed를 곱연산하여 이동시키는데 이처럼 직접적으로 Rigidbody2D.velocity를 변경하지 않고 따로 speed변수를 통해 이동시키는 것을 Unity에서는 권장하고 있습니다. 

비정상적인 시뮬레이션을 일으키기 때문이라고 합니다.



4. 삼항연산자

{% embed url="https://docs.microsoft.com/ko-kr/dotnet/csharp/language-reference/operators/conditional-operator" caption="?: operator Document" %}

* Celeste's Movement에서 어떤 동작에 있어서 조건문이 많이 들어갑니다. 하지만 Flip이나, 간단한 값의 교환에 필요한 조건문을 삼항연산자로 대체해 쓰고 있습니다.
* 하지만 조건문을 자주쓰는 것은 속도면에서 부적절합니다. 그렇기 때문에 비교적 간단하고 조건이 짧은 것들에 대해서는 삼항연산자를 쓰는 것을 권장하고 있습니다.
* **또한 ref 식을 사용하여 두 식 중 하나의 결과에 대한 참조를 반환할 수 있습니다.**

{% embed url="https://programmers.co.kr/learn/questions/3499" caption=" if문과 삼항연산자의 속도비교" %}



5. FindObjectOfType\(Type type\)

{% embed url="https://docs.unity3d.com/kr/530/ScriptReference/Object.FindObjectOfType.html" caption="Object.FindObjectOfType\(Type type\) Document" %}

* 첫번째로 활성화한 Load된 type의 Object를 반환합니다.
* Component는 Object의 기능적인 조각들이기 때문에 Component도 탐색하여 반환가능합니다.
* Component를 찾을 때는 Generic Function을 넣어서 찾습니다.
* ex\)myCube = GameObject.FindObjectOfType&lt;Cubey&gt;\(\);

{% embed url="https://docs.unity3d.com/kr/530/Manual/GenericFunctions.html" %}



6. Coroutine

{% embed url="https://docs.unity3d.com/kr/530/Manual/Coroutines.html" caption="Coroutine Document" %}

{% embed url="http://theeye.pe.kr/archives/2725" caption="" %}

{% embed url="https://teddy.tistory.com/13" %}



* 특정 시간, 특정 작업을 할 때 유용하게 쓰이는 기능입니다.
* Unity는 Single Thread를 사용하기 때문에 Multi Thread 사용시 일어나는 경합조건 같은 side effect에 대해 신경쓸 필요가 없지만 어쩔수 없이 Multi Thread처럼 사용을 해야할 때 이를 이용하여 해결할 수 있습니다.
* Unity에서는 Multi Thread의 사용을 지양합니다.
{% endtab %}
{% endtabs %}























