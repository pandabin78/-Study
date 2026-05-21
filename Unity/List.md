# 유니티 코드 정리

## 리스트 란?
 - 크기(길이, 요소 수)를 변경할 수 있는 동적 배열

 - 배열과 다르게 크기를 미리 정하지 않아도 된다.
   요소를 추가하면 자동으로 크기가 늘어난다.

 - 배열과 동일하게 인덱스로 접근 가능하다.

 -  -> 게임 개발에서는 오브젝트 목록(적, 총알 등등), 인벤토리 등
      수량이 변경되는 데이터에 매우 자주 사용한다.


## 리스트 사용법			  

### 리스트 생성 방법
 - ListName라는 이름 의 함수 타입형 리스트 생성	
   > int형 numbers 새로운 리스트 생성
 
 - List<??> ListName = new List<??>		
   > Ex. List<int> numbers = new List<int>();

### 리스트 추가						
 - ListName 리스트에 값을 추가			
    > numbers 리스트에 요소 10 추가
 - ListName.Add(값)					
    > Ex. numbers.Add(10)

### 리스트 중간 삽입
 - ListName 리스트 n번 자리에 값 추가
    > numbers 리스트 요소 1번 자리에 15 추가
 - ListName.Insert(n번, 값)
    > Ex. numbers.Insert(1, 15)

### 리스트 값 삭제
 - ListName 리스트 요소 중 값을 찾아 제거
    >numbers 리스트 요소 중 20을 찾아 제거 {10, 30}
 - ListName.Remove(값)
    >Ex. numbers.Remove(20)

### 특정 인덱스 삭제
 - ListName 리스트 요소 중 값번 요소 제거
    >numbers 리스트 요소 중 0번 리스트 제거 {20, 30}
 - ListName.RemoveAt(값)
    >Ex. numbers.RemoveAt(0)

## 리스트 탐색
  ### Contains
    ListName 리스트에서 "변수"를 찾고
    
    있다면 contains에 ture 저장
    numbers 리스트에 {10, 20, 30} 이 있다.
    
    없다면 contains에 false 저장
    numbers 리스트에 10을 찾아 contains에 true 저장

    bool contains = ListName.Contains(변수)
    Ex. bool contains = numbers.Contains(10)

  ### IndexOf
    ListName 리스트에서 "변수"를 찾고	
    
    있다면 indexOf에 순번(번호, 인덱스) 저장

    없다면 indesOf에 -1 저장

    bool indexOf =  ListName.IndexOf(변수)
    Ex. bool indexOf = numbers.IndexOf(30)