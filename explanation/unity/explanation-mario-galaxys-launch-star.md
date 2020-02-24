---
description: Explanation Mario Galaxy's Launch Star
---

# Explanation Mario Galaxy's Launch Star\(작업중\)

## 무엇을 하려고 하는가?

* Mario Galaxy's Launch Star Project를 정리하며 How-to-guide에서 기재하지 않은 Script의 설명과 그에 대한 부가 설명을 기재합니다.
* 각 문서는 상업적인 목적이 없습니다.

## Scripting Code Review

* Jammo\_Player Object Scripts

{% tabs %}
{% tab title="MovementInput.cs" %}
기본적인 움직임을 조절하는 Scripting이며, Jammo\_Player에 포함이 되어 있어서 딱히 수정할 필요는 없습니다. 하지만 해당 Script에 포함된 CharacterController를 이용한 Camera의 움직임 설정과, Shortcut에 대한 설명을 적습니다.

Start\(\)에서의 Initialize, Animation parameter Set  관련 설명은 넘기겠습니다.

```csharp
void Update() {
    InputMagnitude();
    isGrounded = controller.isGrounded;
    if (isGrounded) {
        verticalVel -= 0;
    } else {
        verticalVel -= 1;
    }
    moveVector = new Vector3(0, verticalVel * .2 f * Time.deltaTime, 0);
    controller.Move(moveVector);
}
```

InputMagnitutde\(\) 함수에서 Input.GetAxis로 움직이는 Vector를 설정합니다. 그리고 Update\(\) 함수에서는 verticalVel 변수를 가지고 y축\(높이\)를 설정합니다. 

```csharp
moveVector = new Vector3(0, verticalVel * .2f * Time.deltaTime, 0);
controller.Move(moveVector);
```

moveVector변수를 가지고 y축\(높이\)만 설정합니다. 그리고 아래의 Move\(\)함수에서 moveVector를 넣어서 움직입니다. 여기서 move함수는 CharacterController에서 제공하는 함수로써 복잡한 Vector연산은 Shortcut 해주는 함수입니다.

* CharacterController.Move\(Vector 변수\)
  * 임의의 속도 \* 방향을 parameter로 받아서 좀 더 많은 제어가 필요한 경우 사용합니다.
    * ex\) 비행기, 포탄, 부유.....

```csharp
void InputMagnitude() {
    InputX = Input.GetAxis("Horizontal");
    InputZ = Input.GetAxis("Vertical");
    Speed = new Vector2(InputX, InputZ).sqrMagnitude;
    if(Speed > allowPlayerRotation) {
        anim.SetFloat("Blend", Speed, StartAnimTime, Time.deltaTime);
        PlayerMoveAndRotation();
    }
    else if(Speed < allowPlayerRotation) {
        anim.SetFloat("Blend", Speed, StopAnimTime, Time.deltaTime);
    }
}
```

InputMagnitude\(\) 함수는 InputX, Y값을 가지고 새로운 Vector2변수로 설정하여 새로운 방향 Vector를 제시합니다. 여기서 Vector2.sqrMagnitude함수는 크기\(magnitude\)의 제곱을 계산하는 것이magnitude를 사용하는 것보다 훨씬 빠르기 때문에 사용합니다. **두 벡터의 크기를 비교하는 경우에, 크기를 제곱한 값을 사용해서 비교할 수 있습니다.**

그리고 Speed변수와 allowPlayerRotation와 비교하여 값에 따라 Animator의 parameter를 설정하고 그에 맞는 Animation을 재생합니다. 여기서 Blend라는 Trigger parameter가 있는데 이 parameter는 아래의 단락에서 설명하도록 하겠습니다.

```csharp
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
    if (blockRotationPlayer == false) {
        transform.rotation = Quaternion.Slerp(transform.rotation, Quaternion.LookRotation(desiredMoveDirection), desiredRotationSpeed);
        controller.Move(desiredMoveDirection * Time.deltaTime * Velocity);
    }
}
```

여기서도 InputX, Y값을 받아 새로운 Vector변수를 생성하는데 위에서는 y축\(높이\)관련해서 설정했다면 여기서는 x,z축\(양옆, 앞뒤\)을 설정합니다. 그렇기 때문에 var type의 변수를 가지고 Camera의 transform에 따라 움직이도록 합니다. 아래의 blockRotationPlayer bool 변수는 Player를 고정할 때 사용합니다.

