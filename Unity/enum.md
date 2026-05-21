# 2. Enum(열거형)
 ## enum 이란?
  - 서로 관련 있는 상수 값들을 이름으로 묶어서 관리하는 자료형
    > 특정한 상황에 각 int값에 코드상의 이름을 붙여 놓는 것, 게임 개발에서 대상들을 분류할 때 많이 사용한다.
	
 ## enum 사용 방법
```		
public enum 함수이름
{

}
```
```rudy
public enum Job		// enum을 이용해 Job분류,
	{
		Warrior,
		Mage,
		Archer = 10 // 번호를 임의로 수정 가능
	}
```
- enum Job에 값이 {0, 1, 10}