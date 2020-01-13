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

{% hint style="info" %}
{% code title="Movement.cs" %}
```csharp
///<summary>
/// InputManager에서 설정한 정보를 axisName을 통해 가져옵니다.
///</summary>
float x = Input.GetAxis("Horizontal");
float y = Input.GetAxis("Vertical");
```
{% endcode %}

뒤의 GetAxis의 Parameter를 통해 정보를 가져옵니다.                                                               해당 정보는 Edit-&gt;Project Setting-&gt;Axes에서 확인할 수 있습니다.

그리고 Walk함수를 통해 Rigidbody2D를 움직여서 이동시킵니다.

```
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

rb.velocity에 새로운 Vector를 줘서 방향을 부여하고, y축으로는 점프로만 움직이기         때문에 고정 시킵니다. 여기서는 wallJumped 상태변수를 조건문에 넣어서 특정 조건을 만족할때는 else구문을 실행시킵니다.

Vector2.Lerp\(\) 함수는 rb.velocity와 \(new Vector2\(dir.x \* speed, rb.velocity.y\)\) 사이를    연결해서 0부터 1사이의 값을 가진 wallJumpLerp \* Time.deltaTime의 비율에 따라 값을 반환합니다. 

그리고 Animator Component를 받아와서 그 안에 SetHorizontalMovement로 Update안에 선언한 x,y의 값에 따라 parameter를 할당하면 그에 해당하는 Animation을 실행시킵니다.

{% code title="Movement.cs" %}
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
{% endcode %}

Dash 함수에서는 DoTweening의 함수를 사용합니다.

자세한 Document에 대한 정보는 아래의 링크에 들어가면 확인 하실 수 있습니다.

[http://dotween.demigiant.com/getstarted.php](http://dotween.demigiant.com/getstarted.php)

위의 링크에 들어가서 구글번역만 돌려봐도 함수에 대한 설명은 충분히 해석가능하다고 봅니다. 하지만 몇몇의 설정은 지금과 맞지않는 몇가지 있기에 해설문서에 적습니다.
{% endhint %}

{% embed url="http://www.naver.com" %}
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}





















