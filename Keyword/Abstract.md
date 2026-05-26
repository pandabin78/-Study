# 추상화
>추상화(Abstraction)는 복잡한 자료, 모듈, 시스템 등으로부터 핵심적인 개념이나 기능만을 간추려내고, 불필요한 세부 구현 사항은 감추는 것을 뜻합니다. 
사용자에게 "무엇(What)을 하는지"만 보여주고, "어떻게(How) 하는지"는 내부로 숨겨 복잡성을 줄이는 객체 지향 프로그래밍(OOP)의 핵심 개념입니다. 
### 💡 실생활 예시
- 자동차 운전
    >운전자는 핸들, 엑셀, 브레이크(인터페이스)만 알면 차를 몰 수 있습니다. 엔진 내부에서 연료가 어떻게 폭발하고 변속기가 어떻게 맞물리는지(세부 구현) 몰라도 운전하는 데 아무런 지장이 없습니다. 이것이 자동차 기계 장치의 추상화입니다. 
### 🎮 C#과 유니티로 보는 추상화 구현
>C#에서 추상화를 구현하는 대표적인 방법은 추상 클래스(Abstract Class)와 인터페이스(Interface)입니다. 
- 추상 클래스 (abstract) - 공통적인 특징을 묶을 때
    >완전하지 않은 '미완성 설계도'입니다. 자식 클래스들이 공통으로 가질 행동을 강제하면서도, 일부 공통 코드를 미리 작성해 물려줄 수 있습니다. 
```csharp
using UnityEngine;

// 추상 클래스: 그 자체로 객체(new)를 만들 수 없음
public abstract class Weapon : MonoBehaviour
{
    public string weaponName;
    public int damage;

    // 공통 기능: 모든 무기는 내구도가 닳는 방식이 같음
    public void DecreaseDurability()
    {
        Debug.Log($"{weaponName}의 내구도가 감소했습니다.");
    }

    // 추상 메서드: 구체적인 공격 방식은 자식이 직접 구현하도록 강제함 (추상화)
    public abstract void Use();
}

// 구체적인 구현 클래스
public class Sword : Weapon
{
    public override void Use()
    {
        Debug.Log($"{weaponName}으로 검을 휘둘러 {damage}의 피해를 줍니다.");
    }
}
```
- 인터페이스 (interface) - 순수한 기능 규격만 정의할 때
    >공통 분모가 없는 서로 다른 클래스들에 "특정 기능을 할 수 있다"는 계약(규격)만 부여합니다. 내부 구현은 100% 감추고 오직 메서드 이름만 노출합니다.
```csharp
// "데미지를 입을 수 있는 것"이라는 규칙을 추상화
public interface IDamageable
{
    void TakeDamage(int amount); // 무엇을 할지만 정의 (어떻게 할지는 숨김)
}

// 몬스터는 데미지를 입으면 피가 깎임
public class Enemy : MonoBehaviour, IDamageable
{
    private int hp = 100;
    public void TakeDamage(int amount)
    {
        hp -= amount;
        Debug.Log($"몬스터가 {amount}의 피해를 입음. 남은 피: {hp}");
    }
}

// 나무 상자는 데미지를 입으면 부서짐
public class WoodenBox : MonoBehaviour, IDamageable
{
    public void TakeDamage(int amount)
    {
        Debug.Log("나무 상자가 부서지며 아이템을 드랍합니다.");
    }
}
```
### 🚀 추상화를 사용하면 무엇이 좋을까요?
- 코드 복잡도 감소
    >상자든 몬스터든 공격하는 코드를 작성할 때, IDamageable이라는 추상화된 껍데기만 바라보고 .TakeDamage()를 호출하면 끝납니다. 내부 코드가 얼마나 복잡한지 신경 쓰지 않아도 됩니다.
- 유연한 변경 가능
    >무기 인터페이스를 바라보고 코드를 짜두면, 나중에 신규 무기(예: Gun, Laser)를 추가하더라도 기존 플레이어의 공격 시스템 코드를 단 한 줄도 고칠 필요가 없습니다.