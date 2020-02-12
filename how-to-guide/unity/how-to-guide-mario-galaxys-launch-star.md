---
description: How-to-guide Mario Galaxy's Launch Star
---

# How-to-guide Mario Galaxy's Launch Star\(작업중\)

## 무엇을 하려고 하는가?

* Mario Galaxy's Launch Star에 쓰인 Script들을 가지고 Code Review를 합니다.
* 제가 테스트를 하며 작성한 흐름에 따라 정리했기 때문에 읽으실 때 개별 Script로 보시는게 나을것     같습니다.

## Mario Galaxy's Launch Star Code Review

* Jammo\_Player와 LauncherStar Object에 대한 Script들을 별개의 단락으로 설명하는게 좀 더 읽기 편할것 같기때문에 나눠서 기재하겠습니다.



* Jammo\_Player's Scripts 관련 Script

{% tabs %}
{% tab title="MovementInput.cs" %}
{% code title="MovementInput.cs" %}
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
// This script requires you to have setup your animator with 3 parameters, "InputMagnitude", "InputX", "InputZ"
// With a blend tree to control the inputmagnitude and allow blending between animations.
[RequireComponent(typeof(CharacterController))]
public class MovementInput: MonoBehaviour {
    // / <summary>
    // / MovementInput.cs : CharacterController를 사용하여 X,Z값을 받아 움직이고, Animation을 할당
    // / </summary>
    public float Velocity;
    [Space]
    // X값
    public float InputX;
    // Z값
    public float InputZ;
    // 원하는 방향으로 조절
    public Vector3 desiredMoveDirection;
    // Character가 움직임 설정변수
    public bool blockRotationPlayer;
    // Character가 원하는 방향으로 회전하는 속도
    public float desiredRotationSpeed = 0.1 f;
    // Animator
    public Animator anim;
    // 속도
    public float Speed;
    // Character에게 허용된 회전 속도
    public float allowPlayerRotation = 0.1 f;
    public Camera cam;
    public CharacterController controller;
    // 땅에 충돌중인지 확인
    public bool isGrounded;
    [Header("Animation Smoothing")]
    [Range(0, 1 f)]
    public float HorizontalAnimSmoothTime = 0.2 f;
    [Range(0, 1 f)]
    public float VerticalAnimTime = 0.2 f;
    [Range(0, 1 f)]
    public float StartAnimTime = 0.3 f;
    [Range(0, 1 f)]
    public float StopAnimTime = 0.15 f;
    // verticalVel의 값에 따라 땅인지 아닌지 확인
    public float verticalVel;
    // 수직방향 확인 변수
    private Vector3 moveVector;
    // Use this for initialization
    void Start() {
        anim = this.GetComponent<Animator>();
        cam = Camera.main;
        controller = this.GetComponent<CharacterController>();
    }
    // Update is called once per frame
    void Update() {
        InputMagnitude();
        // 움직임이 있을때 isGround가 false가 된다.
        isGrounded = controller.isGrounded;
        if (isGrounded) {
            verticalVel -= 0;
        } else {
            verticalVel -= 1;
        }
        // y값만 따로 계산, Move함수는 속도와 방향을 가지고 deltaTime을 설정해야하기 때문에 미리 곱한다.
        moveVector = new Vector3(0, verticalVel * .2 f * Time.deltaTime, 0);
        controller.Move(moveVector);
    }
    void PlayerMoveAndRotation() {
        InputX = Input.GetAxis("Horizontal");
        InputZ = Input.GetAxis("Vertical");
        var camera = Camera.main;
        var forward = cam.transform.forward;
        var right = cam.transform.right;
        forward.y = 0 f;
        right.y = 0 f;
        forward.Normalize();
        right.Normalize();
        desiredMoveDirection = forward * InputZ + right * InputX;
        if (blockRotationPlayer == false) { // 회전
            transform.rotation = Quaternion.Slerp(transform.rotation, Quaternion.LookRotation(desiredMoveDirection), desiredRotationSpeed);
            // 여기서는 속도를 미리 정해놔서 방향에 곱해주는 형식으로 움직인다. -> CharacterController.Move는 속도에 deltaTime값을 넣어주어야 한다.
            // / 기존의 x, z값을 가지고 새로운 vector를 부여해서 방향을 설정 후 speed를 곱하는 형식으로 움직였는데, charactercontroller를 사용함으로써 앞과는 다른 방법을 써야한다.
            // / 왜냐하면 좀더 많은 제어가 필요한 경우에 Move함수를 사용하기 때문이다.
            // / Character를 Speed로 이동시킵니다.
            controller.Move(desiredMoveDirection * Time.deltaTime * Velocity);
        }
    }
    public void LookAt(Vector3 pos) {
        transform.rotation = Quaternion.Slerp(transform.rotation, Quaternion.LookRotation(pos), desiredRotationSpeed);
    }
    public void RotateToCamera(Transform t) {
        var camera = Camera.main;
        var forward = cam.transform.forward;
        var right = cam.transform.right;
        desiredMoveDirection = forward;
        t.rotation = Quaternion.Slerp(transform.rotation, Quaternion.LookRotation(desiredMoveDirection), desiredRotationSpeed);
    }
    // / <summary>
    // / X, Z값의 크기를 구하는 함수
    // / Next : PlayerMoveAndRotation
    // / </summary>
    void InputMagnitude() { // Calculate Input Vectors
        InputX = Input.GetAxis("Horizontal");
        InputZ = Input.GetAxis("Vertical");
        // anim.SetFloat ("InputZ", InputZ, VerticalAnimTime, Time.deltaTime * 2f);
        // anim.SetFloat ("InputX", InputX, HorizontalAnimSmoothTime, Time.deltaTime * 2f);
        // Calculate the Input Magnitude | Vector길이의 제곱한 값을 반환
        // / 크기(magnitude)의 제곱을 계산하는 것이.magnitude를 사용하는 것보다 훨씬 빠릅니다. 두 벡터의 크기를 비교하는 경우에, 크기를 제곱한 값을 사용해서 비교할 수 있습니다.
        Speed = new Vector2(InputX, InputZ).sqrMagnitude;
        // Physically move player
        if (Speed > allowPlayerRotation) {
            // / public void SetFloat(string name, float value, float dampTime, float deltaTime);
            // / Blend Tree Animation이 사용되었다.
            // / // https://wergia.tistory.com/54?category=739103
            anim.SetFloat("Blend", Speed, StartAnimTime, Time.deltaTime);
            PlayerMoveAndRotation();
        } else if (Speed < allowPlayerRotation) {
            anim.SetFloat("Blend", Speed, StopAnimTime, Time.deltaTime);
        }
    }
}
Inou
```
{% endcode %}
{% endtab %}

{% tab title="CharacterSkinController.cs" %}
{% code title="CharacterSkinController.cs" %}
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
public class CharacterSkinController: MonoBehaviour {
    Animator animator;
    Renderer[] characterMaterials;
    public Texture2D[] albedoList;
    [ColorUsage(true, true)] public Color[] eyeColors;
    public enum EyePosition {
        normal,
        happy,
        angry,
        dead
    }
    public EyePosition eyeState;
    // Start is called before the first frame update
    void Start() {
        animator = GetComponent < Animator > ();
        characterMaterials = GetComponentsInChildren < Renderer > ();
    }
    // Update is called once per frame
    void Update() {
        if (Input.GetKeyDown(KeyCode.Alpha1)) { // ChangeMaterialSettings(0);
            ChangeEyeOffset(EyePosition.normal);
            ChangeAnimatorIdle("normal");
        }
        if (Input.GetKeyDown(KeyCode.Alpha2)) { // ChangeMaterialSettings(1);
            ChangeEyeOffset(EyePosition.angry);
            ChangeAnimatorIdle("angry");
        }
        if (Input.GetKeyDown(KeyCode.Alpha3)) { // ChangeMaterialSettings(2);
            ChangeEyeOffset(EyePosition.happy);
            ChangeAnimatorIdle("happy");
        }
        if (Input.GetKeyDown(KeyCode.Alpha4)) { // ChangeMaterialSettings(3);
            ChangeEyeOffset(EyePosition.dead);
            ChangeAnimatorIdle("dead");
        }
    }
    void ChangeAnimatorIdle(string trigger) {
        animator.SetTrigger(trigger);
    }
    void ChangeMaterialSettings(int index) {
        for (int i = 0; i < characterMaterials.Length; i ++) {
            if (characterMaterials[i].transform.CompareTag("PlayerEyes")) 
                characterMaterials[i].material.SetColor("_EmissionColor", eyeColors[index]);
             else 
                characterMaterials[i].material.SetTexture("_MainTex", albedoList[index]);
            
        }
    }
    void ChangeEyeOffset(EyePosition pos) {
        Vector2 offset = Vector2.zero;
        switch (pos) {
            case EyePosition.normal:
                offset = new Vector2(0, 0);
                break;
            case EyePosition.happy:
                offset = new Vector2(.33 f, 0);
                break;
            case EyePosition.angry:
                offset = new Vector2(.66 f, 0);
                break;
            case EyePosition.dead:
                offset = new Vector2(.33 f, .66 f);
                break;
            default:
                break;
        }
        for (int i = 0; i < characterMaterials.Length; i ++) {
            if (characterMaterials[i].transform.CompareTag("PlayerEyes")) 
                characterMaterials[i].material.SetTextureOffset("_MainTex", offset);
            
        }
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}



* LauncherStar Object 관련 Script

{% tabs %}
{% tab title="First Tab" %}

{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

