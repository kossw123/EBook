---
description: Explanation Melee Attack
---

# Explanation Melee Attack

## 무엇을 하려고 하는가?

* Melee Attack Script에 코드에 대한 설명과, Script 설계에 대한 이유 등을 작성합니다.
* 이 문서는 상업적으로 이용하지 않습니다.

## Code Explanation

{% tabs %}
{% tab title="Bandit.cs" %}
해당 Script에는 움직임과, Animation, Attack 기능, Gizmo기능이 있습니다.

```csharp
void Movement() {
    float inputX = Input.GetAxis("Horizontal");
    if(inputX > 0) GetComponent<SpriteRenderer>().flipX = true;
    else if(inputX < 0) GetComponent<SpriteRenderer>().flipX = false;
    m_body2d.velocity = new Vector2(inputX * m_speed, m_body2d.velocity.y);
    m_animator.SetFloat("AirSpeed", m_body2d.velocity.y);
    HandleAnimation(inputX);
}
```

&#x20;Update()에서 움직이는 Movement 함수입니다. inputX의 기능과, Rigidbody2D를 할당합니다.

* 2 : GetAxis로 Horizontal값을 구해서 inputX변수에 할당합니다.
* 3 \~ 4 : inputX값에 따라 Sprite를 Flip합니다.
* 5 : 실질적으로 Bandit의 Rigidbody2D를 움직여서 움직이는 위치를 구합니다.
* 6 : Animator parameter를 Rigidbody2D값에 따라 할당합니다.
  * 여기서는 AirSpeed를 Rigidbody2D.y값에 따라 할당하는데, Jump기능을 구현할 때 사용합니다.
* 7 : Animation을 실질적으로 실행하는 함수입니다.

```csharp
void HandleAnimation(float inputX) {
    if(!m_grounded && m_groundSensor.State()) {
        m_grounded = true;
        m_animator.SetBool("Grounded", m_grounded);
    }
    if(m_grounded && !m_groundSensor.State()) {
        m_grounded = false;
        m_animator.SetBool("Grounded", m_grounded);
    }
    if(Input.GetKeyDown("e")) {
        if(!m_isDead) m_animator.SetTrigger("Death");
        else m_animator.SetTrigger("Recover");
        m_isDead = !m_isDead;
    }
    else if(Input.GetKeyDown("q")) m_animator.SetTrigger("Hurt");
    if(Time.time > = nextAttackTime) {
        if(Input.GetMouseButtonDown(0)) {
            Attack();
            nextAttackTime = Time.time + 1f / attackRate;
        }
    }
    else if(Input.GetKeyDown("f")) m_combatIdle = !m_combatIdle;
    else if(Input.GetKeyDown("space") && m_grounded) {
        m_animator.SetTrigger("Jump");
        m_grounded = false;
        m_animator.SetBool("Grounded", m_grounded);
        m_body2d.velocity = new Vector2(m_body2d.velocity.x, m_jumpForce);
        m_groundSensor.Disable(0.2f);
    }
    if(Mathf.Abs(inputX) > Mathf.Epsilon) m_animator.SetInteger("AnimState", 2);
    else if(m_combatIdle) m_animator.SetInteger("AnimState", 1);
    else m_animator.SetInteger("AnimState", 0);
}
```

Movement 함수에서 실행되는 Animation을 움직이는 함수입니다.

* 2 \~ 4 : Sensor\_Bandit의 State() 함수 결과값을 받아와서 m\_grounded false일 때, Animation을 설정합니다.
  * m\_grounded가 false이고 State()의 결과값이 true일 때, Grounded parameter를 설정합니다.
* 6 \~ 8 : 2 \~ 4 라인과 반대로 동작합니다.
* 10 \~ 13 : e키를 누른다면 실행되는 Animation 입니다.
  * e키를 누른다면 isDead의 값에 따라 Animation을 실행시킵니다.
* 15 : q키를 누르면 Hurt Animation을 실행시킵니다.
* 16 \~ 19 : Time.time > nextAttackTime 결과에 따라 마우스 왼클릭을 하면 Attack 함수를 실행시키고 nextAttackTime에 일정한 값을 더합니다.
  * 여기서 1f / attackRate로 공격간 딜레이를 설정하여 Attack함수를 실행시킵니다.
* 22 : f키를 누르면 m\_combatIdle값을 전환합니다.
* 23 \~ 28 : Jump기능에 필요한 Animation과 Rigidbody2D 값을 구합니다.
  * 28 : Disable의 값에 따라 Jump기능에 Delay를 걸어줍니다.
* 30 : inputX값의 절대값과, 부동소수점의 아주 작은 값을 비교해서 AnimState parameter를 설정합니다.
  * 여기서 AnimState는 정수형이고, 0\~2의 값에 따라 각 행동을 결정합니다.
* 31 : m\_combatIdle값에 따라 Combat Animation을 실행합니다.
* 32 : 30, 31라인 이외의 값들은 0으로 설정하여 Idle parameter를 설정합니다.

```csharp
void Attack() {
    m_animator.SetTrigger("Attack");
    Collider2D[] hitEnemies = Physics2D.OverlapCircleAll(attackPoint.position, attackRadius, enemyLayer);
    foreach(Collider2D enemy in hitEnemies) {
        Debug.Log("Hit :" + enemy.name);
        enemy.GetComponent<EnemyScript>().TakeDamage(attackDamage);
    }
}
```

