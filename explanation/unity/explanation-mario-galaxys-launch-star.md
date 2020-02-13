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

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

* LaunchStar Object Scripts

## Blend Tree Animation

![Jammo\_Player Animator&#xC758; Normal Status Blend Tree](../../.gitbook/assets/image%20%2834%29.png)

{% embed url="https://wergia.tistory.com/54?category=739103" caption="Blend Tree Animation 사용" %}



## Quaternion

{% embed url="https://docs.unity3d.com/kr/current/Manual/QuaternionAndEulerRotationsInUnity.html" %}











