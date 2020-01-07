---
description: How-to-guide Celeste's Movement
---

# How-to-guide Celeste's Movement\(작업중\)

## 무엇을 하려고 하는가?

* Celeste Movement 강의에 나온 영상을 가지고 코드 리뷰를 작성합니다.
* 각 코드마다 if문의 사용으로 보기 어려울 수 있으니 코드 옆에 줄 수를 바탕으로 작성하겠습니다.

## Celeste Code Review

* 여기서 쓰인 Component에 대한 설명보다는 

{% tabs %}
{% tab title="Movement.cs" %}
```cpp
using System; 
using System.Collections; 
using System.Collections.Generic;
using UnityEngine; 
using DG.Tweening;
public class Movement : MonoBehaviour { 

private Collision coll; 

[HideInInspector] 
public Rigidbody2D rb;
private AnimationScript anim;

[Space]
[Header("Stats")]
public float speed = 10;
public float jumpForce = 50;
public float slideSpeed = 5;
public float wallJumpLerp = 10;
public float dashSpeed = 20;

[Space]
[Header("Booleans")]
public bool canMove;
public bool wallGrab;
public bool wallJumped;
public bool wallSlide;
public bool isDashing;

[Space]

private bool groundTouch;
private bool hasDashed;

public int side = 1;

[Space]
[Header("Polish")]
public ParticleSystem dashParticle;
public ParticleSystem jumpParticle;
public ParticleSystem wallJumpParticle;
public ParticleSystem slideParticle;

// Start is called before the first frame update
void Start()
{
    coll = GetComponent<Collision>();
    rb = GetComponent<Rigidbody2D>();
    anim = GetComponentInChildren<AnimationScript>();
}

// Update is called once per frame
void Update()
{
    float x = Input.GetAxis("Horizontal");
    float y = Input.GetAxis("Vertical");
    float xRaw = Input.GetAxisRaw("Horizontal");
    float yRaw = Input.GetAxisRaw("Vertical");
    Vector2 dir = new Vector2(x, y);

    Walk(dir);
    anim.SetHorizontalMovement(x, y, rb.velocity.y);

    if (coll.onWall && Input.GetButton("Fire3") && canMove)
    {
        if(side != coll.wallSide)
            anim.Flip(side*-1);
        wallGrab = true;
        wallSlide = false;
    }

    if (Input.GetButtonUp("Fire3") || !coll.onWall || !canMove)
    {
        wallGrab = false;
        wallSlide = false;
    }

    if (coll.onGround && !isDashing)
    {
        wallJumped = false;
        GetComponent<BetterJumping>().enabled = true;
    }

    if (wallGrab && !isDashing)
    {
        rb.gravityScale = 0;
        if(x > .2f || x < -.2f)
        rb.velocity = new Vector2(rb.velocity.x, 0);

        float speedModifier = y > 0 ? .5f : 1;

        rb.velocity = new Vector2(rb.velocity.x, y * (speed * speedModifier));
    }
    else
    {
        rb.gravityScale = 3;
    }

    if(coll.onWall && !coll.onGround)
    {
        if (x != 0 && !wallGrab)
        {
            wallSlide = true;
            WallSlide();
        }
    }

    if (!coll.onWall || coll.onGround)
        wallSlide = false;

    if (Input.GetButtonDown("Jump"))
    {
        anim.SetTrigger("jump");

        if (coll.onGround)
            Jump(Vector2.up, false);
        if (coll.onWall && !coll.onGround)
            WallJump();
    }

    if (Input.GetButtonDown("Fire1") && !hasDashed)
    {
        if(xRaw != 0 || yRaw != 0)
            Dash(xRaw, yRaw);
    }

    if (coll.onGround && !groundTouch)
    {
        GroundTouch();
        groundTouch = true;
    }

    if(!coll.onGround && groundTouch)
    {
        groundTouch = false;
    }

    WallParticle(y);

    if (wallGrab || wallSlide || !canMove)
        return;

    if(x > 0)
    {
        side = 1;
        anim.Flip(side);
    }
    if (x < 0)
    {
        side = -1;
        anim.Flip(side);
    }


}

void GroundTouch()
{
    hasDashed = false;
    isDashing = false;

    side = anim.sr.flipX ? -1 : 1;

    jumpParticle.Play();
}

private void Dash(float x, float y)
{
    Camera.main.transform.DOComplete();
    Camera.main.transform.DOShakePosition(.2f, .5f, 14, 90, false, true);
    FindObjectOfType<RippleEffect>().Emit(Camera.main.WorldToViewportPoint(transform.position));

    hasDashed = true;

    anim.SetTrigger("dash");

    rb.velocity = Vector2.zero;
    Vector2 dir = new Vector2(x, y);

    rb.velocity += dir.normalized * dashSpeed;
    StartCoroutine(DashWait());
}

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

IEnumerator GroundDash()
{
    yield return new WaitForSeconds(.15f);
    if (coll.onGround)
        hasDashed = false;
}

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

private void Walk(Vector2 dir)
{
    if (!canMove)
        return;

    if (wallGrab)
        return;

    if (!wallJumped)
    {
        rb.velocity = new Vector2(dir.x * speed, rb.velocity.y);
    }
    else
    {
        rb.velocity = Vector2.Lerp(rb.velocity, (new Vector2(dir.x * speed, rb.velocity.y)), wallJumpLerp * Time.deltaTime);
    }
}

private void Jump(Vector2 dir, bool wall)
{
    slideParticle.transform.parent.localScale = new Vector3(ParticleSide(), 1, 1);
    ParticleSystem particle = wall ? wallJumpParticle : jumpParticle;

    rb.velocity = new Vector2(rb.velocity.x, 0);
    rb.velocity += dir * jumpForce;

    particle.Play();
}

IEnumerator DisableMovement(float time)
{
    canMove = false;
    yield return new WaitForSeconds(time);
    canMove = true;
}

void RigidbodyDrag(float x)
{
    rb.drag = x;
}

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

int ParticleSide()
{
    int particleSide = coll.onRightWall ? 1 : -1;
    return particleSide;
}
```
{% endtab %}

