---
description: Explanation Celeste's Movement
---

# Explanation Celeste's Movement\(작업중\)

## 무엇을 하려고 하는가?

* How-to-guide에서 설명하지 못한 Code Reivew의 해설문서\(Explanation\)을 적고 있습니다.

## Scripting

{% tabs %}
{% tab title="Movement.cs" %}
크게 기능함수, 효과\(Particle\)함수로 구분지어서 설명하도록 하겠습니다. 하지만 기능이 실행됨과 동시에 효과\(Particle\)도 적용해야하고, 보다 쉬운 설명을 위해 임의로 나누었습니다.



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

그 다음 내용들은 거의다 조건문에 따른 예외처리문이기 때문에 해당 내용은 아래에 서술 하겠습니다.
{% endhint %}
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}



