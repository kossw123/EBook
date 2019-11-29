---
description: tutorial PhysicsObject에 이은 How-to-guide
---

# How-to-guide PhysicsObject

## 무엇을 하려고 하는가?

* 강의의 흐름에 따라 내용들을 Code Block으로 정리했으며, 각 단계에 따른 코드의 흐름을                           작성했습니다.
* 강의의 영상에 초점을 맞췄기 때문에 따라하기에 불편함이 있을수 있으니                                            How-to-guide를 보고 하시는 것보다는 tutorial의 코드를 참조하시길 바랍니다. 하지만 똑같은 코드이기 때문에 딱히 수정은 안하셔도 될듯합니다.
* 보다 자세한 설명은 아래의 링크의 해설문서로 작성하겠습니다.

{% page-ref page="../../explanation/unity/physicsobject.md" %}



## Scripting Gravity

```text
PhysicsObject.cs


using System.Collection;
using System.Collection.Generic;
using UnityEngine;

public class PhysicsObject : MonoBehaviour {
    public float gravityModifier = 1f;
    protected Rigidbody2D rb2d;
    protected Vector2 velocity;
    void OnEnable() {
        rb2d = GetComponent<Rigidbody2D>();
    }
    void FixedUpdate() {
        velocity += gravityModifier * Physics2D.gravity * Time.deltaTime;
        Vector2 deltaPosition = velocity * Time.deltaTime;
        Vector2 move = Vector2.up * deltaPosition.y;
        Movement(move);
    }
    void Movement(Vector2 move) {
        rb2d.position = rb2d.position + move;
    }
}
```

## Detecting Overlaps

```text
PhysicsObject.cs


using System.Collection;
using System.Collection.Generic;
using UnityEngine;

public class PhysicsObject: MonoBehaviour {
    protected const float minMoveDistance = 0.001f;
    protected ContactFilter2D contactFilter;
    protected RayCastHit2D[] hitBuffer = new RayCastHit2D[16];
    protected const float shellRadius = 0.01f;
    void Start() {
        contactFilter.useTriggers = false;
        contactFilter.SetLayerMask(Physics2D.GetLayerCollisionMask(gameObject.layer));
        contactFilter.useLayerMask = true;
    }
    void Movement(Vector2 move) {
        float distance = move.magnitude;
        if (distance > minMoveDistance) {
            int count = rb2d.Cast(move, contactFilter, hitBuffer, distance + shellRadius);
        }
        rb2d.position = rb2d.position + move;
    }
}
```

## Scripting Collision

```text
PhysicsObject.cs 


using System.Collection;
using System.Collection.Generic;
using UnityEngine;

public class PhysicsObject: MonoBehaviour {
    protected List < RayCastHit2D > hitBufferList = new List<RayCastHit2D>(16);
    public float minGroundNormalY = 0.65 f;
    protected bool grounded;
    protected Vector2 groundNormal;
    void FixedUpdate() {
        grounded = false;
        move(move, true)
    }
    void Movement(Vector2 move, bool yMovement) {
        if (distance > minMoveDistance) {
            hitBufferList.Clear();
            for (int i = 0; i < count; i ++) {
                hitBufferList.Add(hitBuffer[i]);
            }
            for (int i = 0; i < hitBufferList.count; i ++) {
                Vector2 currentNormal = hitBufferList[i].normal;
                if (currnetNormal.y > minGroundNormalY) {
                    grounded = true;
                    if (yMovement) {
                        groundNormal = currentNormal;
                        currentNormal.x = 0;
                    }
                }
                float projection = Vector2.Dot(velocity, currentNormal);
                if (projection < 0) {
                    velocity = velocity - projection * currentNormal;
                }
                float modifiedDistance = hitBufferList[i].distance - shellRadius;
                distance = modifiedDistance < distance
                    ? modifiedDistance
                    : distance;
            }
        }
    }
}
```

## Horizontal Movement

```text
PhysicsObject.cs


using System.Collection;
using System.Collection.Generic;
using UnityEngine;

public class PhysicsObject: MonoBehaviour {
    protected Vector2 targetVelocity;
    void Update() {
        targetVelocity = Vector2.zero;
        ComputeVelocity();
    }
    void FixedUpdate() {
        velocity.x = targetVelocity.x;
        Vector2 moveAlongGround = new Vector2(groundNormal.y, -groundNormal.x);
        Vector2 move = moveAlongGround * delataPosition.x;
        Movement(move, false);
    }
}
```

```text
PlayerPlatformerController.cs


using System.Collection;
using System.Collection.Generic;
using UnityEngine;

public class PlayerPlatformerController: PhysicsObject {
    public float jumpTakeOffSpeed = 7;
    public float maxSpeed = 7;
    void Start() {}
    void Update() {
        targetVelocity = Vector2.left;
    }
}
```

## Player Controller Script

```text
PhysicsObject.cs


using System.Collection;
using System.Collection.Generic;
using UnityEngine;

public class PhysicsObject: MonoBehaviour {
    protected Vector2 targetVelocity;
    void Update() {
        targetVelocity = Vector2.zero;
        ComputeVelocity();
    }
    void FixedUpdate() {
        velocity.x = targetVelocity.x;
        Vector2 moveAlongGround = new Vector2(groundNormal.y, -groundNormal.x);
        Vector2 move = moveAlongGround * delataPosition.x;
        Movement(move, false);
    }
    protected virtual void ComputeVelocity() {}
}
```

```text
PlayerPlatformerController.cs


using System.Collection;
using System.Collection.Generic;
using UnityEngine;

public class PlayerPlatformerController: PhysicsObject {
    public float jumpTakeOffSpeed = 7;
    public float maxSpeed = 7;
    void Start() {}
    void Update() {}
    protected override void ComputeVelocity() {
        Vector2 move = Vector2.zero;
        move.x = Input.GetAxis("Horizontal");
        if (Input.GetButtonDown("Jump") && grounded) {
            velocity.y = jumpTakeOffSpeed;
        } else if (Input.GetButtonUp("Jump")) {
            if (velocity.y > 0) {
                velocity.y = velocity.y * 0.5 f;
            }
        }
        targetVelocity = move * maxSpeed;
    }

```

## Adding Player Animation

```text
PlayerPlatformerController.cs


using System.Collection;
using System.Collection.Generic;
using UnityEngine;

public class PlayerPlatformerController: PhysicsObject {
    private SpriteRenderer spriteRenderer;
    private Animator animator;
    void Awake() {
        spriteRenderer = GetComponent < SpriteRenderer > ();
        animator = GetComponent < Animator > ();
    }
    protected override void ComputeVelocity() {
        bool filpSprite = (
            spriteRenderer.filpX
                ? (move.x > 0.01 f)
                : (move.x < 0.01 f)
        );
        if (filpSprite) {
            spriteRenderer.filpX = !spriteRenderer.filpX;
        }
        animator.SetBool("grounded", grounded);
        animator.SetFloat("velocityX", Math.Abs(velocity.x) / maxSpeed);
    }
}
```

## etc











#### 





