# 다형성
>다형성(Polymorphism)은 하나의 객체나 메서드가 여러 가지 형태를 가질 수 있는 성질을 뜻합니다.\
객체 지향 프로그래밍(OOP)의 핵심 개념 중 하나로, 코드의 중복을 줄이고 유연성과 확장성을 높이는 데 필수적인 역할을 합니다.\
쉽게 말해, "동일한 명령을 내렸을 때 각 객체가 자신만의 방식으로 다르게 행동하는 것"을 의미합니다.
- 💡 실생활 예시
    - 운전사(코드)와 자동차(인터페이스)
    >운전사는 '가속 페달을 밟는다'는 동일한 행동을 합니다. 하지만 밟았을 때 전기차는 조용히 나아가고, 스포츠카는 굉음을 내며 나아갑니다.\
    자동차의 종류(구현체)가 바뀌어도 운전사가 페달을 밟는 방법(사용법)은 변하지 않습니다.
### 🛠️ 다형성을 구현하는 2가지 핵심 방법
- 오버라이딩 (Overriding - 부모 메서드 재정의)
    - 개념
        >상속 관계에서 부모 클래스의 메서드를 자식 클래스에서 자신에 맞게 다시 정의하는 것입니다.
    - 시점
        >프로그램이 실행되는 시점(Runtime)에 어떤 메서드를 호출할지 결정됩니다.
- 코드 예시
```csharp
using UnityEngine;

// 부모 클래스
public class Monster : MonoBehaviour
{
    // 자식이 재정의할 수 있도록 'virtual' 키워드 사용
    public virtual void Attack()
    {
        Debug.Log("몬스터가 기본 공격을 합니다.");
    }
}

// 자식 클래스 1: 좀비
public class Zombie : Monster
{
    // 부모의 메서드를 재정의하기 위해 'override' 키워드 사용
    public override void Attack()
    {
        Debug.Log("좀비가 근접 물리 공격을 합니다! (콰직)");
    }
}

// 자식 클래스 2: 마법사
public class Wizard : Monster
{
    public override void Attack()
    {
        Debug.Log("마법사가 원거리 파이어볼을 발사합니다! (콰아아)");
    }
}
```
- 사용 예시
```csharp
public class BattleManager : MonoBehaviour
{
    // 다양한 몬스터들을 하나의 리스트로 관리 (다형성)
    public List<Monster> monsterList = new List<Monster>();

    void Start()
    {
        // 부모 타입의 리스트에 자식 객체들을 담을 수 있습니다.
        monsterList.Add(new Zombie());
        monsterList.Add(new Wizard());

        // 일괄 공격 명령
        foreach (Monster monster in monsterList)
        {
            // 실제 객체(Zombie, Wizard)가 가진 각각의 Attack이 실행됩니다.
            monster.Attack(); 
        }
    }
}
```
- 오버로딩 (Overloading - 메서드 다중정의)
    > 개념: 한 클래스 내에서 이름은 같지만 매개변수의 타입이나 개수가 다른 메서드를 여러 개 만드는 것입니다.
    > 시점: 컴파일 시점(Compile time)에 어떤 메서드를 호출할지 결정됩니다.
- 코드 사용 예시
```csharp
using UnityEngine;

// 부모 클래스
public class Monster : MonoBehaviour
{
    // 자식이 재정의할 수 있도록 'virtual' 키워드 사용
    public virtual void Attack()
    {
        Debug.Log("몬스터가 기본 공격을 합니다.");
    }
}

// 자식 클래스 1: 좀비
public class Zombie : Monster
{
    // 부모의 메서드를 재정의하기 위해 'override' 키워드 사용
    public override void Attack()
    {
        Debug.Log("좀비가 근접 물리 공격을 합니다! (콰직)");
    }
}

// 자식 클래스 2: 마법사
public class Wizard : Monster
{
    public override void Attack()
    {
        Debug.Log("마법사가 원거리 파이어볼을 발사합니다! (콰아아)");
    }
}
```
- 사용 예시
```csharp
using System.Collections.Generic;
using UnityEngine;

public class BattleManager : MonoBehaviour
{
    // 다양한 몬스터들을 하나의 리스트로 관리 (다형성)
    public List<Monster> monsterList = new List<Monster>();

    void Start()
    {
        // 부모 타입의 리스트에 자식 객체들을 담을 수 있습니다.
        monsterList.Add(new Zombie());
        monsterList.Add(new Wizard());

        // 일괄 공격 명령
        foreach (Monster monster in monsterList)
        {
            // 실제 객체(Zombie, Wizard)가 가진 각각의 Attack이 실행됩니다.
            monster.Attack(); 
        }
    }
}
```
### 🚀 다형성이 주는 장점
- 유지보수성 향상
    >새로운 기능(예: 새로운 동물 클래스)이 추가되어도 기존의 호출 코드(예: myAnimal.makeSound())를 수정할 필요가 없습니다.
- 인터페이스 중심 설계
    >개발자는 구체적인 구현 내용을 몰라도 인터페이스나 부모 클래스의 규칙만 알면 프로그램을 작성할 수 있습니다.
