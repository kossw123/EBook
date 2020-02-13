---
description: Explanation Mario Galaxy's Launch Star
---

# Explanation Mario Galaxy's Launch Star\(작업중\)

## 무엇을 하려고 하는가?

* Mario Galaxy's Launch Star Project를 정리하며 How-to-guide에서 기재하지 않은 Script의 설명과 그에 대한 부가 설명을 기재합니다.
* 각 문서는 상업적인 목적이 없습니다.

## Scripting Code Review

{% tabs %}
{% tab title="MovementInput.cs" %}
기본적인 움직임을 조절하는 Scripting이며, Jammo\_Player에 포함이 되어 있어서 딱히 수정할 필요는 없습니다. 하지만 해당 Script에 포함된 CharacterController를 이용한 Camera의 움직임 설정과, Shortcut에 대한 설명을 적습니다.



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

InputMagnitutde\(\) 함수에서 Input.GetAxis로 움직이는 Vector를 설정합니다. 그리고 Update\(\) 함수에서는 verticalVel 변수를 가지고 y축\(높이\)를 설정합니다. 굳이 이렇게 설정하는 이유는 Dolly Cart로 Player를 움직일 때 vertical 값을 CharacterController를 가지고 움직이기 힘들기 때문입니다.
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}