* var변수
  * templete, generic 등등의 정해져 있지않는 type을 써야할 때가 있을 때 var type을 쓴다면 유동적으로 변경 가능합니다. 하지만 유동적이기 때문에 실행속도는 다른 변수타입에 비해 느립니다.

여기서 Camera는 움직이면 안되기 때문에 forward, right변수의 Normalize\(\)를 해야 법선벡터를 생성하여 방향벡터를 뽑아내서 새로운 desiredMoveDirection에 할당하여 실질적으로 움직이는 Vector가 됩니다.

\*\*\* LookAt\(\), RotateToCamera\(\)는 여기서 쓰이진 않지만 Sample Project에 기재 되어있기 때문에 아래의 Quaternion 단락에서 정리하도록 하겠습니다.
{% endtab %}

{% tab title="CharacterSkinController.cs" %}
해당 Script는 CharacterSkin의 Texture를 가져와 Animator parameter에 맞는 표정으로 변경하는 Script입니다.

주요 내용은 ChangeAnimatorIdle\(\), ChangeEyeOffset\(\) 함수이며 Update\(\) 함수에서 입력키에 따라 Animation을 초기화 합니다.
{% endtab %}
{% endtabs %}

* LaunchStar Object Scripts

{% tabs %}
{% tab title="StarLauncher.cs" %}
해당 Script는 Character Object에 추가하여 Dolly Cart가 움직일 때 Player의 행동을 변경하는 Script입니다.

주요 내용은 Dolly Cart transform에 Character transform을 옮기는 것, 그에 따른 DoTween을 사용한 Dolly Cart의 움직임, Space를 눌렀을 때의 StarLauncher Object의 움직임을 표현한 Coroutine, Object를 Tag로 검색해 LaunchObject에 담아 Dolly Cart로 옮기는 기능으로 나눠져 있습니다.

```csharp
void Update() 
{
    /// 1번
    if(insideLaunchStar) 
        if(Input.GetKeyDown(KeyCode.Space)) 
            StartCoroutine(CenterLaunch());
    /// 2번    
    if(flying) {
        animator.SetFloat("Path", dollyCart.m_Position);
        playerParent.transform.position = dollyCart.transform.position;
        if(!almostFinished) {
            playerParent.transform.rotation = dollyCart.transform.rotation;
        }
    }
   /// 3번
   if(dollyCart.m_Position > 0.7f && !almostFinished && flying) {
        almostFinished = true;
        playerParent.DORotate(new Vector3(360 + 180, 0, 0), 0.5f, RotateMode.LocalAxisAdd).SetEase(Ease.Linear) .OnComplete(() => playerParent.DORotate(new Vector3(-90, playerParent.eulerAngles.y, playerParent.eulerAngles.z), 0.2f));
    }
    // Debug
    if(Input.GetKeyDown(KeyCode.O)) Time.timeScale = .2f;
    if(Input.GetKeyDown(KeyCode.P)) Time.timeScale = 1f;
    if(Input.GetKeyDown(KeyCode.R)) UnityEngine.SceneManagement.SceneManager.LoadSceneAsync(UnityEngine.SceneManagement.SceneManager.GetActiveScene().name);
}
```

 위에서 번호표시한 함수가 순차대로 실행됩니다. 

1. Space를 눌렀을 때, Couroutine 실행합니다.
2. playerParent에 Dolly Cart transform을 옮깁니다.
3. Dolly Cart의 Position과 flying 변수가 true일 때, playerParent\(Character Object\)를 회전합니다.
4. 아래의 Debug는 게임 play시 TimeScale을 조정해 슬로우 모션 효과를 줍니다.

```csharp
IEnumerator CenterLaunch() {

    movement.enabled = false;
    transform.parent = null;
    DOTween.KillAll();
    if(launchObject.GetComponent<CameraTrigger>() != null) launchObject.GetComponent<CameraTrigger>().SetCamera();
    if(launchObject.GetComponent<SpeedModifier>() != null) speedModifier = launchObject.GetComponent<SpeedModifier>().modifier;
    if(launchObject.GetComponentInChildren<StarAnimation>() != null) starAnimation = launchObject.GetComponentInChildren<StarAnimation>();
    dollyCart.m_Position = 0;
    dollyCart.m_Path = null;
    dollyCart.m_Path = launchObject.GetComponent<CinemachineSmoothPath>();
    dollyCart.enabled = true;
    yield return new WaitForEndOfFrame();
    yield return new WaitForEndOfFrame();
    Sequence CenterLaunch = DOTween.Sequence();
    CenterLaunch.Append(transform.DOMove(dollyCart.transform.position, 0.2f));
    CenterLaunch.Join(transform.DORotate(dollyCart.transform.eulerAngles + new Vector3(90, 0, 0), 0.2f));
    CenterLaunch.Join(starAnimation.Reset(0.2f));
    CenterLaunch.OnComplete(() => LaunchSequence());
}
```

