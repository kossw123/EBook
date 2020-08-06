---
description: tutorial Grappling Parkour FPS Game
---

# tutorial Grappling Parkour FPS Game

## 무엇을 하려고 하는가?

사이드 프로젝트를 진행하면서, 현재 진행한 프로젝트에 관한 내용을 정리하려고 합니다.

* 다음과 같은 기능들이 해당 프로젝트 내부에 존재합니다.
  * First Person View의 Rigidbody를 이용한 Movement
  * Physics.Overlap Component를 이용한 충돌 지점 판별 여부
  * Spring Joint을 이용한 Rigidbody의 연결, Line Renderer를 이용한 Spring Joint의 표시
  * Probuilder를 통한 맵제작
  * Post Processing을 이용한 Global Volume 처리
  * UnityEvent를 이용한 상태변수 Callback System 기초 구현
    * 해당 Callback System 관련된 부분은 기존 프로젝트에 아직 미적용 했기 때문에, 다른 프로젝트에서 구현한 내용을 적용할 예정입니다.
    * 그렇기 때문에, 구현이 된 기능들과 Callback System을 구현한 내용이 연동이 안될 수 있습니다.

{% hint style="warning" %}
Tutorial을 보시기 전 주의점

* 해당 문서를 작성하는 이유는 위의 기능의 추가 중 Callback System을 도입하고나서, 즉흥적으로 기능을 추가하는 것에 대해 한계를 느껴 해당 내용을 정리하고자 작성됨을 알립니다.
* **즉, 설계가 즉흥적으로 추가한 것이다 보니, 해당 프로젝트를 가지고 첨삭하는 것을 추천하지 않습니다.**
* Grapple 기능에 대해서는 아래의 영상에서 R&D를 했습니다.
  * Grappling Gun 기능은 아래의 영상을 참조하였습니다.
  * 해당문서는 상업적으로 이용되지 않음을 알립니다.
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=Xgh4v1w5DxU&t=203s" %}



해당 프로젝트는 아래 파일의 압축을 해제하고 Export 하시면 됩니다.

{% file src="../../../.gitbook/assets/parkour-fps-game.zip" caption="Parkour FPS Game" %}

## 작성하기 전 받은 Package

해당 프로젝트를 진행하기 전 다음과 같은 Package를 다운 받으시는 것을 추천드립니다.

* Window -&gt; Package Manager -&gt; 상단의 "+" UI 옆 Box에서 All Packages를 선택하고 다음과 같은 Package를 다운받습니다.
  * Post Processing
  * Probuilder

## 작성하기 전 프로젝트의 Class diagram

해당 프로젝트는 4개의 기능 Class로 구현되어 있습니다.

![](../../../.gitbook/assets/image%20%28228%29.png)

이렇게 구성한 이유는 사이드 프로젝트의 성격상 그때그때의 생각난 기능들을 구현했기 때문이고, 이를 바탕으로 Callback System을 적용한 Class diagram을 다시 제작할 예정입니다.

## 작성법

### Player

해당 내용에서는 다음 내용과 같은 목차가 있습니다.

1. Player - Movement
2. Player - Detected Layer
3. Player - Grappling

## 

### Player - Movement

좀 더 물리적인 느낌을 주기 위해 다음과 같이 구성했습니다.

* 카메라의 회전과 동시에 회전 방향으로 Object가 이동
* Rigidbody를 이용하여 이동할 위치로 Velocity\(속도\)를 줘서, 물리엔진을 사용하는 이동을 구현

Object의 배치는 아래의 그림과 같습니다.

![](../../../.gitbook/assets/image%20%28225%29.png)

위 그림은 Capsule Object를 추가하여 하위 Object로 Cube Object를 생성하여 이름을 Visor로 놓고 다시 Visor Object 아래에 Main Camera를 둔 형태입니다.

Gun Object는 Probuilder를 통한 Mesh 제작을 통해 Pipe모양의 Mesh를 적용하고 Muzzle이라는 Empty Object을 총구처럼 두고 쓰고 있습니다.

4개의 Class로 중 Movement에 관련된 Class는 2개입니다.

* Playerable라는 추상 클래스
* PlayerMovement라는 움직임 전반을 담당하는 Class

이를 바탕으로 Movement에 관련한 Class의 내용을 다음과 같습니다.

{% tabs %}
{% tab title="Playerable" %}
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Events;

namespace FPSGame.AbstractClass
{
    public abstract class Playerable : MonoBehaviour
    {
        public abstract void MyInput();
        public abstract void Movement();
        public abstract void Rotation();

        public abstract void Slide();
        public abstract void Jump();
        public abstract void Crouch();
    }
}


