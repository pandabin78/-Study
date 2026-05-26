# 추상 클래스
>추상 클래스(Abstract Class)는 상속 관계에서 자식 클래스들의 공통적인 특징(변수, 메서드)을 한데 묶어 표준화하기 위해 만드는 '미완성 설계도'입니다.\
C#에서 abstract 키워드를 사용하여 선언하며, 그 자체로는 완벽한 객체가 아니기 때문에 new 연산자를 사용해 단독으로 인스턴스(객체)를 생성할 수 없습니다. \
반드시 다른 자식 클래스가 이를 상속받아 미완성된 부분을 채워야만 비로소 사용할 수 있습니다.
### 💡 실생활 예시
- 가전제품 설계도(추상 클래스)
    >모든 가전제품은 '전원 켜기/끄기' 기능과 '전기 플러그'가 필요합니다. 하지만 '가전제품'이라는 추상적인 물건을 상점에서 바로 살 수는 없습니다. 이를 바탕으로 실체화된 '냉장고', '세탁기'(자식 클래스)를 만들어야만 비로소 물건으로 사용할 수 있는 것과 같습니다.
### 🎮 C#과 유니티(Unity) 코드로 보는 추상 클래스
>유니티에서 다양한 적(Enemy) 캐릭터를 만들 때, 모든 적이 공통으로 가질 체력(HP)이나 움직임 규칙은 부모 추상 클래스에 적어두고, 공격(Attack) 방식처럼 적마다 완전히 다른 기능만 자식이 직접 구현하도록 만듭니다.
1. 부모 추상 클래스 정의
```csharp
using UnityEngine;

// abstract 키워드로 추상 클래스 선언 (MonoBehaviour 상속 가능)
public abstract class EnemyBase : MonoBehaviour
{
    // 1. 공통 데이터 멤버 (필드)
    public string enemyName;
    public int hp = 100;

    // 2. 일반 메서드: 모든 자식이 똑같이 공유할 완성된 기능
    public void TakeDamage(int damage)
    {
        hp -= damage;
        Debug.Log($"{enemyName}이 {damage}의 데미지를 입음. 남은 피: {hp}");
        if (hp <= 0) Die();
    }

    private void Die()
    {
        Debug.Log($"{enemyName}이 사망했습니다.");
        Destroy(gameObject);
    }

    // 3. 추상 메서드: 몸체({ })가 없는 미완성 메서드. 
    // 자식 클래스들이 자기 방식에 맞게 '반드시' 재정의(Override)해야 함.
    public abstract void Attack();
}
```
2. 자식 클래스에서 구현 (상속 및 오버라이딩)
>부모 추상 클래스를 상속받은 자식들은 override 키워드를 사용해 추상 메서드를 구체적으로 구현해야 합니다. 만약 구현하지 않으면 컴파일 에러가 발생합니다.
```csharp
using UnityEngine;

// 좀비 자식 클래스
public class Zombie : EnemyBase
{
    // 부모의 미완성 메서드를 구체적으로 구현
    public override void Attack()
    {
        Debug.Log($"{enemyName}가 플레이어를 물어뜯어 물리 피해를 줍니다!");
    }
}

// 마법사 자식 클래스
public class Mage : EnemyBase
{
    public override void Attack()
    {
        Debug.Log($"{enemyName}가 원거리에서 파이어볼 마법을 캐스팅합니다!");
    }
}
```
### 🛠️ 추상 클래스를 사용하는 이유 (장점)
- 코드 중복 제거
    >체력 관리, 데미지 처리, 사망 처리 등 모든 적에게 똑같이 들어가는 코드를 부모 클래스에 딱 한 번만 적어두면 되므로 코드가 간결해집니다.
- 설계의 강제성(가이드라인)
    >동료 개발자가 새로운 몬스터 클래스를 만들 때, 부모가 선언해 둔 Attack() 메서드를 빼먹지 않고 안전하게 구현하도록 규칙을 강제합니다.
- 다형성 구현
    >이전에 다루었던 다형성 규칙에 따라, 하나의 리스트에 좀비와 마법사를 EnemyBase 타입으로 묶어서 일괄 제어할 수 있습니다.
```csharp
// 부모 타입의 변수로 자식 객체들을 제어 가능
EnemyBase currentEnemy = GetComponent<EnemyBase>();
currentEnemy.Attack(); // 실제 정체에 맞는 공격 패턴이 실행됨
```
### ❓ 인터페이스(Interface)와는 무엇이 다른가요?
>추상 클래스를 공부할 때 가장 많이 헷갈리는 개념이 인터페이스입니다. 핵심적인 차이는 다음과 같습니다.
- 추상 클래스 (abstract)
    >"혈연관계"입니다. 변수(HP, 속도 등)를 가질 수 있고 완성된 메서드도 물려줄 수 있습니다. C# 특성상 단 하나의 클래스만 상속받을 수 있습니다.
- 인터페이스 (interface)
    >"형제/동맹관계"입니다. 변수를 가질 수 없으며 오직 빈 메서드(껍데기 규칙)만 나열합니다. 대신 클래스 혈통과 관계없이 여러 개를 동시에 구현(다중 상속)할 수 있습니다.