---
description: PhysicsObject tutorial에 이은 How-to-guide
---

# How-to-guide PhysicsObject

## 무엇을 하려고 하는가?

* tutorial보다 자세한 설명 및 작성 단계를 표시합니다.
* tutorial 영상을 보고 각 단계에 따른 리뷰를 작
* 작성 단계에서 나타나는 문제점을 이의제기, 해결방법 등을 업데이트 할 예정입니다.

## Scripting Gravity

```text
public float gravityModifier = 1f;            // 중력 변수
protected RigidBody rb2d;                      // RigidBody2D Component를 가져오는 변수
protected Vector2 velocity;                    // RigidBody를 움직이기 위한 Vector2 변

void OnEnable() {
    rb2d = GetComponent < RigidBody2D > ();    
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
```



## Detecting Overlaps

```text
protected const float minMoveDistance = 0.001f;
protected ContactFilter2D contactFilter;
protected RayCastHit2D[] hitBuffer = new RayCastHit2D[16];
protected const float shellRadius = 0.01f;

void Start() {
    contactFilter.useTrigger = false;
    contactFilter.SetLayerMask(Physics2D.GetLayerCollisionMask(gameObject.layer));
    contactFilter.useLayerMask = true;
}

void Movement(Vector2 move) {
    float distance = move.magnitude;
    if(distance > minMoveDistance) {
        int count = rb2d.Cast(move, contactFilter, hitBuffer, distance + shellRadius);
    }
    rb2d.position = rb2d.position + move;
}
```



## Scripting Collision

```text
protected List<RayCastHit2D>hitBufferList = new List<RayCastHit2D>(16);
public float minGroundNormalY = 0.65f;
protected bool grounded;
protected Vector2 groundNormal;

void FixedUpdate()
{
    grounded = false;
    move(move, true) // 교
}


void Movement(Vector2 move, bool yMovement)
{
    hitBufferList.Clear();
    for(int i = 0; i < count; i++)
    {
        hitBufferList.Add(hitBuffer[i]);
    }
    for(int i = 0; i < hitBufferList.count; i++)
    {
        Vector2 currentNormal = hitBufferList[i].normal;
        if(currnetNormal.y > minGroundNormalY)
        {
            grounded = true;
            if(yMovement)
            {
                groundNormal = currentNormal;
                currentNormal.x = 0;
            }
        }
        
        float projection = Vector2.Dot(velocity, currentNormal);
        if(projection < 0)
        {
            velocity = velocity - projection * currentNormal;
        }
        
        float modifiedDistance = hitBufferList[i].distance - shellRadius;
        distance = modifiedDistance < distance ? modifiedDistance : distance;
    }
}

```

## Horizontal Movement



## Player Controller Script



## Adding Player Animation





## etc











#### 