```
{% endtab %}

{% tab title="PlayerMovement" %}
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using FPSGame.AbstractClass;
using FPSGame.Helper;

namespace FPSGame.Movement
{
    public class PlayerMovement : Playerable
    {
        [Header("Physics Movement")]
        public bool PhysicsMovement = false;
        [Space]

        //[Header("Require Component")]
        public Transform PlayerBody;
        public Transform cameraArm;
        private Rigidbody rb;

        private float axisX;
        private float axisZ;
        private Vector3 movement;
        [Range(10, 100)] public float Sensitive = 50f;
        private float xRotation = 0f;

        [Header("Character float Property")]  
        [Range(1, 20)] public float speed;
        public float jump;
        [Range(5, 10)] public float slide;

      

        [Header("Character bool Property")]
        public bool readyJump = true;
        public bool readySlide = true;
        public bool readyCrouch = true;

        [Header("Button KeyDown")]
        public bool isJump = false;
        public bool isSlide = false;
        public bool isCrouch = false;

        private DetectedLayer detect;

        [Header("Editor Variable")]
        public Vector3 playerScale;
        Vector3 crouchScale = new Vector3(1, 0.5f, 1);

        public override void MyInput()
        {
            axisX = Input.GetAxis("Horizontal") * Time.deltaTime * speed;
            axisZ = Input.GetAxis("Vertical") * Time.deltaTime * speed;
            isJump = Input.GetButton("Jump");
            isSlide = Input.GetButton("Slide");
            isCrouch = Input.GetButtonDown("Crouch");
        }

        public override void Movement()
        {
            if(!PhysicsMovement)
            {
                Vector3 lookForward = new Vector3(cameraArm.forward.x, 0, cameraArm.forward.z).normalized;
                Vector3 lookRight = new Vector3(cameraArm.right.x, 0, cameraArm.right.z).normalized;
                Vector3 movement = lookForward * axisZ + lookRight * axisX;
                rb.MovePosition(PlayerBody.position + movement);
            }

            else if(PhysicsMovement)
            {
                float physicsSpeed = 10f;
                float maxVelocityChange = 10.0f;
                rb.AddForce(Vector3.down * rb.mass);

                Vector3 targetVelocity = new Vector3(Input.GetAxis("Horizontal"), 0, Input.GetAxis("Vertical"));
                targetVelocity = transform.TransformDirection(targetVelocity);
                targetVelocity *= physicsSpeed;

                Vector3 velocity = rb.velocity;
                Vector3 velocityChange = (targetVelocity - velocity);
                velocityChange.x = Mathf.Clamp(velocityChange.x, -maxVelocityChange, maxVelocityChange);
                velocityChange.z = Mathf.Clamp(velocityChange.z, -maxVelocityChange, maxVelocityChange);
                velocityChange.y = 0;
                rb.AddForce(velocityChange, ForceMode.VelocityChange);
            }

        }

        public override void Rotation()
        {
            float mouseX = Input.GetAxis("Mouse X") * Sensitive * Time.deltaTime;
            float mouseY = Input.GetAxis("Mouse Y") * Sensitive * Time.deltaTime;

            xRotation -= mouseY;
            xRotation = Mathf.Clamp(xRotation, -70, 90);

            cameraArm.transform.localRotation = Quaternion.Euler(xRotation, 0, 0);
            PlayerBody.Rotate(Vector3.up * mouseX);
        }


        public override void Slide()
        {
            if (readySlide && detect.isGround)
            {
                readySlide = false;
                rb.AddForce(slide * cameraArm.forward, ForceMode.Impulse);
                Invoke("ResetSlide", 1f);
            }
        }
        public void ResetSlide()
        {
            readySlide = true;
        }


        public override void Jump()
        {
            if(readyJump)
            {
                readyJump = false;
                rb.AddForce(PlayerBody.position * jump * Vector2.up, ForceMode.Impulse);
                
                Invoke("ResetJump", 1f);
            }
        }
        public void ResetJump()
        {
            readyJump = true;
        }
        

        private void Start() {
            rb = GetComponent<Rigidbody>();
            Cursor.lockState = CursorLockMode.Locked;
            detect = GetComponent<DetectedLayer>();
            playerScale = PlayerBody.localScale;
        }

        void FixedUpdate() {

            Movement();
            if (readySlide && isSlide) Slide();
            if (readyJump && isJump) Jump();

            if (readyCrouch && isCrouch) Crouch();
            if (!readyCrouch && isCrouch) ResetCrouch();
        }

        private void Update() {
            MyInput();
            Rotation();
        }

        public override void Crouch()
        {
            if (readyCrouch && detect.isGround)
            {
                readyCrouch = false;
                PlayerBody.localScale = crouchScale;
                PlayerBody.position = new Vector3(PlayerBody.position.x, PlayerBody.position.y - 0.5f, PlayerBody.position.z);
            }
        }

        public void ResetCrouch()
        {
            PlayerBody.localScale = playerScale;
            PlayerBody.position = new Vector3(PlayerBody.position.x, PlayerBody.position.y + 0.5f, PlayerBody.position.z);
        }
    }
}
```
{% endtab %}
{% endtabs %}