Update\(\)에서 Space를 눌렀을 때 실행되는 Coroutine입니다.

순차적로 코드리뷰를 해보자면,

* 3 ~ 4 : Movement Input Script를 비활성화, transform.parent을 null로 설정합니다.
  * 여기서 transform.parent는 Jammo\_Player의 Component로 추가되어 있을텐데 이 Object의 parent\(부모\)가 어디있을까에 대한 의문점이 있을 수 있습니다.
    * 직접 Debug.Log\(transform.parent\) 한다면 null을 확인 할수 있습니다. 그렇기 때문에 Coroutine 함수에서 또 다시 transform.parent = null을 하는 이유를 찾자면 위치정보의 초기화 때문입니다.
* 5 : DoTween.KillAll\(\)은 DoTween API에서의 Tween, Sequence들을 Destroy하는 함수입니다.
* 6 ~ 8 : 다른 Script들의 함수들을 GetComponent를 통해 가져 설정합니다.
* 9 ~ 12 : Dolly Cart의 Position 초기화 및 Path 설정
* 13 ~ 14 : WaitForEndOfFrame\(\)를 통해 한 프레임의 렌더링이 완전히 종료 됬을 때, 다음 진행으로 넘어가는데 실제로 확인해보니 완전히 종료되려면 2프레임이 필요하다는 것을 알수 있었습니다.
  * Garbage가 남아있는 것으로 추측되며, 이를 해결하기 위해 2개의 yield문을 사용함으로 완전히 초기화 시키는 것으로 추정됩니다.

{% embed url="https://ejonghyuck.github.io/blog/2016-12-12/unity-coroutine-optimization/" %}

* 15 ~ 19 : DoTween API를 사용하려는 Sequence를 생성하고 작업 끝에 Append로 추가하고 Join으로 동시에 처리되는 작업을 넣습니다.
  * 이에 대한 내용은 영상에서 자세히 설명이 되어있습니다.

```csharp
Sequence LaunchSequence() {

    float distance;
    CinemachineSmoothPath path = launchObject.GetComponent<CinemachineSmoothPath>();
    float finalSpeed = path.PathLength /(speed * speedModifier);
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
    s.Join(starAnimation.PunchStar(0.5f));
    s.Join(transform.DOLocalMove(new Vector3(0, 0, -0.5f), 0.5f)); // Return player's Y position
    s.Join(transform.DOLocalRotate(new Vector3(0, 360, 0), // Slow rotation for when player is flying(finalSpeed / 1.3f), RotateMode.LocalAxisAdd)).SetEase(Ease.InOutSine);
    s.AppendCallback(() => Land()); // Call Land Function
    return s;
}
```

순차적으로 코드리뷰를 해보자면

* 3 ~ 10 : 
  * 거리 
  * LauncherStar\_Path의 CinemachineSmoothPath Component를 가져와 설정
  * 최종 속도
  * 카메라 각도,
  * LaunchObject의 위치를 parent의 위치에 넣고
  * flying변수 활성화,
  * Animator parmeter Set을 합니다.
* 11 ~ 23 : DoTween을 사용하여 Dolly Cart가 Path를 따라 움직임을 표현합니다.

```csharp
void Land()    {
        playerParent.DOComplete();
        dollyCart.enabled = false;
        dollyCart.m_Position = 0;
        movement.enabled = true;
        transform.parent = null
        flying = false;
        almostFinished = false;
        animator.SetBool("flying", false);
        followParticles.Stop();
        trail.emitting = false;
    }
```

Character가 LauncherStar Object에서 떨어지고 땅에 착지할 시 실행되는 함수입니다.

순차적으로 코드리뷰를 해보자면

* 2 : DoTween 초기화
* 3 ~ 4 : Dolly Cart의 비활성화와 원래 위치로 초기화 합니다.
* 5 : MovementInput Script 활성화 합니다.
* 6 : transform parent = null로 만들어서 LaunchObject 변수 위치 초기화 합니다.
* 7 : flying 변수 비활성 합니다.
* 8 : almostFinished를 false 합니다.
* 9 : flying Animator를 false로 설정 합니다.
* 10 : particle을 멈춥니다.
* 11 : trail Renderer의 emitting\(발생\)을 false로 설정합니다.

