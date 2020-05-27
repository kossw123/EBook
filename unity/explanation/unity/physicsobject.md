---
description: How-to-guide PhysicsObject에 이은 Explanation
---

# Explanation PhysicsObject

## 무엇을 하려고 하는가?

* How-to-guide PhysicsObject에서는 코드의 흐름을 단계적으로 나눴다면 이 페이지 에서는 단계적으로 나눈 Code Block들에 대한 설명과 전반지식에 대한 설명을 합니다.
* 설명에 대한 함수 설명과 사용법들은 아래의 기술문서 링크를 타고 보실수 있습니다.
* 아래의 글들은 영상을보고 정리한 것이니 일부의 오류가 있을 수도 있습니다. 이러한 부분에서는         오류를 수정 발견 및 수정을 원하신다면 이메일로 지적해 주신다면 확인 및 수정절차에                            들어가겠습니다.
* 보다 쉽게 설명한 예가 있어서 링크한 URL 들에 대해 상업적 목적이 없다는 뜻을 미리 알립니다.

{% page-ref page="../../technical-reference/unity/tr-physicsobject.md" %}

## Scripting Gravity

PhysicsObject.cs는 물체에 중력을 적용함과 동시에 움직임을 부여하고, 충돌관련 기능들을                                    가지고 있습니다. 

우선적으로 중력을 적용시키기 위해 Object를 움직여야 하는데 RigidBody의 Position\(위치\)를 가지고 움직임을 부여 합니다.

움직임을 부여하기 위해서 Rigidbody2D Component에 접근을 해야하는데 이를 위해서는 GetComponent를 사용하여 해결할 수 있습니다.

{% hint style="info" %}
rb2d = GetComponent&lt;Rigidbody2D&gt;\(\);

위와 같이 해당 Script가 있는 Object의 Rigidbody2D에 접근하여 Position이나 다른 Component들을 조절하기 위해서 위와 같은 문법을 씁니다.

  
허나 이를 쓰기 위해서는 Start\(\), Awake\(\)나 Enable\(\)등 Update\(\)가 시작하기 전에 미리 쓴다고 선언을 해야합니다.
{% endhint %}

그리고 Rigidbody2D에 움직임을 부여할 Vector2변수를 생성하여 속도를 부여합니다. Velocity는 중력을 적용하기 위한 변수이고, Object의 움직임을 실질적으로 주기 위해서는 deltaPosition이라는 변수를 선언하여 Velocity라는 중력을 적용한 변수를 더한 새로운 Position값을 만들어서 Rigidbody에 부여합니다.

이 자료에서는 Movement라는 Method\(함수\)를 통해 따로 작성하고 그 함수를 FixedUpdate\(\)에서 돌아가게 끔 만들어 움직이고 있습니다.



## Detecting Overlaps

위에서 중력을 적용하고 움직임을 부여 했지만 그 움직임은 기본적으로 중력을 적용하기 위한 움직임이지 PlayerStart가 움직이기 위한 움직임은 아닙니다. PlayerStart가 움직이기 위해서는  여러 작업이 필요합니다. 그 중에 이 단락에서는 충돌체가 겹치는지 안겹치는지 확인하기 위한 작업을 합니다.

핵심적으로는 Rigidbody2D.cast라는 함수가 존재합니다. 이 함수를 이용하여 충돌체를 반환하고 결과를     int형 배열에 저장 하는 기능을 가진 함수인데 이것을 가지고 충돌을 감지하는 이 단락에서 핵심 기능입니다.

{% hint style="info" %}
int count = rb2d.Cast\(move, contactFilter, hitBuffer, distance + shellRadius\);

위와 같은 parameter를 가지고 그 결과를 int형 배열로 반환하기 때문에 int형 변수에                   저장합니다.
{% endhint %}

 이 기능을 사용하기 위해서는 RayCastHit2D, ContactFilter2D Component가 필요한데 각각 아래의 기능을 가지고 있습니다.

* RayCastHit2D : 2D physics에서 RayCast\(광선을 따라 놓여 있는 물체를 감지하기 위해 사용\)에 의해           감지된 물체에 대한 정보를 반환합니다.
* ContactFilter2D : 접촉한 물체에 대한 정보를 반환합니다. ContactFilter를 사용한다면 접촉 결과를     나중에 필터링 할 필요없이 더 빠르고 편리하게 사용 가능합니다.

 대충 흐름을 보자면 충돌을 감지하기 위해 RayCastHit2D로 PlayerStart에서 시작되는 광선을 따라 놓여있는 물체를 감지하고 ContactFilter2D로 결과를 미리 필터링하여 닿는 물체를 확인한다는 의미 인듯합니다.

