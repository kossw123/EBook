---
description: TR Celeste's Movement
---

# TR Celeste's Movement\(작업중\)

## 무엇을 하려고 하는가?

* 해설문서\(Explanation\)에서 설명하지 못한 함수의 상세 내용과 사용법을 명시합니다.
* 외부 API에 관한 상세 설명을 명시합니다.
* 해당자료는 학습적인 목적 이외에는 이용하지 않습니다.

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
7. DoTween



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
* Coroutine에서는 IEnumerator\(열거자\)를 사용하여 특정 시간 및 반환값을 조절할 수 있습니다.
* **한 프레임에서 동작하는 Update\(\)문에 StartCoroutine을 사용하여 제어권을 가져온 다음 Coroutine이 끝난 다음 프레임부터는 다시 제어권을 넘겨서 중지된 곳부터 실행시킵니다.**

\*\*\*\*

* Coroutine을 사용할 시 필요한 요소들을 정리해봤습니다.
  * StartCoroutine\(IEnumerator enum\); 을 사용하여 진입점을 설정합니다.
  * IEnumerator\(\)를 사용하여 진입점 이후 실행될 내용을 설정합니다. 
  * IEnumerator\(\)의 실행이 끝나면 다음 프레임부터 Update\(\)문의 중단된 지점부터 다시 시작합니다.



7. DoTween

* Unity에서의 외부 API입니다. 이를 사용하여 statement의 간략화와, 여러가지 성능을 알아볼 수 있습니다.
* 아래에서 설명되는 Tween은 DoTween namespace에 포함된 method들을 뜻합니다.

1. Tween을 만들면 모든 루프가 완료 될 때까지 \(전역 defaultAutoPlay 동작을 변경하지 않는 한\) 자동으로 재생됩니다.
2. Tween이 완료되면 전역 defaultAutoKill 동작을 변경하지 않는 한 자동으로 종료되므로 더 이상 사용할 수 없습니다.
3. 동일한 Tween을 재사용하려면 모든 Tween에 대한 global autoKill 설정을 변경하거나 SetAutoKill \(false\)을 Tween에 연결하여 자동 킬 동작을 FALSE로 설정하십시오.
4. Tween이 재생되는 동안 Tween의 대상이 NULL이되면 오류가 발생할 수 있습니다. 조심하거나 안전 모드를 활성화해야합니다.

* Movement.cs에서 쓰이는 Method들은 다음과 같습니다.

{% hint style="info" %}
Camera.main.transform.DOComplete\(\);

이 함수를 실행하면 DoTween을 사용한 변경이 즉시 종료되고, 이동이 완료됩니다.
{% endhint %}

{% hint style="info" %}
Camera.main.transform.DOShakePosition\(float duration, float/Vector3 strength, int vibrato, float randomness, bool fadeOut\)

카메라를 흔듭니다.

* duration : 흔들림 강도. float 대신 Vector3를 사용하면 각 축의 강도를 선택할 수 있습니다.
* strength : 진동 세기
* randomness : 임의의 흔들림 정도, 0으로 설정하면 한 방향으로 흔들립니다.
* fadeOut : default가 true입니다. 만약 true라면 흔들림이 자동으로 부드럽게 사라집니다.
{% endhint %}
{% endtab %}

{% tab title="Collision.cs" %}
다음과 같은 목차를 가지고 있습니다. 이번 문서에서는 불편하시겠지만 GitBook 양식의 한계로 인해  목차를 가지고 Ctrl+F로 검색하시기 바랍니다.

1. Physics2D.OverlapCircle\(\)

1.Physics2D.OverlapCircle\(\)

{% embed url="https://m.blog.naver.com/PostView.nhn?blogId=pxkey&logNo=221324857701&proxyReferer=https%3A%2F%2Fwww.google.com%2F" %}

* Physics2D.OverlapCircle\(\) 함수는 Overloading된 2가지의 함수형태를 가지고 있습니다.

```csharp
1. public static Collider2D OverlapCircle(Vector2 point, float radius, int layerMask = DefaultRaycastLayers, float minDepth = -Mathf.Infinity, float maxDepth = Mathf.Infinity);
2. public static int OverlapCircle(Vector2 point, float radius, ContactFilter2D contactFilter, Collider2D[] results);
```

* 위와 같은 내용을 가지고 있으며, 이번에 사용한 것은 1번 형태입니다. 그렇기 때문에 1번 형태 위주로 해설문서\(Explanation\)을 작성하겠습니다.



* Collider\(충돌체\)가 Radius로 생성된 Circle영역 안에 있는지 확인합니다.
* LayerMask parameter를 이용해서 특정 Layer의 Object만 검사할 수 있습니다.
{% endtab %}
{% endtabs %}

