{% tab title="BetterJumping.cs" %}
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class BetterJumping : MonoBehaviour
{
    private Rigidbody2D rb;
    public float fallMultiplier = 2.5f;
    public float lowJumpMultiplier = 2f;

    void Start()
    {
        rb = GetComponent<Rigidbody2D>();
    }

    void Update()
    {
        if(rb.velocity.y < 0)
        {
            rb.velocity += Vector2.up * Physics2D.gravity.y * (fallMultiplier - 1) * Time.deltaTime;
        }else if(rb.velocity.y > 0 && !Input.GetButton("Jump"))
        {
            rb.velocity += Vector2.up * Physics2D.gravity.y * (lowJumpMultiplier - 1) * Time.deltaTime;
        }
    }
}
```
{% endtab %}

{% tab title="Collision.cs" %}
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Collision : MonoBehaviour
{

    [Header("Layers")]
    public LayerMask groundLayer;

    [Space]

    public bool onGround;
    public bool onWall;
    public bool onRightWall;
    public bool onLeftWall;
    public int wallSide;

    [Space]

    [Header("Collision")]

    public float collisionRadius = 0.25f;
    public Vector2 bottomOffset, rightOffset, leftOffset;
    private Color debugCollisionColor = Color.red;

    // Start is called before the first frame update
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {  
        onGround = Physics2D.OverlapCircle((Vector2)transform.position + bottomOffset, collisionRadius, groundLayer);
        onWall = Physics2D.OverlapCircle((Vector2)transform.position + rightOffset, collisionRadius, groundLayer) 
            || Physics2D.OverlapCircle((Vector2)transform.position + leftOffset, collisionRadius, groundLayer);

        onRightWall = Physics2D.OverlapCircle((Vector2)transform.position + rightOffset, collisionRadius, groundLayer);
        onLeftWall = Physics2D.OverlapCircle((Vector2)transform.position + leftOffset, collisionRadius, groundLayer);

        wallSide = onRightWall ? -1 : 1;
    }

    void OnDrawGizmos()
    {
        Gizmos.color = Color.red;

        var positions = new Vector2[] { bottomOffset, rightOffset, leftOffset };

        Gizmos.DrawWireSphere((Vector2)transform.position  + bottomOffset, collisionRadius);
        Gizmos.DrawWireSphere((Vector2)transform.position + rightOffset, collisionRadius);
        Gizmos.DrawWireSphere((Vector2)transform.position + leftOffset, collisionRadius);
    }
}

```
{% endtab %}

{% tab title="AnimationScript.cs" %}
```
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class AnimationScript : MonoBehaviour
{

    private Animator anim;
    private Movement move;
    private Collision coll;
    [HideInInspector]
    public SpriteRenderer sr;

    void Start()
    {
        anim = GetComponent<Animator>();
        coll = GetComponentInParent<Collision>();
        move = GetComponentInParent<Movement>();
        sr = GetComponent<SpriteRenderer>();
    }

    void Update()
    {
        anim.SetBool("onGround", coll.onGround);
        anim.SetBool("onWall", coll.onWall);
        anim.SetBool("onRightWall", coll.onRightWall);
        anim.SetBool("wallGrab", move.wallGrab);
        anim.SetBool("wallSlide", move.wallSlide);
        anim.SetBool("canMove", move.canMove);
        anim.SetBool("isDashing", move.isDashing);

    }

    public void SetHorizontalMovement(float x,float y, float yVel)
    {
        anim.SetFloat("HorizontalAxis", x);
        anim.SetFloat("VerticalAxis", y);
        anim.SetFloat("VerticalVelocity", yVel);
    }

    public void SetTrigger(string trigger)
    {
        anim.SetTrigger(trigger);
    }

    public void Flip(int side)
    {

        if (move.wallGrab || move.wallSlide)
        {
            if (side == -1 && sr.flipX)
                return;

            if (side == 1 && !sr.flipX)
            {
                return;
            }
        }

        bool state = (side == 1) ? false : true;
        sr.flipX = state;
    }
}
```
{% endtab %}
{% endtabs %}

