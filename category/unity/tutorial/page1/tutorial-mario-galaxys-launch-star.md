---
description: tutorial Mario Galaxy's Launch Star
---

# tutorial Mario Galaxy's Launch Star

## 왜 하려고 하는가?

* 3D Project를 통해 2D game에서의 Sprite의 한계를 넘어서 3D Modeling를 경험함과 동시에 아무래도 3D에서는 2D와는 또 다른 환경에서 제작을 하기 때문에 여러 장르의 개발을 연습과정을 문서화로 남기게 되었습니다.
* 아래의 강의 영상를 토대로 작성한 문서입니다.
* 해당 문서 및 문서가 사용되는 다른문서들은 오직 학습목적으로만 이용됩니다.

## 무엇을 하려고 하는가?

* Cinemachine의 Dolly Track with Cart를 사용하여 경로와 Camera Trigger Script를 작성하여 역동적인 카메라 액션 구현합니다.
* 3D Modeling을 이용하여 Character import 및 Mixamo를 이용한 Animation import를 합니다.
* DoTween API를 사용하여 shortcut way, generic way의 사용방법 등을 작성합니다.
* Material을 사용하여 Particle System의 모양과 효과를 구현합니다.

## 작성법

{% embed url="https://www.youtube.com/watch?v=T_3cne2tzYM&t=75s" %}
Mario Galaxy's Launch Star tutorial Video
{% endembed %}

해당 강의 영상을 참고하여 아래와 같은 요소를 중점적으로 정리 및 문서화를 하겠습니다.

* Path Setup(경로 설치)
* Animations(애니메이션)
* Polish(광택 : 제작에 질을 높이는 방법)

처음으로 해당 영상의 Character Modeling은 아래의 링크에서 받으실 수 있습니다.

{% embed url="https://github.com/mixandjam/Jammo-Character" %}
Jammo-Character Github Link
{% endembed %}

Mixamo를 이용한 3D Modeling의 import 과정은 tutorial이 아닌 다른 문서에서 다루도록 하겠습니다.

![Character 및 사전준비](<../../../../.gitbook/assets/image (108).png>)

위의 Github Link에서 다운 받은 Character를 Plane Object 위에 배치하고 몇가지 수정을 합니다.

* Plane Object에 Rigidbody Component를 추가 후 Use Gravity = false로 설
* Plane Object에서 Mesh Collider Component의 Convex = ture로 설정
* Plane Object에서 Mesh Renderer의 Materials의 Element 0 = m\_ground라는 Materials로 설
  * Character와 Plane사이의 충돌을 위한 Component들은 따로 추가하지 않습니다.
    * Character Controller Component가 충돌을 위한 Collider, Rigidbody Component를 대신해     사용할 수 있습니다.

위의 사전준비한 내용을 Play하여 충돌 및 입력을 확인 후 Path SetUp을 위한 Cinemachine Package를 설치합니다.

Window -> Package Manager -> Cinemachine을 설치니다.

![Window -> Package Manager -> Cinemachine Install](<../../../../.gitbook/assets/image (104).png>)

설치 후 Editor의 상단에는 Cinemachine이라는 탭이 하나 생깁니다. 클릭하여 Create Dolly Track with Cart항목을 눌러 새로운 Dolly Cart라는 Object를 생성합니다.&#x20;

Hierarchy에서 DollyTrack1, DollyCart1 Object가 생성되고 아래의 그림과 같은 Scene View가 생성됩니다.

![Create Dolly Track with Cart](<../../../../.gitbook/assets/image (34).png>)

그리고 Import한 Jammo\_Player를 Unpack을 하고 Cinemachine 탭에서 FreeLook Camera를 추가하여 생성된 Object의 Inspector에서 Follow, LookAt Component에 Character를 넣고 Orbits를 아래의 그림과 같이 수정합니다.

![](<../../../../.gitbook/assets/image (138).png>)