&#x20;Attack함수가 실행되면 Attack Animation과, 충돌체를 검색하여 EnemyScript에 있는 TakeDamage를 실행시킵니다.

* 2 : Attack parameter를 설정하여 Animation을 실행시킵니다.
* 3 : Collider2D를 찾습니다. Physics2D.OverlapCircleAll()로 일정 위치의 Radius만큼의 Circle을 구하여 그 안에 들어오는 모든 충돌체를 구합니다.
* 4 \~ 6 : hitEnemies를 enemy에 넣어서 Foreach를 돌립니다. 그리고 들어온 모든 충돌체에 EnemyScript의 TakeDamage를 실행시킵니다.

```csharp
void OnDrawGizmoSelected() {
    if (attackPoint == null) 
        return;
    
    Gizmos.DrawWireSphere(attackPoint.position, attackRadius);
}
```

Gizmo를 그리고 싶은 경우 OnDrawGizmos, OnDrawGizmoSelected를 사용합니다.

* 2 \~ 3 : attackPoint가 없을 경우에 대한 예외 처리입니다.
* 5 : DrawWireSphere() 를 통해 Sphere Gizmo를 그립니다.
{% endtab %}

{% tab title="Sensor_Bandit.cs" %}
Character Object 발 밑에 위치한 Object로써, 충돌을 감지합니다. 원래는 Update()에서 쓰일 Character가 지면 위에 있는지 확인하는 Logic을 Script를 생성하여 따로 할당하여 가독성을 높인   설계입니다.

```csharp
public class Sensor_Bandit: MonoBehaviour {
    // / <summary>
    // / 충돌시 m_ColCount 1 증가, 비충돌시 1 감소
    // / 매 프레임마다 Disable에서 설정한 시간만큼 매 프레임마다 deltaTime만큼 감소 시켜서 State를 결정합니다.
    // / </summary>
    private int m_ColCount = 0;
    private float m_DisableTimer;
    private void OnEnable() {
        m_ColCount = 0;
    }
    public bool State() {
        if (m_DisableTimer > 0) 
            return false;
        
        return m_ColCount > 0;
    }
    void OnTriggerEnter2D(Collider2D other) {
        m_ColCount ++;
    }
    void OnTriggerExit2D(Collider2D other) {
        m_ColCount --;
    }
    void Update() {
        m_DisableTimer -= Time.deltaTime;
    }
    public void Disable(float duration) {
        m_DisableTimer = duration;
    }
}
```

코드리뷰 보다는 흐름에 따라 설명하겠습니다.

1. Bandit.cs에서 State를 호출합니다.
2. State안에서 m\_DisableTimer의 값을 검사합니다.
3. Update()에서 m\_DisableTimer의 값을 deltaTime을 빼서 0이하로 만듭니다.
4. m\_ColCount의 값을 검사합니다.
5. 발 밑의 Box Collider가 충돌 시에는 m\_ColCount를 1 증가시킵니다.
   1. 충돌이 종료하면 1 감소 시킵니다.
6. m\_ColCount가 0 초과 일때 Bool type method를 반환시켜 최종적으로 땅에 있을 때 1의 값을 가집니다.


{% endtab %}

{% tab title="EnemyScript.cs" %}
Enemy로 설정한 Object에 대한 Script입니다. 해당 Script에는 가장 기초적인 Damage를 받으면 실행되는 함수와, HP가 0 이하일 때 실행되는 Die함수로 이루어져 있습니다.

```csharp
public void TakeDamage(int damage) {
    currentHP -= damage;
    animator.SetTrigger("Hurt");
    if (currentHP <= 0) Die();
}
```

&#x20;Bandit.cs에서 Collider를 검색하고 enemyLayer를 가진 Object를 검색하여 위의 함수를 실행시킵니다.

* 2 : currentHP에서 parameter로 받은 damage를 감소시킵니다.
* 3 : Hurt Animation을 실행합니다.
* 4 : currentHP가 0 이하일 때 Die()를 실행합니다.

```csharp
void Die() {
    Debug.Log("Enemy Died");
    animator.SetBool("isDead", true);
    GetComponent<Collider2D>().enabled = false;
    this.enabled = false;
}
```

TakeDamage 함수에서 currentHP가 0 이하일 때 실행되는 함수입니다.

* 3 : Animator의 isDead를 true로 할당합니다.
* 4 : Enemy가 currentHP가 0이 되었으니 죽었다고 하고 Collider2D Component를 비활성화 시킵니다.
* 5 : Enemy가 죽었으니 해당 Script도 비활성화 시켜서 불필요한 연산처리를 감소시킵니다.
{% endtab %}
{% endtabs %}

## 마치며

* 앞의 다른 문서들을 작성하며 느낀 난이도 보다, 수치조정에 대한 난이도가 체감상 낮았습니다.
* 이에 대한 기술문서는 아래의 링크에 기재하겠습니다.

{% content-ref url="../../technical-reference/unity/tr-melee-attack.md" %}
[tr-melee-attack.md](../../technical-reference/unity/tr-melee-attack.md)
{% endcontent-ref %}

