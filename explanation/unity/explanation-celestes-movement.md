---
description: Explanation Celeste's Movement
---

# Explanation Celeste's Movement\(작업중\)

## 무엇을 하려고 하는가?

* How-to-guide에서 설명하지 못한 Code Reivew의 해설문서\(Explanation\)을 적고 있습니다.

## Scripting

{% tabs %}
{% tab title="Movement.cs" %}
동작의 예외처리문이 많기 때문에 주요 함만 소개하고 해설하는 형식으로 서술하겠습니다. 

```csharp
///<summary>
/// InputManager에서 설정한 정보를 axisName을 통해 가져옵니다.
///</summary>
float x = Input.GetAxis("Horizontal");
float y = Input.GetAxis("Vertical");
```

뒤의 GetAxis의 Parameter를 통해 정보를 가져옵니다. 

해당 정보는 Edit-&gt;Project Setting-&gt;Axes에서 확인할 수 있습니다. 그리고 Walk함수를 통해 Rigidbody2D를 움직여서 이동시킵니다.

```csharp
private void Walk(Vector2 dir) {
    if (!canMove) 
        return;
    
    if (wallGrab) 
        return;
    
    if (!wallJumped) {
        rb.velocity = new Vector2(dir.x * speed, rb.velocity.y);
    } else {
        rb.velocity = Vector2.Lerp(rb.velocity, (new Vector2(dir.x * speed, rb.velocity.y)), wallJumpLerp * Time.deltaTime);
    }
}
```

rb.velocity에 새로운 Vector를 줘서 방향을 부여하고, y축으로는 점프로만 움직이기 때문에 고정 시킵니다. 

여기서는 wallJumped 상태변수를 조건문에 넣어서 특정 조건을 만족할때는 else구문을 실행시킵니다. 

Vector2.Lerp\(\) 함수는 rb.velocity와 \(new Vector2\(dir.x  _speed, rb.velocity.y\)\) 사이를 연결해서 0부터 1사이의 값을 가진 wallJumpLerp_  Time.deltaTime의 비율에 따라 값을 반환합니다. 

그리고 Animator Component를 받아와서 그 안에 SetHorizontalMovement로 Update안에 선언한 x,y의 값에 따라 parameter를 할당하면 그에 해당하는 Animation을 실행시킵니다.

```csharp
private void Dash(float x, float y){
        Camera.main.transform.DOComplete();                                                             // DOTweening 함수 사용
        Camera.main.transform.DOShakePosition(.2f, .5f, 14, 90, false, true);
        FindObjectOfType<RippleEffect>().Emit(Camera.main.WorldToViewportPoint(transform.position));    // Ripple Effect Script삽입

        hasDashed = true;

        anim.SetTrigger("dash");

        rb.velocity = Vector2.zero;
        Vector2 dir = new Vector2(x, y);

        rb.velocity += dir.normalized * dashSpeed;
        StartCoroutine(DashWait());
    }
```

DoTween API에 관한 자료는 아래의 링크에 들어가서 확인하실 수 있습니다.

{% embed url="http://dotween.demigiant.com/documentation.php\#genericTo" caption="DoTween Document" %}

간략하게 정리하자면,  DoTween API는 Unity Asset에 import하여 사용할 수 있는 API로써                어떤 기능을 작성하는데 들어가는 Code의 길이를 줄인 Code의 Shortcut을 강점으로 합니다.                 거기서 여러가지 Option을 줘서 작성자가 좀더 유연하게 작성할 수 있도록 도와주는 API라고 설명할 수 있습니다.

DoTween이 알려주는 몇가지 특징이 있습니다.

1. 트윈을 만들면 모든 루프가 완료 될 때까지 \(전역 defaultAutoPlay 동작을 변경하지 않는 한\) 자동으로 재생됩니다.
2. 트윈이 완료되면 전역 defaultAutoKill 동작을 변경하지 않는 한 자동으로 종료되므로 더 이상 사용할 수 없습니다.
3. 동일한 트윈을 재사용하려면 모든 트윈에 대한 전역 자동 킬 설정을 변경하거나 SetAutoKill \(false\)을 트윈에 연결하여 자동 킬 동작을 FALSE로 설정하십시오.
4. 트윈이 재생되는 동안 트윈의 대상이 NULL이되면 오류가 발생할 수 있습니다. 조심하거나 안전 모드를 활성화해야합니다.

API를 사용하는데 있어서 참고 시 이 문서가 유용하게 사용되길 바랍니다.

다시 Code  Review로 돌아가서 Movement.cs의 Dash\(\) 함수에서 사용되는 DoTween은 아래와 같습니다.

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

위와 같은 함수가 실행된 후 RippleEffect Script를 FindObjectOfType으로 찾고 Script 내부의 Emit함수를 실행시킵니다. 그 후 새로운 Vector2를 생성하여 방향을 설정하고 normalized \* dashSpeed를 통해 이동시키고 Coroutine을 시작합니다.
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}





