![gizmo.DrawWireSphere&#xB97C; &#xC774;&#xC6A9;&#xD55C; transform.position &#xBD80;&#xD130;  Rigidbody2D.cast&#xC758; Count](../../../.gitbook/assets/explanation-3.gif)

## Scripting Collision

 위에서는 충돌을 감지하는 기능이 대부분 이였다면 여기서는 직접 충돌에 관한 처리를 Scripting을 합니다. RayCastHit2D List를 가지고 광선에 따라 놓여진 물체들을 List형식으로 관리합니다.

 그 후 move.magnitude를 반환하는 distance 변수를 가지고 움직인 Vector2의 길이를 알아낼 수 있습니다. 이 변수는 곧 움직인 거리가 되며 최소 minMoveDistance변수와 비교하여 조금이라도 움직였을 시 hitBufferList, 즉 놓여진 물체를 조사하는 List에 넣습니다.

{% hint style="info" %}
List&lt;RayCastHit2D&gt; hitBufferList = new List&lt;RayCastHit2D&gt;\(\);

위의 List는 RayCastHit2D가 반환하는 결과물들을 List형식으로 정리하여 탐색하기 위한 List
{% endhint %}

그 후 List에 들어간 결과물을 가지고 반복문을 돌려서 normal Vector를 뽑아냅니다. 이 normal Vector는    캐릭터를 표면에 세우거나 발사체의 반사, 도탄을 정렬하는 방법으로 유용하게 사용할 수 있습니다.               즉, normal Vector\(법선벡\)는 표면에 수직이면 양수값을 반환하기 때문에 표면에 수직인지 아닌지를 판별 할 수 있습니다.

표면인지 아닌지를 판별했다면 groundNormal = currentNormal를 하여 ground에 있다고 체크 후 currentNoraml.x = 0으로 함으로써 초기화 하는 과정을 거칩니다.   //이부분 왜 x값을 0으로 하는가에 대한 확

float projection = Vector2.Dot함수를 통해 Velocity와 currentNoraml의 내적을 구합니다.                               양수를 가리킨다면 정확히 같은 방향을 가지고 있기 때문에 따로 처리를 하진 않지만 다른 방향을 가리킨다면 예외 처리를 해줘야 합니다. 그렇기 때문에 velocity에 projection\(음수\) \* currentNormal\(음수\)를 하여 velocity에 더하여 \(x + a, y + a\)와 같은 과정을 통해 땅 위에 고정시켜 놓습니다.

////// 이 부분 상세설명

원점에서 충돌지점 까지의 거리구하여 shellRadius보다 가까운 지점에 충돌시  기존의 distance\(이동 길이\)를 수정합니다. 

////// 이 부분 상세설명

projection 이라는 변수가 등장하는데 이 변수는 velocity와 currentNormal의 내적을 구하는 변수로써 어떤 충돌체와 충돌시에 그 충돌체가 앞인지 뒤인지 판별하는 변수입니다. 이 변수가 양수, 혹은 음수를 가지게 되는데 양수면 정면에 있고, 음수면 후방에 있는 것으로 판별 난다. 하지만 이 변수를 왜 쓰는가?

/////// 이부분 확인 필요 및 전체적으로 왜 쓰는가에 대한 의문이 필요할 

Vector의 내적을 구했다면 rb2d변수를 가지고 이동 시 move.normalized \* distance의 공식을 더해                  \(단위벡터값  \* 앞에 있는지 뒤에 있는지\)의 식을 통하여 움직이게 됩니다.

## Horizontal Movement

새로이 ComputeVelocity\(\)라는 함수와 기존에 쓰지 않았던 moveAlongGround라는 Vector2변수가               등장합니다. 각각 앞의 함수는 다른 Script에서 지금 작업중인 Script를 상속받아 쓰기 위함이고, 뒤의 변수는  땅에 수직인 Vector2를 구하기 위함입니다. 앞의 함수를 상속받는 것은 기능을 분할 시키기 위함이라고 쳐도 뒤에 moveAlongGround라는 변수가 왜 필요한지 보면 아래의 링크의 원문을 보고 이해했습니다.

{% embed url="https://tharindumathew.com/2014/03/01/finding-a-normal-perpendicular-vector-on-a-2d-plane/" caption="2D 평면에서 정상\(수직\) 벡터를 찾는 이유" %}

즉, 어떤 공간에서 두 지점은 같은 공간을 공유하지만 반대 방향을 가리키는 두 개의 평면이 실제로 지나가고 있기 때문에 두 지점에서 정상적으로 계산하려면 먼저 방향 벡터\(vector2.dot\)를 확보한 다음 양쪽으로 90도 회전해야 하기 때문입니다.

{% embed url="https://ddomyangggung.tistory.com/entry/%EA%B3%A0%EB%8F%84%EC%97%94%EC%A7%84-%ED%8A%9C%ED%86%A0%EB%A6%AC%EC%96%BC-17-%EB%B2%A1%ED%84%B0-%EC%88%98%ED%95%99Vector-math" caption="2D 평면에서의 정상\(수직\)벡터를 찾는 이유2" %}

위와 같은 이유로 moveAlongGround라는 변수는 2D 평면상에서 정상적으로 계산하기 위한 절차라고        이해했습니다.

이제 실질적으로 PlayerStart를 움직이기 위한 Script를 제작해야 하는데 따로 PlayerPlatformerController.cs를 생성하고 생성한 Script에서 Update\(\)문에서 Vector2.left를 통해 Update마다 왼쪽으로 가도록 설정합니다.

## Player Controller Script

PhysicsObject.cs에서는 충돌에 관한 기능들을 만들었다면 PlayerPlatformerController.cs에서는 실질적으로 움직이기 위한 Script로 기능을 나눕니다.

PlayerPlatformerController.cs에서는 기존 PhysicsObject.cs의 기능들을 상속받아서 씁니다. 그렇기 때문에 길게 어떠한 기능들에 대해서 작성하는 것도 아니며, 이미 Unity에 내장되어 있는 기능들을 활용합니다.

{% hint style="info" %}
move.x = Input.GetAxis\("Horizontal"\);

위의 GetAxis기능은 따로 기능에 관해서 설명하진 않겠습니다. 기존 Unity Document 및 Google에 검색만 하면 해당 글보다 좀 더 자세하게 얻을 수 있으며, 제가 정보를 얻은 글에 대한 링크는 아래에 남기겠습니다. 

그러니 여기서는 그냥 모니터를 보는 시선을 기준으로 하여 좌우를 눌렀을 때 -1 ~ 1의 값을 가지고 입력이 없을시에는  0의 값을 가지며 독립적인 프레임속도를 가지기 때문에 이 기능이 사용시 프레임의 속도 변화량에 따른 문제를 고민하지 않아도 된다는 것입니다.

[https://docs.unity3d.com/kr/530/ScriptReference/Input.GetAxis.html](https://docs.unity3d.com/kr/530/ScriptReference/Input.GetAxis.html)
{% endhint %}

그리고 Jump Button을 눌렀을 시에 velocity.y = jumpTakeOffSpeed  즉, PhysicsObject.cs에서 쓰인 velocity변수가 중력의 영향을 받아 0이하로 떨어지게 되고 이는 물체가 낙하한다는 것을 의미하니까 위의 문법은 velocity.y에 7의 힘을 주겠다는 의미이고 아래의 else if문에서 velocity.y &gt; 0일때 0.5씩 곱하여 매 프레임마다 중력기능과 같이 적용하여 점프하는 기능을 작성한다는 뜻으로 보입니다.

결국 최종적인 PlayerStart에 대한 속도는 targetVelocity = move \* maxSpeed의 문법으로  확정짓게 됩니다.

## Adding Player Animation

이 단락에서는 Animation, PlayerStart가 움직이는 것처럼 보이는 방법에 대하여 설명하고 있습니다.

Unity에서의 Animation은 Animator를 가지고 있으며 이는 Inspector에서 따로 Animator Component를 추가해야 합니다. 그러나 이 예제에서는 기본적으로 추가가 되어있기 때문에 좀더 깊게 들어가는 설명은 하지   않고 따로 기술문서에서 다루겠습니다.

그리고 SpriteRenderer는 기본적으로 Object가 추가 될 때 보여지는 형체이기 때문

첫 단락 Scripting Gravity에서 했던 것처럼 Component를 가져와야 합니다. GetComponent로 Animator와 SpriteRenderer를 가져와서 ComputeVelocity\(\) 함수에서 GetAxis를 통해 수평적인\(Horizontal\) 움직임을 보였을 때 반환되는 값을 가지고 0을 기준으로 하여 양수, 음수의 값을 가진다면 움직임\(오른쪽 혹은 왼쪽\)을 보인다는 것이니 그 값에 따라 Flip해줍니다. 

그리고 Animator에서는 특정 변수의 값에 따라 해당 Animation을 실행하는 parameter가 있습니다. 이번 예제에서는 bool, float 변수의 통제를 통해 미리  설정한 조건이 참일 시 실행하도록 코딩했습니다.

## 마치며

* 이상으로 해설 문서를 마치겠습니다.
* 각 해설에 따른 기술적인 문서는 아래의 링크에 기재했습니다.

{% page-ref page="../../technical-reference/unity/tr-physicsobject.md" %}

사실 CharacterController라는 기능을 써서 처리하는 것이 코딩하는데 있어서 더 편하거나 강의 영상의 코딩보다 효율적일 수 있습니다. 하지만 Collision Scripting을 통해서 많은 것을 얻을 수 있었고, 엔진의 이해가 좀더 쉽게 된다는 점이 장점으로써 크게 와닿았습니다.


