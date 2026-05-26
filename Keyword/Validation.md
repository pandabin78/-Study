# Validation
>밸리데이션(Validation, 검증)은 소프트웨어 개발 및 데이터 처리에서 "입력된 데이터나 완성된 시스템이 사용자의 요구사항과 목적에 부합하는지, 그리고 올바른 형식과 범위 내에 있는지 확인하는 과정"을 뜻합니다.\
유니티(Unity)와 C# 개발을 포함한 소프트웨어 공학 전반에서 매우 중요하게 다루어집니다.
### 💡 Verification(확인) vs Validation(검증)의 차이
>소프트웨어 테스트 분야에서 두 단어는 엄격히 구분됩니다.
- Verification (확인)
    >"소프트웨어를 올바른 방식으로 만들고 있는가?"를 검사합니다. (설계서대로 코딩이 되었는지, 버그는 없는지 개발자 관점에서 확인)
- Validation (검증)
    >"올바른 소프트웨어를 만들었는가?"를 검사합니다. (사용자가 원했던 진짜 목적과 요구사항에 맞는지 사용자 관점에서 확인)
### 🎮 유니티(Unity)와 C# 개발에서의 Validation 활용 사례
>유니티 개발에서는 크게 데이터 입력 검증(인스펙터)과 런타임 오류 방지 검증 두 가지로 자주 쓰입니다.
- 유니티 인스펙터 데이터 검증 (기획자/디자이너의 실수 방지)
    >유니티 에디터 인스펙터 창에서 수치를 입력할 때, 잘못된 범위의 값이 입력되지 않도록 어트리뷰트(Attribute)를 사용해 검증합니다.
```csharp
using UnityEngine;

public class PlayerStats : MonoBehaviour
{
    // [Range] 어트리뷰트를 사용해 기획자가 체력을 음수로 입력하는 것을 원천 차단 (Validation)
    [Range(1, 100)] 
    public int maxHp = 100;

    [Range(0.5f, 3.0f)]
    public float moveSpeed = 1.0f;

    // 유니티 에디터에서 값이 변경될 때마다 자동 검증하는 내장 메서드
    private void OnValidate()
    {
        if (maxHp < 1) maxHp = 1;
        Debug.Log("데이터 검증 완료");
    }
}
```
- C# 코드 내의 데이터 검증 (Runtime Validation)
    >외부에서 들어온 데이터나 매개변수가 안전한지 프로그래밍 적으로 검증하는 단계입니다. 이 단계를 거치지 않으면 NullReferenceException 같은 치명적인 크래시(Crash) 스멜이 발생합니다.
```csharp
using UnityEngine;

public class Inventory : MonoBehaviour
{
    // 아이템을 추가할 때 발생할 수 있는 예외 상황 검증
    public void AddItem(Item newItem, int count)
    {
        // 1. Null 검증 (데이터가 비어있는가?)
        if (newItem == null)
        {
            Debug.LogError("Validation 실패: 추가하려는 아이템이 Null입니다.");
            return;
        }

        // 2. 범위 검증 (올바른 숫자가 들어왔는가?)
        if (count <= 0)
        {
            Debug.LogWarning($"Validation 실패: 아이템 개수({count})는 0보다 커야 합니다.");
            return;
        }

        // 3. 비즈니스 로직 검증 (가방이 가득 찼는가?)
        if (IsInventoryFull())
        {
            Debug.Log("Validation 실패: 인벤토리 공간이 부족합니다.");
            return;
        }

        // 모든 검증(Validation) 통과 후 실제 로직 실행
        ExecuteAddItem(newItem, count);
    }
}
```
### 🚀 데이터 검증(Validation)을 철저히 했을 때의 이점
- 시스템 안정성 
    >잘못된 데이터가 유입되어 발생하는 프로그램의 오작동이나 런타임 크래시를 초반에 완벽히 차단합니다.
- 디버깅 시간 단축
    >에러가 발생했을 때 엉뚱한 곳을 헤매지 않고, 데이터 입력 시점에 바로 로그(LogError)를 찍어주므로 원인 파악이 빨라집니다.
- 보안 강화
    >웹/서버 통신 환경에서는 악의적인 유저가 조작된 패킷(예: 공격력 99999 등)을 보냈을 때 서버 사이드에서 Validation을 거쳐 변조된 치트를 방어할 수 있습니다.