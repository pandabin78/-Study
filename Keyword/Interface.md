# 인터페이스
>인터페이스(Interface)는 클래스가 구현해야 하는 기능의 규격(프로토콜)만을 선언해 둔 '극단적인 형태의 설계도'입니다. \
C#에서 interface 키워드로 선언하며, 내부에는 실제 작동하는 코드(몸체)나 데이터를 저장하는 변수(필드)를 가질 수 없고 오직 메서드의 이름과 반환 타입, 매개변수만 정의합니다. \
이전에 다루었던 추상 클래스가 공통 데이터를 물려주는 ‘혈연관계’라면, 인터페이스는 서로 아무런 관계가 없는 클래스들에게도 "이 기능을 할 수 있다"는 능력(계약)을 부여하는 도구입니다. \
관례상 이름 앞에 대문자 'I'를 붙여 명명합니다.
### 💡 실생활 예시
- USB 규격 (인터페이스)
    >컴퓨터 본체에 있는 USB 포트는 내부적으로 마우스가 연결되는지, 키보드가 연결되는지, 미니 선풍기가 연결되는지 알 필요가 없습니다. \
    오직 'USB 규격(모양과 통신 방식)'만 맞으면 어떤 기기든 꽂아서 작동시킬 수 있습니다.
### 🎮 C#과 유니티(Unity) 코드로 보는 인터페이스
>유니티에서 "플레이어가 공격했을 때 데미지를 입힐 수 있는 모든 것"을 인터페이스로 추상화해 보겠습니다. \
몬스터뿐만 아니라 부서지는 나무 상자, 터지는 드럼통 등 혈통이 완전히 다른 객체들에게도 동일한 능력을 부여할 수 있습니다.
1. 인터페이스 선언 (규격 정의)
>인터페이스 내의 모든 선언은 기본적으로 외부에 공개되므로 접근 제어자(public)를 쓰지 않으며, 변수(필드)를 가질 수 없습니다.
```csharp
// "데미지를 입을 수 있는 기능"에 대한 규격 정의
public interface IDamageable
{
    // 무엇(What)을 할지만 적고, 어떻게(How) 할지는 적지 않음
    void TakeDamage(int amount); 
}
```
2. 서로 다른 클래스에서 인터페이스 구현
>C# 클래스명 뒤에 콜론(:)을 붙여 인터페이스를 구현합니다. \
추상 클래스와 달리 override 키워드 없이 일반 메서드처럼 구현하면 됩니다.
```csharp
using UnityEngine;

// 1. 몬스터 클래스 (적 캐릭터)
public class Enemy : MonoBehaviour, IDamageable
{
    public int hp = 100;

    public void TakeDamage(int amount)
    {
        hp -= amount;
        Debug.Log($"몬스터가 {amount}의 피해를 입음. 남은 체력: {hp}");
        if (hp <= 0) Debug.Log("몬스터 사망");
    }
}

// 2. 나무 상자 클래스 (사물/오브젝트)
// 몬스터와 상자는 혈연관계가 전혀 없지만 IDamageable 규격을 공유함
public class PropBox : MonoBehaviour, IDamageable
{
    public void TakeDamage(int amount)
    {
        Debug.Log($"나무 상자가 {amount}의 충격을 받아 부서지며 아이템을 떨어뜨립니다!");
        Destroy(gameObject);
    }
}
```
3. 플레이어의 공격 코드 (결합도 최소화)
>플레이어는 레이캐스트나 충돌 체크로 부딪힌 상대가 Enemy인지 PropBox인지 확인할 필요가 없습니다. \
오직 IDamageable 기능을 가지고 있는가?만 확인하면 되므로 결합도가 극도로 낮아집니다.
```csharp
using UnityEngine;

public class PlayerAttack : MonoBehaviour
{
    void Update()
    {
        if (Input.GetMouseButtonDown(0)) // 마우스 좌클릭 시 공격
        {
            // 예시: 정면에 있는 오브젝트를 감지했다고 가정
            GameObject hitObject = GetLookObject(); 

            if (hitObject != null)
            {
                // 상대방이 데미지를 입을 수 있는 규격(인터페이스)을 가졌는지 확인
                IDamageable damageable = hitObject.GetComponent<IDamageable>();

                if (damageable != null)
                {
                    // 적인지 상자인지 묻지 않고 공격 명령 전달 (다형성)
                    damageable.TakeDamage(25); 
                }
            }
        }
    }
}
```
### 🚀 인터페이스의 강력한 장점
- 다중 상속 가능
    >C# 클래스는 단 하나의 부모 클래스만 상속받을 수 있습니다. 하지만 인터페이스는 복수 구현(public class Player : MonoBehaviour, IDamageable, IInteractable, ISaveable)이 가능하므로 객체에 여러 능력을 유연하게 조립할 수 있습니다.
- 압도적인 유연성(유지보수성)
    >나중에 DestructibleWall(부서지는 벽)이나 PlayerCharacter(유저)가 추가되어도 플레이어의 공격 로직(PlayerAttack)은 단 한 줄도 고칠 필요가 없습니다.