## 

### Player - Detected layer

Rigidbody를 이용한 움직임을 구현하다 보니, Object가 다른 물체에 충돌 시 프레임이 끊기면서, 이상한 충돌을 일으키는 현상을 발견했습니다. 

이를 해결하기 위해, 나중에 추가할 다른 움직임을 위해, Detected Layer라는 기능 Class를 제작하여 충돌 시 Kinematic이 true가 되도록 작성했습니다.

Object의 배치는 다음과 같습니다.

![](../../../.gitbook/assets/image%20%28226%29.png)

OnDrawGizmos\(\) 함수를 통해 원 모양의 Gizmos를 Editor상에 띄울 수 있도록 작업 했기에 중간의 원 모양이 나타납니다.

Inspector가 아닌 Script로 설정하여, 바로 적용할 수 있는 Class는 다음과 같습니다.

* Detected Layer라는 충돌 부위 감지 Class

이를 바탕으로 Detected Layer에 관한 내용은 아래와 같습니다.

{% tabs %}
{% tab title="Detected Layer" %}
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using FPSGame.Movement;

namespace FPSGame.Helper
{
    public class DetectedLayer : MonoBehaviour
    {
        public Transform body;
        public LayerMask groundLayer;
        public LayerMask wallLayer;

        [Space] [Header("Collision Property")]
        private Collider[] groundColl;
        private Collider[] frontWallColl;
        private Collider[] behindWallColl;
        private Collider[] leftWallColl;
        private Collider[] rightWallColl;
        private Collider[] ceilingColl;
        private float collRadius = 0.25f;

        [Space]
        [Header("Boolean Property")]
        public bool isGround;
        public bool isFrontWall;
        public bool isBehindWall;
        public bool isLeftWall;
        public bool isRightWall;
        public bool isCeiling;

        [Space]
        [Header("Offset Property")]
        public Vector3 bottomOffset;
        public Vector3 frontOffset;
        public Vector3 behindOffset;
        public Vector3 leftOffset;
        public Vector3 rightOffset;
        public Vector3 upperOffset;

        public void InitializeOffset()
        {
            bottomOffset = -body.up;
            upperOffset = body.up;
            frontOffset = body.forward;
            behindOffset = -body.forward;
            leftOffset = -body.right;
            rightOffset = body.right;
        }
        public void DetectedToTerrain()
        {
            // Ground
            groundColl = Physics.
                OverlapSphere(body.position + bottomOffset, collRadius, groundLayer);
            if(groundColl.Length != 0)  {  isGround = true;  }
            else  { isGround = false; }

            WallDetected(frontWallColl, frontOffset, wallLayer, ref isFrontWall);
            WallDetected(behindWallColl, behindOffset, wallLayer, ref isBehindWall);
            WallDetected(leftWallColl, leftOffset, wallLayer, ref isLeftWall);
            WallDetected(rightWallColl, rightOffset, wallLayer, ref isRightWall);
            WallDetected(ceilingColl, upperOffset, wallLayer, ref isCeiling);
        }
        public void WallDetected(Collider[] coll, Vector3 offset, 
                                LayerMask targetMask, ref bool targetBool)
        {
            coll = Physics.OverlapSphere(body.position + offset, 
                        collRadius, targetMask);
            if(coll.Length != 0) 
            { 
                targetBool = true;
                body.GetComponent<Rigidbody>().isKinematic = true;
            }
            else 
            { 
                targetBool = false;
                body.GetComponent<Rigidbody>().isKinematic = false;
            }
        }
        private void OnDrawGizmos()
        {
            Gizmos.color = Color.red;
            var position = new Vector3[] { 
                bottomOffset, upperOffset, 
                frontOffset, behindOffset, 
                leftOffset, rightOffset 
            };

            foreach(var item in position)
            {
                Gizmos.DrawWireSphere((Vector3)body.position + item, collRadius);
            }
        }
        private void Update()
        {
            InitializeOffset();
            DetectedToTerrain();
        }
    }
}

