---
description: How-to-guide Mario Galaxy's Launch Star
---

# How-to-guide Mario Galaxy's Launch Star\(작업중\)

## 무엇을 하려고 하는가?

* Mario Galaxy's Launch Star에 쓰인 Script들을 가지고 Code Review를 합니다.
* 제가 테스트를 하며 작성한 흐름에 따라 정리했기 때문에 읽으실 때 개별 Script로 보시는게 나을것     같습니다.
* DoTween의 Sequnce, Join 함수 등은 동영상에서 보시는 것이 이해가 더빠를 수도 있으니, 초반 시청 바랍니다.
* 이 문서는 상업적인 목적으로 만들어지지 않았습니다.



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
{% tab title="StarLauncher.cs" %}
{% code title="StarLauncher.cs" %}
```csharp
using System.Collections;
using UnityEngine;
using DG.Tweening;
using Cinemachine;
public class StarLauncher: MonoBehaviour {
    Animator animator;
    MovementInput movement;
    StarAnimation starAnimation;
    TrailRenderer trail;
    public AnimationCurve pathCurve;
    [Range(0, 50)] public float speed = 10 f;
    float speedModifier = 1;
    [Space][Header("Booleans")] public bool insideLaunchStar;
    public bool flying;
    public bool almostFinished;
    // 여기서 Dolly Cart Object를 return
    public Transform launchObject;
    [Space][Header("Public References")] public CinemachineFreeLook thirdPersonCamera;
    public CinemachineDollyCart dollyCart;
    float cameraRotation;
    // / <summary>
    // / 이 변수를 이용해서 Player Character를 임시로 담는다.
    // / </summary>
    public Transform playerParent;
    [Space][Header("Launch Preparation Sequence")] public float prepMoveDuration = .15 f;
    public float launchInterval = .5 f;
    [Space][Header("Particles")] public ParticleSystem followParticles;
    public ParticleSystem smokeParticle;
    void Start() {
        animator = GetComponent < Animator > ();
        movement = GetComponent < MovementInput > ();
        trail = dollyCart.GetComponentInChildren<TrailRenderer>();
    }
    void Update() {
        if (insideLaunchStar) 
            if (Input.GetKeyDown(KeyCode.Space)) 
                StartCoroutine(CenterLaunch());
            
        
        if (flying) {
            animator.SetFloat("Path", dollyCart.m_Position);
            playerParent.transform.position = dollyCart.transform.position;
            if (!almostFinished) {
                playerParent.transform.rotation = dollyCart.transform.rotation;
            }
        }
        if (dollyCart.m_Position > .7 f && !almostFinished && flying) {
            almostFinished = true;
            // thirdPersonCamera.m_XAxis.Value = cameraRotation;
            playerParent
                .DORotate(new Vector3(360 + 180, 0, 0), .5 f, RotateMode.LocalAxisAdd)
                .SetEase(Ease.Linear)
                .OnComplete(() => playerParent.DORotate(new Vector3(-90, playerParent.eulerAngles.y, playerParent.eulerAngles.z), .2 f));
        }
        // Debug
        if (Input.GetKeyDown(KeyCode.O)) 
            Time.timeScale = .2 f;
        
        if (Input.GetKeyDown(KeyCode.P)) 
            Time.timeScale = 1 f;
        
        if (Input.GetKeyDown(KeyCode.R)) 
            UnityEngine
                .SceneManagement
                .SceneManager
                .LoadSceneAsync(UnityEngine
                    .SceneManagement
                    .SceneManager
                    .GetActiveScene()
                    .name);
        
    }
    // / <summary>
    // / 1번째로 실행되는 함수 StartCoroutine을 사용한 함수
    // / </summary>
    // / <returns></returns>
    IEnumerator CenterLaunch() { // 조작권을 뺏는다
        movement.enabled = false;
        // transform, 즉 Jammo의 parent를 null로 만든다. <- 그 이전에 parent가 있었다는 의미인데 어디에?
        transform.parent = null;
        // DoTween을 모두 Kill
        DOTween.KillAll();
        // Checks to see if there is a Camera Trigger at the DollyTrack object - if there is activate its camera
        if (launchObject.GetComponent<CameraTrigger>() != null) 
            launchObject.GetComponent<CameraTrigger>().SetCamera();
        
        // Checks to see if there is a Camera Trigger at the DollyTrack object - if there is activate its camera
        // / 재밌는게 Script에 변수 한줄만 있는데이것을 Component로 받아와서 다시 할당한다는게 재밌는 구조인데, 이러한 구조의 이유는
        // / Track의 각자의 속도를 다르게 하기 위함인데, 똑같은 Script를 재활용 했던 방법이 좋았다.
        if (launchObject.GetComponent<SpeedModifier>() != null) 
            speedModifier = launchObject.GetComponent<SpeedModifier>().modifier;
        
        // Checks to see if there is a Star Animation at the DollyTrack object
        // / 다시 주소를 받아와야 한다. 왜냐하면 Object의 저장 위치 주소가 다 다르기 때문이다.
        if (launchObject.GetComponentInChildren<StarAnimation>() != null) 
            starAnimation = launchObject.GetComponentInChildren<StarAnimation>();
        
        dollyCart.m_Position = 0;
        dollyCart.m_Path = null;
        // 기존에 Dolly Cart에 있는 CinemachineSmoothPath를 가져온다.
        dollyCart.m_Path = launchObject.GetComponent<CinemachineSmoothPath>();
        dollyCart.enabled = true;
        // / 2번의 yield문을 써야 제대로 돌아감
        // / 적어도 2프레임을 소모해야 제대로 움직인다는 것인데 이것에 대한 정확한 이유가 필요
        yield return new WaitForEndOfFrame();
        yield return new WaitForEndOfFrame();
        // / 움직임을 위한 DoTweening 함수들
        Sequence CenterLaunch = DOTween.Sequence();
        CenterLaunch.Append(transform.DOMove(dollyCart.transform.position, .2 f));
        CenterLaunch.Join(transform.DORotate(dollyCart.transform.eulerAngles + new Vector3(90, 0, 0), .2 f));
        CenterLaunch.Join(starAnimation.Reset(.2 f));
        // / OnComplete()는 콜백이 끝난 후 바로 호출할 함수를 정하는것
        CenterLaunch.OnComplete(() => LaunchSequence());
    }
    // / <summary>
    // / DoTween의 Sequnce는 Tweener와 다르게 Animate까지 포함 가능
    // / </summary>
    // / <returns></returns>
    Sequence LaunchSequence() {
        float distance;
        CinemachineSmoothPath path = launchObject.GetComponent<CinemachineSmoothPath>();
        float finalSpeed = path.PathLength / (speed * speedModifier);
        cameraRotation = transform.eulerAngles.y;
        // / LaunchObject(별모양 오브젝트)의 position을 playerParent의 Position에 넣는다.
        playerParent.transform.position = launchObject.position;
        playerParent.transform.rotation = transform.rotation;
        // / 애니메이터 parameter Set
        flying = true;
        animator.SetBool("flying", true);
        Sequence s = DOTween.Sequence();
        // / playerParent의 위치(별모양 Object의 위치)
        s.AppendCallback(() => transform.parent = playerParent.transform); // Attatch the player to the empty gameObject
        s.Append(transform.DOMove(transform.localPosition - transform.up, prepMoveDuration)); // Pull player a little bit back
        s.Join(transform.DOLocalRotate(new Vector3(0, 360 * 2, 0), prepMoveDuration, RotateMode.LocalAxisAdd).SetEase(Ease.OutQuart));
        s.Join(starAnimation.PullStar(prepMoveDuration));
        s.AppendInterval(launchInterval); // Wait for a while before the launch
        s.AppendCallback(() => trail.emitting = true);
        s.AppendCallback(() => followParticles.Play());
        s.Append(DOVirtual.Float(dollyCart.m_Position, 1, finalSpeed, PathSpeed).SetEase(pathCurve)); // Lerp the value of the Dolly Cart position from 0 to 1
        s.Join(starAnimation.PunchStar(.5 f));
        s.Join(transform.DOLocalMove(new Vector3(0, 0, -.5 f), .5 f)); // Return player's Y position
        s.Join(transform.DOLocalRotate(new Vector3(0, 360, 0), // Slow rotation for when player is flying
                (finalSpeed / 1.3 f), RotateMode.LocalAxisAdd)).SetEase(Ease.InOutSine);
        s.AppendCallback(() => Land()); // Call Land Function
        return s;
    }
    void Land() {
        playerParent.DOComplete();
        dollyCart.enabled = false;
        dollyCart.m_Position = 0;
        movement.enabled = true;
        transform.parent = null;
        flying = false;
        almostFinished = false;
        animator.SetBool("flying", false);
        followParticles.Stop();
        trail.emitting = false;
    }
    public void PathSpeed(float x) {
        dollyCart.m_Position = x;
    }
    public void PlaySmoke() {
        CinemachineImpulseSource[] impulses = FindObjectsOfType < CinemachineImpulseSource > ();
        for (int i = 0; i < impulses.Length; i ++) 
            impulses[i].GenerateImpulse();
        
        smokeParticle.Play();
    }
    private void OnTriggerEnter(Collider other) {
        if (other.CompareTag("Launch")) {
            insideLaunchStar = true;
            launchObject = other.transform;
        }
        if (other.CompareTag("CameraTrigger")) 
            other.GetComponent<CameraTrigger>().SetCamera();
        
    }
    private void OnTriggerExit(Collider other) {
        if (other.CompareTag("Launch")) {
            insideLaunchStar = false;
        }
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="StarAnimation.cs" %}
{% code title="StarAnimation.cs" %}
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using DG.Tweening;
using Cinemachine;
// / <summary>
// / starLauncher 쏘기 위한 ParticleScript
// / </summary>
public class StarAnimation: MonoBehaviour {
    Animator animator;
    Transform big;
    Transform small;
    public AnimationCurve punch;
    [Space][Header("Particles")] public ParticleSystem glow;
    public ParticleSystem charge;
    public ParticleSystem explode;
    public ParticleSystem smoke;
    // / <summary>
    // / GetComponent
    // / </summary>
    private void Start() {
        animator = GetComponent < Animator > ();
        big = transform.GetChild(0);
        small = transform.GetChild(1);
    }
    // / <summary>
    // / DoTweening에서 사용할 Sequence.Reset
    // / </summary>
    // / <param name="time">시간 조절함수</param>
    // / <returns></returns>
    public Sequence Reset(float time) {
        animator.enabled = false;
        Sequence s = DOTween.Sequence();
        s.Append(big.DOLocalRotate(Vector3.zero, time).SetEase(Ease.InOutSine));
        s.Join(small.DOLocalRotate(Vector3.zero, time).SetEase(Ease.InOutSine));
        return s;
    }
    public Sequence PullStar(float pullTime) {
        glow.Play();
        charge.Play();
        Sequence s = DOTween.Sequence();
        s.Append(big.DOLocalRotate(new Vector3(0, 0, 360 * 2), pullTime, RotateMode.LocalAxisAdd).SetEase(Ease.OutQuart));
        s.Join(small.DOLocalRotate(new Vector3(0, 0, 360 * 2), pullTime, RotateMode.LocalAxisAdd).SetEase(Ease.OutQuart));
        s.Join(small.DOLocalMoveZ(-4.2 f, pullTime));
        return s;
    }
    public Sequence PunchStar(float punchTime) {
        CinemachineImpulseSource[] impulses = FindObjectsOfType < CinemachineImpulseSource > ();
        animator.enabled = false;
        Sequence s = DOTween.Sequence();
        s.AppendCallback(() => explode.Play());
        s.AppendCallback(() => smoke.Play());
        s.AppendCallback(() => impulses[0].GenerateImpulse());
        s.Append(small.DOLocalMove(Vector3.zero, .8 f).SetEase(punch));
        s.Join(small.DOLocalRotate(new Vector3(0, 0, 360 * 2), .8 f).SetEase(Ease.OutBack));
        s.AppendInterval(.8 f);
        s.AppendCallback(() => animator.enabled = true);
        return s;
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="CameraTrigger.cs" %}
{% code title="CameraTrigger.cs" %}
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Cinemachine;

/// <summary>
/// 
/// </summary>
public class CameraTrigger : MonoBehaviour
{
    CinemachineBrain brain;
    Transform camerasGroup;

    [Header("Camera Settings")]
    public bool activatesCamera = false;
    public CinemachineVirtualCamera camera;
    public bool cut;

    private void Start()
    {
        camerasGroup = GameObject.Find("Cameras").transform;
        brain = Camera.main.GetComponent<CinemachineBrain>();
    }

    public void SetCamera()
    {
        brain.m_DefaultBlend.m_Style = cut ? CinemachineBlendDefinition.Style.Cut : CinemachineBlendDefinition.Style.EaseOut;

        if (camerasGroup.childCount <= 0)
            return;
        /// Cameras Object의 자식 Object들을 일단 모두 비활성화 한다.
        /// 초기화
        for (int i = 0; i < camerasGroup.childCount; i++)
        {
            camerasGroup.GetChild(i).gameObject.SetActive(false);
        }
        /// VirtualCamera의 Object를 SetActive하는데 activatesCamera = false이기 때문에 비활성화
        camera.gameObject.SetActive(activatesCamera);
    }

    // Collider를 표시하기 위한 Circle
    private void OnDrawGizmos()
    {
        Gizmos.color = Color.red;
        Gizmos.DrawSphere(transform.position, .1f);
    }
}

```
{% endcode %}
{% endtab %}

{% tab title="SpeedModifier.cs" %}
{% code title="SpeedModifier " %}
```csharp
using UnityEngine;

public class SpeedModifier : MonoBehaviour
{
    public float modifier;
}
```
{% endcode %}
{% endtab %}
{% endtabs %}