후에 Jamoo\_Player에 StarLauncher.cs Script를 넣고 아래의 Script들을 생성합니다.

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
    public Transform launchObject;
    [Space][Header("Public References")] public CinemachineFreeLook thirdPersonCamera;
    public CinemachineDollyCart dollyCart;
    float cameraRotation;
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
    IEnumerator CenterLaunch() {
        movement.enabled = false;
        transform.parent = null;
        DOTween.KillAll();
        // Checks to see if there is a Camera Trigger at the DollyTrack object - if there is activate its camera
        if (launchObject.GetComponent<CameraTrigger>() != null) 
            launchObject.GetComponent<CameraTrigger>().SetCamera();
        
        // Checks to see if there is a Camera Trigger at the DollyTrack object - if there is activate its camera
        if (launchObject.GetComponent<SpeedModifier>() != null) 
            speedModifier = launchObject.GetComponent<SpeedModifier>().modifier;
        
        // Checks to see if there is a Star Animation at the DollyTrack object
        if (launchObject.GetComponentInChildren<StarAnimation>() != null) 
            starAnimation = launchObject.GetComponentInChildren<StarAnimation>();
        
        dollyCart.m_Position = 0;
        dollyCart.m_Path = null;
        dollyCart.m_Path = launchObject.GetComponent<CinemachineSmoothPath>();
        dollyCart.enabled = true;
        yield return new WaitForEndOfFrame();
        yield return new WaitForEndOfFrame();
        Sequence CenterLaunch = DOTween.Sequence();
        CenterLaunch.Append(transform.DOMove(dollyCart.transform.position, .2 f));
        CenterLaunch.Join(transform.DORotate(dollyCart.transform.eulerAngles + new Vector3(90, 0, 0), .2 f));
        CenterLaunch.Join(starAnimation.Reset(.2 f));
        CenterLaunch.OnComplete(() => LaunchSequence());
    }
    Sequence LaunchSequence() {
        float distance;
        CinemachineSmoothPath path = launchObject.GetComponent<CinemachineSmoothPath>();
        float finalSpeed = path.PathLength / (speed * speedModifier);
        cameraRotation = transform.eulerAngles.y;
        playerParent.transform.position = launchObject.position;
        playerParent.transform.rotation = transform.rotation;
        flying = true;
        animator.SetBool("flying", true);
        Sequence s = DOTween.Sequence();
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
public class StarAnimation: MonoBehaviour {
    Animator animator;
    Transform big;
    Transform small;
    public AnimationCurve punch;
    [Space][Header("Particles")] public ParticleSystem glow;
    public ParticleSystem charge;
    public ParticleSystem explode;
    public ParticleSystem smoke;
    private void Start() {
        animator = GetComponent < Animator > ();
        big = transform.GetChild(0);
        small = transform.GetChild(1);
    }
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
// / <summary>
// /
// / </summary>
public class CameraTrigger: MonoBehaviour {
    CinemachineBrain brain;
    Transform camerasGroup;
    [Header("Camera Settings")] public bool activatesCamera = false;
    public CinemachineVirtualCamera camera;
    public bool cut;
    private void Start() {
        camerasGroup = GameObject.Find("Cameras").transform;
        brain = Camera.main.GetComponent<CinemachineBrain>();
    }
    public void SetCamera() {
        brain.m_DefaultBlend.m_Style = cut
            ? CinemachineBlendDefinition.Style.Cut
            : CinemachineBlendDefinition.Style.EaseOut;
        if (camerasGroup.childCount <= 0) 
            return;
        
        for (int i = 0; i < camerasGroup.childCount; i ++) {
            camerasGroup
                .GetChild(i)
                .gameObject
                .SetActive(false);
        }
        camera.gameObject.SetActive(activatesCamera);
    }
    private void OnDrawGizmos() {
        Gizmos.color = Color.red;
        Gizmos.DrawSphere(transform.position, .1 f);
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="SpeedModifier.cs" %}
{% code title="SpeedModifier.cs" %}
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

StarLauncher.cs에 미리 생성한 FreeLook Camera, Dolly Cart, parent Object를 Empty Component에 넣고 Path Curve를 임의대로 설정 합니다. 후에 실행한다면 Particle Attribute의 Follow, Smoke Particle이 비어있어서 오류가 날 수도 있는데, 이는 임의의 Particle을 넣어서 해결하실 수 있습니다.

![Insert StarLauncher.cs Empty Component](<../../../../.gitbook/assets/image (106).png>)

Play시 오류가 없는 것을 확인 하셨다면 본격적으로 Dolly Track을 가지고 Launcher Path를 제작해야 하는데 미리 생성된 Dolly Track1 Object에 Sphere Collider, Camera Trigger, Speed Modifier Script를 넣습니다.그리고 Virtual Camera를 하나 생성하고 "Cameras"라는 Empty Object를 생성하여 ChildObject로 넣습니다.

* Cinemachine Smooth Path : 임의의 값으로 설정합니다.
* Sphere Collider : isTrigger를 true로 활성화 합니다.
* Camera Trigger : Activates Camera를 true로 활성화 하고 미리 생성한 Virtual Camera를 Camera 항목에 넣습니다.

그리고 Launch Tag를 하나 생성하여 Dolly Track1 Object의 Tag를 Launch로 설정합니다. 그러면 Launcher Path Object는 아래의 그림과 같은 Component와 설정값을 가지게 됩니다.

![Dolly Track1 Inspector](<../../../../.gitbook/assets/image (65).png>)

이제 LauncherObject에 쓸 별모양의 Trigger Object를 작성하는데 아래의 그림과 같은 모양을 가지고 있습니다.

![LauncherStar Objecct](<../../../../.gitbook/assets/image (57).png>)

해당 Object를 작성하기 위해서는 견본 Object가 필요합니다. 아래의 File을 다운 받아 내부의 Package를 Import 하셔도 무방하고 만약 파일이 손상되었다면, Sample Project 내부의 Images, Models, Animation File들을 Import 하셔도 됩니다. 혹은 Sample Project의 Prefab을 Import 하시면 됩니다.

{% file src="../../../../.gitbook/assets/launcherstar_object.zip" %}

LauncherStar\_Object는 아래와 같은 Hierarchy를 가지고 있고 다음과 같이 작성합니다.&#x20;

![LauncherStar Hierarchy](<../../../../.gitbook/assets/image (92).png>)

그리고 각 Object에 설정할 Component들을 아래의 Tab에 기재했습니다.

{% tabs %}
{% tab title="LauncherStar Object" %}
![LauncherStar Object Component](<../../../../.gitbook/assets/image (59).png>)

* LauncherStar Object Component
  * Animator = LaunchStart로 설정
  * Cinemachine Dolly Cart Script 추가, Position Units = Normalized로 설정
  * StarAnimation Script 추가, 각 항목에 맞는 Empty Object 생성 후 추가

\*\*\* 그림의 Object들의 이름은 임의로 설정했습니다.
{% endtab %}

{% tab title="Plane Object" %}
![Plane Object Component](<../../../../.gitbook/assets/image (134).png>)

* Plane Object Component
  * Project View에서 Plane Prefab 추가
  * Mesh Renderer의 Material = LaunchStar로 설

![LaunchStar Material Component](<../../../../.gitbook/assets/image (7).png>)

* LaunchStar Material Component
  * Shader = Standard(Specular setup)
  * Albedo = R : 255 / G : 104 / B : 0 / A : 255
  * Specular = R : 135 / G : 79 / B : 39 / A : 255
  * Smoothness = 0.83
  * Emission = true / R : 191 / G : 62 / B : 23 / A : 255 / Intensity : 1
{% endtab %}

{% tab title="Inner_Plane Object" %}
![Inner Plane Object Component](<../../../../.gitbook/assets/image (66).png>)

* Inner\_Plane Object Component
  * Project View에서 Plane.001 Prefab 추가
  * Mesh Renderer의 Material = LaunchStar로 설정
{% endtab %}

{% tab title="Point Light Object" %}
![Point Light Object](<../../../../.gitbook/assets/image (32).png>)

* Color = R : 255 / G: 198 / B: 64 / A: 255
{% endtab %}

{% tab title="glow Object" %}
![glow Object Component](<../../../../.gitbook/assets/image (46).png>)

glow Object들은 Particle System만 있기 때문에 Particle System 설정값만 기재하겠습니다.

* Particle System&#x20;
  * glow
    * Duration = 1
    * Looping = false
    * StartLifetime = 1.5
    * StartSpeed = 0
    * StartColor = R : 255 / G : 230 / B : 112 / A : 166
    * Play On Awake = false
  * Emission
    * Rate over Time = 0
    * Bursts에 list 추가
  * Shape
    * Shape = Sphere
    * Radius = 0.0001
  * Color Over Lifetime
    * Color = R : 255 / G : 255 / B : 255 로 통일
    * Location : (0% / A : 255), (100% / A : 66)
  * Size over Lifetime
    * y = x Curve
  * Sub Emitters
    * Birth = Inner\_glow로 설정
      * Inner\_glow가 미리 존재하고 있거나, 하나 생성하여 Inner\_glow Object로 바꿉니다.
  * Renderer
    * Material = chargeParticle\_1
    * Trail Material = chargeParticle\_1
{% endtab %}

{% tab title="Inner_glow Object" %}
![Inner\_glow Object Component](<../../../../.gitbook/assets/image (128).png>)

glow Object와 Inner\_glow Object는 거의 같은 Particle 설정값을 가지고 있기 때문에 달라진 부분만 굵게 표시하여 작성하겠습니다.

* Particle System&#x20;
  * Inner\_glow
    * Duration = 1
    * Looping = false
    * StartLifetime = 1.5
    * StartSpeed = 0
    * **StartColor = R : 255 / G : 230 / B : 112 / A : 102**
    * Play On Awake = false
  * Emission
    * Rate over Time = 0
    * Bursts에 list 추가
  * Shape
    * Shape = Sphere
    * Radius = 0.0001
  * Color Over Lifetime
    * Color = R : 255 / G : 255 / B : 255 로 통일
    * Location : (0% / A : 255), (100% / A : 66)
  * Size over Lifetime
    * y = x  Curve
  * Renderer
    * **Material = chargeParticle\_2**
    * **Trail Material = chargeParticle\_2**
{% endtab %}

{% tab title="charge Object" %}
![charge Object Particle System](<../../../../.gitbook/assets/image (56).png>)

* Particle System&#x20;
  * charge
    * Duration = 5
    * Looping = false
    * StartLifetime = 1
    * StartSpeed = 0
    * Start Size(Random Between Two Constants로 전환) = 0.5 / 0.2&#x20;
    * StartColor = R : 187 / G : 0 / B : 87 / A : 166
    * Play On Awake = false
  * Emission
    * Rate over Time = 0
    * Bursts에 list 2개 추가 후&#x20;
      * 1번 = Time : 0.000 / Count : 30 / Cycles : 1 / Interval : 0.010 / Probability : 1.0
      * 2번 = Time : 0.400 / Count : 20 / Cycles : 1 / Interval : 0.010 / Probability : 1.0
  * Shape
    * Shape = Sphere
    * Radius = 2.34
    * Radius Thickness = 0.1
  * Velocity over Lifetime
    * Orbital = X : 0.27 / Y : 0.2 / Z : 0
    * Radial = -3
  * Color Over Lifetime
    * Color = R : 255 / G : 255 / B : 255 로 통일
      * Location : (0% / A : 0),   (25% / A : 255),   (50% / A : 255),  (75% / A : 255),         (100% / A : 66)
  * Size over Lifetime
    * y = -x Curve
  * Renderer
    * Material = starParticle\_1
    * Trail Material = starParticle\_1

![starParticle Material Component](<../../../../.gitbook/assets/image (129).png>)

* starParticle\_1 Material Component
  * Shader = Particle / Standard Units
  * Rendering Mode = Fade
  * Color Mode = Additive
  * Maps = star
  * Albedo = R : 191 / G : 129 / B : 79 / B : 255 / intensity : 1
{% endtab %}

{% tab title="explode Object" %}
![explode Object Particle System](<../../../../.gitbook/assets/image (47).png>)

* Particle System&#x20;
  * explode&#x20;
    * Duration = 5
    * Looping = false
    * StartLifetime = 1
    * StartSpeed(Random Between Two Constant) = 50 / 20
    * Start Size(Random Between Two Constants로 전환) = 0.5 / 0.2&#x20;
    * StartColor = R : 187 / G : 0 / B : 87 / A : 87
    * Play On Awake = false
  * Emission
    * Rate over Time = 0
    * Bursts에 list 1개 추가 후&#x20;
      * 1번 = Time : 0.000 / Count : 30 / Cycles : 1 / Interval : 0.010 / Probability : 1.0
  * Shape
    * Shape = Sphere
    * Radius = 0.0001
  * Velocity over Lifetime = true
  * Limit Velocity over Lifetime
    * Speed = 2
    * Dampen = 0.15
  * Color Over Lifetime
    * Color = R : 255 / G : 255 / B : 255 로 통일
      * Location : (0% / A : 0),   (25% / A : 255),   (50% / A : 255),  (75% / A : 255),         (100% / A : 66)
  * Size over Lifetime
    * y = -x Curve
  * Renderer
    * Material = starParticle\_1
    * Trail Material = starParticle\_2



![starParticle\_2 Material Component](<../../../../.gitbook/assets/image (83).png>)

* starParticle\_2 Material Component
  * Shader = Particles / Standard Unit
  * Rendering Mode = Fad
  * Color Mode = Additive
  * Maps = Radius
  * Albedo = R : 191 / G : 132 / B : 57 / B : 86 / intensity : 1
{% endtab %}

{% tab title="smoke Object" %}
![smoke Object Particle System Component](<../../../../.gitbook/assets/image (131).png>)

* Particle System&#x20;
  * smoke&#x20;
    * Duration = 0.20
    * Looping = false
    * StartLifetime = 0.8
    * StartSpeed = 10
    * Start Size(Random Between Two Constants로 전환) = 0.2 / 0.7&#x20;
    * StartColor = R : 255 / G : 255 / B : 255 / A : 200
    * Gravity Modifier = -0.19
    * Play On Awake = false
  * Emission
    * Rate over Time = 0
    * Bursts에 list 1개 추가 후&#x20;
      * 1번 = Time : 0.000 / Count : 30 / Cycles : 1 / Interval : 0.010 / Probability : 1.0
  * Shape
    * Shape = Circle
    * Radius = 0.02
  * Velocity over Lifetime = true
  * Limit Velocity over Lifetime
    * Speed = 1
    * Dampen = 0.3
  * Color Over Lifetime
    * Color = R : 255 / G : 255 / B : 255 로 통일
      * Location : (0% / A : 0),   (25% / A : 255),   (50% / A : 255),  (75% / A : 255),         (100% / A : 66)
  * Size over Lifetime
    * y = -x Curve
  * Noise
    * Strength = 0.3
  * Texture Sheet Animation&#x20;
    * Tiles = X : 2, Y : 1
    * Animation = Single Row
    * Time Mode = Speed
    * Speed Range = 0 / 1
    * Start Frame = 0 / 1.9998
  * Renderer = true
{% endtab %}
{% endtabs %}

설정값 대로 LauncherStar Object를 작성합니다.

![LauncherStar\_Path Hierarchy](<../../../../.gitbook/assets/image (99).png>)



Unpack Prefab을 한 LauncherStar\_Path의 하위 Object로 두 개의 그림과 같은 Object를 생성합니다.           각 Object는 Dolly Cart로 Camera 이동이 움직이는 순간 활성화 시킬 것인지에 대한 옵션설정 Script입니다. Script에 대한 설정값은 아래의 그림과 같이 설정합니다.

![Activate Object Component](<../../../../.gitbook/assets/image (102).png>)

위의 그림과 같이 설정했다면 최종적으로 아래의 그림과 같이 Path가 설정이 될것이고 아래의 그림과 같은 결과가 나옵니다.

![LauncherStar\_Path Test](../../../../.gitbook/assets/launcherstar\_path.gif)

## 마치며

* Dolly Cart Track과 Material에 대한 자료찾기와 DoTween에 Sequence, Join, Animation에 대한 조사가  좀 더 많았던 문서였던 것 같습니다.
* 위에서 작성하는 Object에서 좀 더 보완하고 추가한 그림이 아래와 같습니다.



* Sample Project에 나온 Scene에 대한 Post Processing이나, TimeLine을 이용한 Cameara 이동 등은   후에 작성하도록 하겠습니다.
* Script에 대한 정리는 How-to-guide문서에서 다루도록 하겠습니다.

{% content-ref url="../../how-to-guide/unity/how-to-guide-mario-galaxys-launch-star.md" %}
[how-to-guide-mario-galaxys-launch-star.md](../../how-to-guide/unity/how-to-guide-mario-galaxys-launch-star.md)
{% endcontent-ref %}



