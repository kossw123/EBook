---
description: How-to-guide Melee Attack
---

# How-to-guide Melee Attack

## 무엇을 하려고 하는가?

* Melee Attack에 관한 Script를 코드리뷰 합니다.
  * Script에 쓰인 Sensor\_Bandit.cs와 다른 Script를 코드리뷰 합니다.
* 이 문서를 상업적으로 이용하지 않습니다.

## Code Review

{% tabs %}
{% tab title="Bandit.cs" %}
{% code title="Bandit.cs" %}
```csharp
using UnityEngine;
using System.Collections;
public class Bandit: MonoBehaviour {

    [SerializeField] float m_speed = 1.0 f;
    [SerializeField] float m_jumpForce = 2.0 f;
    private Animator m_animator;
    private Rigidbody2D m_body2d;
    private Sensor_Bandit m_groundSensor;
    private bool m_grounded = false;
    private bool m_combatIdle = false;
    private bool m_isDead = false;
    
    [Header("Attack Property")] 
    public Transform attackPoint;
    public int attackDamage = 0;
    public float attackRadius = 0.5 f;
    public LayerMask enemyLayer;
    public float playerHP = 100 f;
    public float attackRate = 2 f;
    float nextAttackTime = 0 f;
    
    // Use this for initialization
    void Start() {
        m_animator = GetComponent < Animator > ();
        m_body2d = GetComponent < Rigidbody2D > ();
        m_groundSensor = transform.Find("GroundSensor").GetComponent<Sensor_Bandit>();
    }
    // Update is called once per frame
    void Update() {
        Movement();
    }
    void Movement() { 
        // -- Handle input and movement --
        float inputX = Input.GetAxis("Horizontal");
        // Swap direction of sprite depending on walk direction
        if (inputX > 0) 
            GetComponent < SpriteRenderer > ().flipX = true;
         else if (inputX < 0) 
            GetComponent < SpriteRenderer > ().flipX = false;
        
        // Move
        m_body2d.velocity = new Vector2(inputX * m_speed, m_body2d.velocity.y);
        // Set AirSpeed in animator
        m_animator.SetFloat("AirSpeed", m_body2d.velocity.y);
        HandleAnimation(inputX);
    }
    void HandleAnimation(float inputX) { // Check if character just landed on the ground
        if (!m_grounded && m_groundSensor.State()) {
            m_grounded = true;
            m_animator.SetBool("Grounded", m_grounded);
        }
        // Check if character just started falling
        if (m_grounded && !m_groundSensor.State()) {
            m_grounded = false;
            m_animator.SetBool("Grounded", m_grounded);
        }
        // -- Handle Animations --
        // Death
        if (Input.GetKeyDown("e")) {
            if (!m_isDead) 
                m_animator.SetTrigger("Death");
             else 
                m_animator.SetTrigger("Recover");
             m_isDead = !m_isDead;
        }
        // Hurt else if (Input.GetKeyDown("q")) 
            m_animator.SetTrigger("Hurt");
        
        // Attack
        if (Time.time >= nextAttackTime) {
            if (Input.GetMouseButtonDown(0)) {
                Attack();
                nextAttackTime = Time.time + 1 f / attackRate;
            }
        }
        // Change between idle and combat idle else if (Input.GetKeyDown("f")) 
            m_combatIdle = !m_combatIdle;
        
        // Jump else if (Input.GetKeyDown("space") && m_grounded) {
            m_animator.SetTrigger("Jump");
            m_grounded = false;
            m_animator.SetBool("Grounded", m_grounded);
            m_body2d.velocity = new Vector2(m_body2d.velocity.x, m_jumpForce);
            m_groundSensor.Disable(0.2 f);
        }
        // Run
        if (Mathf.Abs(inputX) > Mathf.Epsilon) 
            m_animator.SetInteger("AnimState", 2);
        
        // Combat Idle else if (m_combatIdle) 
            m_animator.SetInteger("AnimState", 1);
        
        // Idle else 
            m_animator.SetInteger("AnimState", 0);
        
    }
    void Attack() {
        m_animator.SetTrigger("Attack");
        Collider2D[] hitEnemies = Physics2D.OverlapCircleAll(attackPoint.position, attackRadius, enemyLayer);
        foreach(Collider2D enemy in hitEnemies) {
            Debug.Log("Hit :" + enemy.name);
            enemy.GetComponent<EnemyScript>().TakeDamage(attackDamage);
        }
    }
    // Draw Sphere Gizmo
    void OnDrawGizmoSelected() {
        if (attackPoint == null) 
            return;
        
        Gizmos.DrawWireSphere(attackPoint.position, attackRadius);
    }
}
```
{% endcode %}
{% endtab %}

{% tab title="Sensor_Bandit.cs" %}
{% code title="Sensor_Bandit.cs" %}
```csharp
using UnityEngine;
using System.Collections;
public class Sensor_Bandit: MonoBehaviour {
    /// <summary>
    /// 충돌시 m_ColCount 1 증가, 비충돌시 1 감소
    /// 매 프레임마다 Disable에서 설정한 시간만큼 매 프레임마다 deltaTime만큼 감소 시켜서 State를 결정합니다.
    /// </summary>
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
{% endcode %}
{% endtab %}

{% tab title="EnemyScript.cs" %}
{% code title="EnemyScript.cs" %}
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
public class EnemyScript: MonoBehaviour {
    public int maxHP = 100;
    int currentHP;
    public Animator animator;
    [Header("Boolean")] public bool isDead = false;
    // Start is called before the first frame update
    void Start() {
        animator = GetComponent < Animator > ();
        currentHP = maxHP;
    }
    public void TakeDamage(int damage) {
        currentHP -= damage;
        animator.SetTrigger("Hurt");
        if (currentHP <= 0) 
            Die();
        
    }
    void Die() {
        Debug.Log("Enemy Died");
        animator.SetBool("isDead", true);
        GetComponent < Collider2D > ().enabled = false;
        this.enabled = false;
    }
}
```
{% endcode %}
{% endtab %}
{% endtabs %}

## 마치며

* 손쉽게 Melee Attack에 대한 기능을 완성 시켰습니다.
* 코드에 대한 자세한 내용은 아래의 해설문서에서 작성하겠습니다.

{% content-ref url="../../explanation/unity/explanation-melee-attack.md" %}
[explanation-melee-attack.md](../../explanation/unity/explanation-melee-attack.md)
{% endcontent-ref %}



