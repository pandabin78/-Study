# 리펙토링
>리팩토링(Refactoring)은 소프트웨어의 겉으로 보이는 기능(동작)은 그대로 유지한 채, 내부의 코드 구조를 깨끗하게 개선하는 작업을 뜻합니다.
버그를 고치거나 새로운 기능을 추가하는 것이 아니라, 이전 단계에서 다룬 코드 스멜(나쁜 징후)을 제거하여 코드의 가독성을 높이고 결합도를 낮추는 것이 핵심 목표입니다.
### 💡 실생활 예시
- 책상 정리
    >책상을 정리한다고 해서 방의 크기가 넓어지거나 새로운 가구가 생기는 것은 아닙니다. 하지만 물건들의 위치를 제자리로 찾고 정돈하면, 나중에 필요한 물건을 훨씬 빠르게 찾을 수 있고 공간을 효율적으로 쓸 수 있게 됩니다.
### 🎮 유니티(Unity) 코드로 보는 리팩토링 전후
>유니티 개발에서 흔히 발생하는 '긴 메서드(Long Method)'와 '중복 코드' 스멜을 리팩토링하는 예시입니다.
- ❌ 리팩토링 전 (복잡하고 읽기 힘든 코드)
    >Update 메서드 하나에서 입력 감지, 이동 처리, 애니메이션 제어까지 모두 처리하고 있어 가독성이 떨어집니다.
```csharp
public class PlayerMovement : MonoBehaviour
{
    public float speed = 5f;
    private Animator anim;

    void Start() { anim = GetComponent<Animator>(); }

    void Update()
    {
        // 1. 입력 감지 및 이동
        float h = Input.GetAxisRaw("Horizontal");
        float v = Input.GetAxisRaw("Vertical");
        Vector3 dir = new Vector3(h, 0, v).normalized;
        transform.translate(dir * speed * Time.deltaTime);

        // 2. 애니메이션 제어 (중복 및 복잡)
        if (dir != Vector3.zero) {
            anim.SetBool("isWalking", true);
        } else {
            anim.SetBool("isWalking", false);
        }
    }
}
```
- ⭕ 리팩토링 후 (메서드 추출 기법 적용)
    >각 기능을 별도의 의미 있는 메서드로 쪼개어(Extract Method) Update 내부의 가독성을 극대화했습니다.
```csharp
public class PlayerMovement : MonoBehaviour
{
    public float speed = 5f;
    private Animator anim;

    void Start() { anim = GetComponent<Animator>(); }

    void Update()
    {
        Vector3 moveDirection = GetMoveDirection();
        Move(moveDirection);
        UpdateAnimation(moveDirection);
    }

    // 역할을 명확히 분리하여 메서드로 추출
    private Vector3 GetMoveDirection()
    {
        float h = Input.GetAxisRaw("Horizontal");
        float v = Input.GetAxisRaw("Vertical");
        return new Vector3(h, 0, v).normalized;
    }

    private void Move(Vector3 direction)
    {
        transform.Translate(direction * speed * Time.deltaTime);
    }

    private void UpdateAnimation(Vector3 direction)
    {
        // 삼항 연산자로 코드도 간결화
        anim.SetBool("isWalking", direction != Vector3.zero);
    }
}
```
### 🛠️ 주요 리팩토링 기법 (마틴 파울러)
- 메서드 추출 (Extract Method)
    >위의 예시처럼 한 메서드 안에 너무 많은 로직이 들어있을 때, 이를 의미 있는 단위로 쪼개어 새로운 메서드로 만듭니다.
- 클래스 추출 (Extract Class)
    >하나의 클래스가 너무 많은 책임을 지고 있을 때(거대 클래스), 일부 책임을 새로운 클래스로 분리합니다. (예: Player에서 Inventory 로직을 분리)
- 매직 넘버를 상수로 변경 (Replace Magic Number with Constant)
    >코드에 if (state == 3) 처럼 의미를 알 수 없는 숫자를 쓰는 대신 const int STATE_DEAD = 3; 이나 enum으로 변경합니다.
- 조건문 분해 (Decompose Conditional)
    >복잡한 복합 조건식(if (a && b || !c))을 의미를 알 수 있는 명명된 메서드나 변수로 뺍니다.
### 🚀 리팩토링의 황금 규칙 (안전하게 하는 법)
- 기능 추가와 리팩토링을 동시에 하지 마세요
    >코드를 깨끗하게 만드는 중에는 절대로 새로운 기능을 넣거나 버그를 고치면 안 됩니다. 모자를 바꿔 쓰듯 명확히 분리해야 합니다.
- 테스트 코드가 지켜주어야 합니다
    >리팩토링 후에도 기존 기능이 똑같이 작동하는지 검증할 수 있는 단위 테스트(Unit Test) 시스템이 구축되어 있을 때 리팩토링의 효과가 극대화됩니다.