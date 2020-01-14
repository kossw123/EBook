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

여기서는 wallJumped 상태변수를 조건문에 어서 특정 조건을 만족할때는 else구문을 실행시킵니다. 

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

```csharp
IEnumerator DashWait()
    {
        FindObjectOfType<GhostTrail>().ShowGhost();
        StartCoroutine(GroundDash());
        DOVirtual.Float(14, 0, .8f, RigidbodyDrag);

        dashParticle.Play();
        rb.gravityScale = 0;
        GetComponent<BetterJumping>().enabled = false;
        wallJumped = true;
        isDashing = true;

        yield return new WaitForSeconds(.3f);

        dashParticle.Stop();
        rb.gravityScale = 3;
        GetComponent<BetterJumping>().enabled = true;
        wallJumped = false;
        isDashing = false;
    }
```

IEnumerator를 사용하여 함수포인터의 역할 yield문을 만나고 함수의 실행이 종료될 때 까지 반복합니다. 그 후 DOVirtual.Float를 사용합니다.

DOVirtual.Float\(\)를 사용하여 RigidbodyDrag\(\) 함수를 호출하여 14, 0, .8f의 강도로 함수를 실행시킵니다. 다음은 아래의 코드를 실행시키고 yield문을 만나 0.3f Time 후에 아래의 코드를 실행시킵니다.

```csharp
IEnumerator GroundDash()
    {
        yield return new WaitForSeconds(.15f);
        if (coll.onGround)
            hasDashed = false;
    }
```

GroundDash IEnumerator\(열거자\) 입니다. 0.15f Time 후에 아래의 코드를 실행키고 종료합니다.

```csharp
private void WallJump()
    {
        if ((side == 1 && coll.onRightWall) || side == -1 && !coll.onRightWall)
        {
            side *= -1;
            anim.Flip(side);
        }

        StopCoroutine(DisableMovement(0));
        StartCoroutine(DisableMovement(.1f));

        Vector2 wallDir = coll.onRightWall ? Vector2.left : Vector2.right;

        Jump((Vector2.up / 1.5f + wallDir / 1.5f), true);

        wallJumped = true;
    }
```

Update\(\)에서 조건문에 따라 WallJump\(\) 함수를 실행시키는데 조건문에 따라 Sprite Flip을 하고 Coroutine을 실행 시킵니다. 그 후 새로운 Vector2를 생성하는데 coll.onRightWall, 즉 오른쪽인가 아닌가에 따라 Vector2를 할당하고 Jump함수를 실행시킵니다. 

여기서 Jump\(\)함수의 parameter를 보자면 Vector2.up으로 위쪽 Vector를 주고 1.5f + wallDir을 누고 또 1.5f로 나눕니다. 이 부분은 기존의 Jump와는 다르게 벽에서 뛰는 Jump이기 때문에 각도를 조절해줘야 합니다. 이를 위해 Vector2에서 나눈다는 것인데, 

**Vector의 연산에서는 나누기가 없습니다. 내적과 외적은 벡터의 곱이라고 볼 수 있는데 나눗셈은 내적과 외적의 역원이 성립되지 않기 때문에 나눗셈이 정의 될 수 없습니다.** 

이는 Vector와 Vector의 나눗셈이 아니라 스칼라와 Vector의 나눗셈이라고 볼 수 있습니다. 그렇기 때문에 방향에 영향을 주지않고 크기에만 영향을 줄 수 있습니다. 이를 통해 WallJump시 크기만 조절할 수 있게 됩니다.

```text
    IEnumerator DisableMovement(float time)
    {
        canMove = false;
        yield return new WaitForSeconds(time);
        canMove = true;
    }
```

위의 IEnumerator\(열거자\)는 Coroutine으로 행동에 딜레이를 거는 함수입니다.

```csharp
    private void Jump(Vector2 dir, bool wall)
    {
        slideParticle.transform.parent.localScale = new Vector3(ParticleSide(), 1, 1);
        ParticleSystem particle = wall ? wallJumpParticle : jumpParticle;

        rb.velocity = new Vector2(rb.velocity.x, 0);
        rb.velocity += dir * jumpForce;

        particle.Play();
    }
```

Jump\(\)함수는 Walk\(\)함수와 마찬가지로 Rigidbody2D의 velocity를 조절하여 방향 Vector인 dir과 jumpForce인 크기를 곱하여 세기를 조절합니다.

```csharp
private void WallSlide()
    {
        if(coll.wallSide != side)
         anim.Flip(side * -1);  

        if (!canMove)
            return;

        bool pushingWall = false;
        if((rb.velocity.x > 0 && coll.onRightWall) || (rb.velocity.x < 0 && coll.onLeftWall))
        {
            pushingWall = true;
        }
        float push = pushingWall ? 0 : rb.velocity.x;

        rb.velocity = new Vector2(push, -slideSpeed);

    }
```

WallSlide\(\)함수에서는 Sprite Flip과 움직일 수 없을 때의 예외\(!canMove\)처리 후 pushingWall이라는 변수를 통해 오른쪽 벽에서 Rigidbody2D.velocity가 움직이거나, 왼쪽 벽에서 Rigidbody2D.velocity가 움직 일때 true로 바꾸고 push변수를 통해 pushingWall에 따라 true면 0, false면 Rigidbody2D.velocity.x를 할당합니다. 그 후 최종적인 Rigidbody2D.velocity의 Vector를 x값이 push, y값이 미끌어져서 떨어지는 수치인 slideSpeed로 할당합니다.

{% code title="Movement.cs" %}
```csharp
    void RigidbodyDrag(float x)
    {
        rb.drag = x;
    }
```
{% endcode %}

DashWait에서 DOVirtual\(\)함수를 이용하여 Rigidbody Component의 Drag기능을 조절합니다.

```text
    void WallParticle(float vertical)
    {
        var main = slideParticle.main;

        if (wallSlide || (wallGrab && vertical < 0))
        {
            slideParticle.transform.parent.localScale = new Vector3(ParticleSide(), 1, 1);
            main.startColor = Color.white;
        }
        else
        {
            main.startColor = Color.clear;
        }
    }
```

WallParticle 함수는 var변수를 통하여 암시적 변수 타입을 통해 대입되는 값으로 변수형을 결정하여 이를 통해 particle의 Color를 제어합니다. 그리고 조건문에 따라 Particle의 Local Position과 Scale을 정합니다.

{% code title="Movement.cs" %}
```csharp
    int ParticleSide()
    {
        int particleSide = coll.onRightWall ? 1 : -1;
        return particleSide;
    }
```
{% endcode %}

위의 WallParticle\(\)함수에서 ParticleSide함수를 통해 왼쪽, 오른쪽을 조절합니다.
{% endtab %}

{% tab title="BetterJumping.cs" %}

{% endtab %}
{% endtabs %}

