```csharp
public void PathSpeed(float x)
    {
        dollyCart.m_Position = x;
    }
```

Dolly Cart의 속도를 조절합니다.

```csharp
public void PlaySmoke()
{
    CinemachineImpulseSource[] impulses = FindObjectsOfType<CinemachineImpulseSource>();
    for (int i = 0; i < impulses.Length; i++)
        impulses[i].GenerateImpulse();
    smokeParticle.Play();
}
```

Cinemachine Dolly Cart with Track이라는 Cinemachine을 생성하고 CinemachineImpulseSource Component를 통해 Camera Shake Effect를 부여합니다. tutorial 문서에서는 사용하지 않지만 Sample Project에서는 LaunchStar Object를 타고 Character가 착지할 때, 작동됩니다.

순차적으로 코드리뷰를 하자면,

* 3 : CinemachineImpluseSource 배열을 생성하여 다른 Camera의 CinemachineImpulseSource를 찾아서 넣습니다.
* 4 ~ 5 : Impulse 배열의 Index 0부터 impulses.Length까지 각 Index에서 GenerateImpulse를 생성합니다.
* 6 : Smoke particle을 재생합니다.

```csharp
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
```

 Collider 내장함수를 이용하여 OnTriggerEnter\(충돌 중\), OnTriggerExit\(충돌 종료시\)의 함수를 사용하여 미리 부여한 Tag를 찾아서 내용을 실행시킵니다.

순차적으로 코드리뷰를 해보자면,

* 2 : Launch Tag를 찾습니다.
* 3 : Collider 충돌 시 활성화 되는 InsideLaunchStar 변수를 true로 설정합니다.
* 4 : Collider의 transform을 LaunchObject에 넣어서 위치를 설정합니다.
* 6 : CameraTrigger Tag를 찾습니다.
* 7 : Collider의 CameraTrigger Component를 가져와 SetCamera함수를 실행합니다.
* 10 : Launch Tag를 찾습니다.
* 11 : insideLaunchStar 변수를 false로 설정합니다.
{% endtab %}

{% tab title="StarAnimation.cs" %}
 해당 Script는 LauncherStar Object를 통해 Space를 눌렀을 때 Character가 움직이면서 나오는 Effect를 설정합니다.

주요 내용은 DoTween을 사용한 Animation 구현입니다. 

```csharp
public Sequence Reset(float time) {
    animator.enabled = false;
    Sequence s = DOTween.Sequence();
    s.Append(big.DOLocalRotate(Vector3.zero, time).SetEase(Ease.InOutSine));
    s.Join(small.DOLocalRotate(Vector3.zero, time).SetEase(Ease.InOutSine));
    return s;
}
```

Sequence 타입의 Reset 함수입니다. 이 함수는 Tween으로 설정한 모든 Sequence들을 Ease합니다.

* Sequence 타입의 함수가 생소했는데, 

순차적으로 코드리뷰를 하자면,

* 2 : Animator를 비활성화 합니다.
* 3 : Sequence를 생성합니다.
* 4 : Sequence에 대한 설정값\(LocalRotate\)을 Ease 합니다.
  * DoTween에 대한 자세한 사항은 API, Component reference 항목에 기재하겠습니다.
* 5 : Sequence에 대한 설정값을 동시에 Ease 합니다.

```csharp
public Sequence PullStar(float pullTime) {
    glow.Play();
    charge.Play();
    Sequence s = DOTween.Sequence();
    s.Append(big.DOLocalRotate(new Vector3(0, 0, 360 * 2), pullTime, RotateMode.LocalAxisAdd).SetEase(Ease.OutQuart));
    s.Join(small.DOLocalRotate(new Vector3(0, 0, 360 * 2), pullTime, RotateMode.LocalAxisAdd).SetEase(Ease.OutQuart));
    s.Join(small.DOLocalMoveZ(-4.2f, pullTime));
    return s;
}
```

Sequence 타입의 PullStar 함수는 LaunchStar가 Start할 때의 Effect 및, 움직임을 나타냅니다.

순차적으로 코드리뷰를 하자면,

* 
{% endtab %}
{% endtabs %}

## Blend Tree Animation

![Jammo\_Player Animator&#xC758; Normal Status Blend Tree](../../.gitbook/assets/image%20%2835%29.png)

{% embed url="https://wergia.tistory.com/54?category=739103" caption="Blend Tree Animation 사용" %}



## Quaternion

{% embed url="https://docs.unity3d.com/kr/current/Manual/QuaternionAndEulerRotationsInUnity.html" %}











