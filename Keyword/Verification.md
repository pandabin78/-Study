# Verification
>베리피케이션(Verification, 확인)은 소프트웨어 개발 프로세스에서 "제품이 명세서, 설계서 및 개발 표준에 맞춰 올바른 방식으로 개발되고 있는지 검증하는 과정"을 뜻합니다.\
이전 답변에서 다루었던 Validation(검증)이 사용자의 최종 목적에 맞는지(What) 확인하는 과정이라면, Verification(확인)은 개발자가 중간 과정에서 설계대로 똑바로 만들고 있는지(How) 검사하는 개발자 중심의 활동입니다.
### 💡 핵심 키워드로 구별하는 차이점
- Verification (확인)
    >"Are we building the product right?" (제품을 올바른 방식으로 만들고 있는가?)
- Validation (검증) 
    >"Are we building the right product?" (사용자에게 올바른 제품을 만들었는가?)
### 🛠️ Verification의 주요 활동과 도구
>Verification은 코드를 직접 실행하지 않고 산출물을 검사하는 정적 분석과, 실제 코드를 빌드하고 실행하여 검사하는 동적 분석으로 나뉩니다.
- 정적 검사 (Static Verification)
    - 코드 리뷰 (Code Review)
        >동료 개발자들과 함께 소스 코드를 읽으며 논리적 오류, 코드 스멜, 코딩 컨벤션 위반을 잡아냅니다.
    - 인스펙션 및 워크스루 (Inspection & Walkthrough)
        >설계 문서나 요구사항 정의서 자체가 올바르게 작성되었는지 개발 초기에 검토합니다.
    - 정적 분석 도구
        >IDE(Visual Studio, Rider 등)에 탑재된 린터(Linter)나 SonarQube 같은 도구를 사용하여 잠재적 버그, 사용되지 않는 변수, 보안 취약점을 자동으로 스캔합니다.
- 동적 검사 (Dynamic Verification)
    - 단위 테스트 (Unit Test)
        >구현된 개별 메서드나 클래스가 설계된 스펙대로 정확한 입력값에 대해 정확한 출력값을 내는지 확인합니다.
    - 통합 테스트 (Integration Test)
        >분리된 모듈들을 합쳤을 때 컴포넌트 간 인터페이스 데이터 결합이 올바르게 작동하는지 확인합니다.
### 🎮 유니티(Unity)와 C# 환경에서의 Verification 예시
>유니티 프로젝트에서 개발 스펙과 설계 규칙을 준수하기 위해 수행하는 구체적인 Verification 방식입니다.
- C# 유닛 테스트를 통한 로직 확인 (C# Test Runner)
>플레이어의 경험치 계산 공식이 기획서(설계)대로 소스 코드에 올바르게 구현되었는지 테스트 코드를 빌드하여 검증합니다.
```csharp
using NUnit.Framework;

public class PlayerLevelTests
{
    [Test]
    public void Verify_LevelUp_On_MaxExp()
    {
        // 1. 조건 설정 (Arrange)
        PlayerStats player = new PlayerStats();
        player.currentExp = 90;

        // 2. 실행 (Act): 경험치 20을 추가 획득하는 로직 작동
        player.AddExperience(20);

        // 3. 설계 스펙과 일치하는지 확인 (Assert - Verification 실행)
        // 최대 경험치(100)를 넘었으므로 레벨이 2가 되었는지 확인
        Assert.AreEqual(2, player.currentLevel); 
    }
}
```
- 어설션(Assertion)을 사용한 개발 중 런타임 스펙 확인
>코드가 실행되는 동안 개발자가 상정한 내부 조건이나 제약 사항이 절대로 깨지지 않았는지 실시간으로 감시합니다.
```csharp
using UnityEngine;
using UnityEngine.Assertions; // 유니티 어설션 네임스페이스

public class GameManager : MonoBehaviour
{
    void Start()
    {
        // Verification: 게임이 시작될 때 UI 매니저 오브젝트가 반드시 할당되어 있어야 함을 보증
        // 만약 인스펙터에서 누락되었다면 에디터에 즉시 에러 크래시를 발생시킴
        Assert.IsNotNull(uiManager, "설계 오류: UIManager가 할당되지 않은 채 게임이 시작되었습니다.");
    }
}
```
### 🚀 Verification을 철저히 했을 때의 이점
- 결함 조기 발견
    >전체 소프트웨어 개발 비용 중 요구사항이나 설계 오류를 배포 단계에서 발견하면 개발 초기 단계보다 수십 배의 수정 비용이 듭니다. \
    Verification은 이를 초반에 차단하여 프로젝트의 납기 지연(소프트웨어 위기)을 막아줍니다.
- 코드 품질 유지
    >정적 분석과 코드 리뷰를 통해 시스템 전반의 결합도를 낮추고 가독성을 유지하여 코드 스멜이 쌓이는 것을 예방합니다.