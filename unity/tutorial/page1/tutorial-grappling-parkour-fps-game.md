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
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=Xgh4v1w5DxU&t=203s" %}



해당 프로젝트는 아래 파일의 압축을 해제하고 Export 하시면 됩니다.

{% file src="../../../.gitbook/assets/parkour-fps-game.zip" caption="Parkour FPS Game" %}

## 작성하기 전 받은 Package

해당 프로젝트를 진행하기 전 다음과 같은 Package를 다운 받으시는 것을 추천드립니다.

* Window -&gt; Package Manager -&gt; 상단의 "+" UI 옆 Box에서 All Packages를 선택하고 다음과 같은 Package를 다운받습니다.
  * Post Processing
  * Probuilder

## 작성법

### Player - Object

Capsule Object를 추가하여 해당 Object의 이동과 카메라 방향으로의 이동을 구현합니다.

![](../../../.gitbook/assets/image%20%28219%29.png)

해당 그림은 Capsule Object를 추가하여 하위 Object로 Cube Object를 생성하여 이름을 Visor로 놓고 다시 Visor Object 아래에 Main Camera를 둔 형태입니다.

### Player - Script

해당 프로젝트에서는 다음과 같이 간단한 클래스 다이어그램을 가지고 있습니다.

![](../../../.gitbook/assets/image%20%28222%29.png)

해당 Script는 다음과 같은 내용으로 구성 되어 있습니다.

{% tabs %}
{% tab title="Playable" %}
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

{% tab title="DetectedLayer" %}
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
            groundColl = Physics.OverlapSphere(body.position + bottomOffset, collRadius, groundLayer);
            if(groundColl.Length != 0)  {  isGround = true;  }
            else  { isGround = false; }

            WallDetected(frontWallColl, frontOffset, wallLayer, ref isFrontWall);
            WallDetected(behindWallColl, behindOffset, wallLayer, ref isBehindWall);
            WallDetected(leftWallColl, leftOffset, wallLayer, ref isLeftWall);
            WallDetected(rightWallColl, rightOffset, wallLayer, ref isRightWall);
            WallDetected(ceilingColl, upperOffset, wallLayer, ref isCeiling);
        }
        public void WallDetected(Collider[] coll, Vector3 offset, LayerMask targetMask, ref bool targetBool)
        {
            coll = Physics.OverlapSphere(body.position + offset, collRadius, targetMask);
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

{% tab title="GrapplingGun" %}
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

    // Start is called before the first frame update
    void Start()
    {
        line = GetComponent<LineRenderer>();
    }

    // Update is called once per frame
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

            line.positionCount = 2f;
            currentGrapplePos = pistolMuzzle.position;
        }
    }

    void DrawRope()
    {
        if (!joint)
            return;

        currentGrapplePos = 
            Vector3.Lerp(currentGrapplePos, grapplePoint, Time.deltaTime * 8f);
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

### Enviroment - Post Processing

* 해당부분은 Post Processing에 관한 기능과 프로젝트에 적용된 Profile에 관하여 설명합니다.

{% embed url="https://unity3d.com/kr/how-to/set-up-post-processing-stack" %}