```
{% endtab %}
{% endtabs %}

## 

### Player - Grappling

대형 Unity Youtuber인 dani의 Grappling Gun을 이용한 Parkour Game이 존재함을 알게 되었고 이를 사용하여 Grappling Gun 기능을 구현했습니다. 

Object의 배치는 다음과 같습니다.

![](../../../.gitbook/assets/image%20%28223%29.png)

Gun Object를 보시면 간단하게 Gun Object와 하위 Object인 Muzzle Object만 존재합니다.

![](../../../.gitbook/assets/image%20%28222%29.png)

{% hint style="warning" %}
Probuilder를 통하여 작성 했기 때문에, 위와 같은 그림이 나오는 경우 오류가 아님을 알립니다.
{% endhint %}

Grappling Gun 기능의 대한 Class는 다음과 같습니다.

* grapplingGun라는 Class

위 기능 Class에 대한 기능은 다음과 같습니다.

{% tabs %}
{% tab title="grapplingGun" %}
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class grapplingGun : MonoBehaviour
{
    [SerializeField] private LineRenderer line;
    [SerializeField] private Vector3 grapplePoint;
    public LayerMask whatIsGrappleable;
    public Transform Player, playerCamera, pistolMuzzle;
    private SpringJoint joint;

    public float lineDistance = 100f;
    public Vector3 currentGrapplePos;

    void Start()
    {
        line = GetComponent<LineRenderer>();
    }

    void Update()
    {
        if (Input.GetMouseButtonDown(0))
        {
            StartGrapple();
        }
        else if (Input.GetMouseButtonUp(0))
        {
            StopGrapple();
        }
    }

    private void LateUpdate()
    {
        DrawRope();
    }

    public void StartGrapple()
    {
        RaycastHit hitInfo;
        if (Physics.Raycast(playerCamera.position, playerCamera.forward, 
                            out hitInfo, lineDistance))
        {
            grapplePoint = hitInfo.point;

            joint = Player.gameObject.AddComponent<SpringJoint>();
            joint.autoConfigureConnectedAnchor = false;
            joint.connectedAnchor = grapplePoint;

            float distanceFromPoint = Vector3.Distance(Player.position, grapplePoint);

            joint.maxDistance = distanceFromPoint * 0.8f;
            joint.minDistance = distanceFromPoint * 0.25f;

            joint.spring = 4.5f;
            joint.damper = 7f;
            joint.massScale = 4.5f;

            line.positionCount = 2;
            currentGrapplePos = pistolMuzzle.position;
        }
    }

    void DrawRope()
    {
        if (!joint)
            return;

        currentGrapplePos = Vector3.Lerp(currentGrapplePos, grapplePoint, 
                            Time.deltaTime * 8f);
        line.SetPosition(0, pistolMuzzle.position);
        line.SetPosition(1, currentGrapplePos);
    }

    public void StopGrapple()
    {
        line.positionCount = 0;
        Destroy(joint);
    }
}

```
{% endtab %}
{% endtabs %}

## 

### Enviroment

해당 내용에는 다음과 같은 목차로 구성되어 있습니다.

1. Post Processing
2. Probuilder

### **Enviroment - Post Processing**

* 해당부분은 Post Processing에 관한 기능과 프로젝트에 적용된 Profile에 관하여 설명합니다.

Post Processing이란?

카메라 이미지 버퍼가 화면에 표시되기 전에 화면 전체에 필터 및 효과를 적용하는 과정입니다.

다음과 같은 과정을 거쳐서 적용시킵니다.

1. 카메라에 Post-Process Layer Component 적용
2. Post Profile을 Project View에서 생성하여 적절한 효과를 부여
3. Empty Object를 생성하여 Post-process Volume을 적용하여 작성한 Profile을 해당 Component 삽

{% embed url="https://unity3d.com/kr/how-to/set-up-post-processing-stack" %}

작성자는 아래와 같이 적용했습니다.

![](../../../.gitbook/assets/image%20%28230%29.png)

## 

### Enviroment - ProBuilder

* 해당 부분은 Probuilder에 관한 설명을 합니다.

ProBuilder란?

Unity 자체 Package로써, 지형지물을 제작을 좀 더 편리하게 적용할 수 있는 Package입니다. 간단하게 Blender나 3Ds Max, Maya 같은 Unity 내장 Tool이라고 생각하시면 됩니다.

{% embed url="https://unity3d.com/kr/unity/features/worldbuilding/probuilder" %}

## 마치며

* 위의 내용을 가지고 How-to-guide에서 마저 설명하도록 하겠습니다.

{% page-ref page="../../how-to-guide/unity/how-to-guide-grappling-parkour-fps-game.md" %}

