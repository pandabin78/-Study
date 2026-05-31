# 실행 사이클
>실행 사이클(Execution Cycle)은 CPU가 인출한 명령어를 해독하고, 제어 신호를 생성하여 실제 연산을 수행하는 명령어 사이클의 핵심 단계입니다.

### 핵심 요약
- 정의: 
    >명령어의 Op-code(연산 코드)를 해독하여 CPU 내부 레지스터와 ALU(산술논리연산장치)를 통해 실제 작업을 수행하는 과정입니다.
- 위치: 
    >인출 사이클(또는 간접 사이클)이 끝난 후 실행됩니다.
- 특징: 
    >명령어의 종류(ADD, LOAD, STORE, JUMP 등)에 따라 수행되는 마이크로 연산이 완전히 다릅니다.

## 대표적인 명령어별 실행 사이클 동작 (마이크로 연산)


### 1. LOAD 명령어 (메모리의 데이터를 AC 레지스터로 가져오기)

$t_0 : \text{MAR} \leftarrow \text{IR(addr)}$ : 
>명령어의 주소를 MAR로 전송합니다.

$$t_1 : \mathtt{MAR} \leftarrow \mathtt{IR(addr)}$$ : 
>메모리에서 데이터를 읽어 MBR에 저장합니다.

$$t_1 : \mathtt{AC} \leftarrow \mathtt{MBR}$$ : 
>MBR의 데이터를 누산기(AC) 레지스터에 적재합니다.

### 2. ADD 명령어 (메모리의 데이터와 AC의 값을 더하기)
$t_0 : \text{MAR} \leftarrow \text{IR(addr)}$ : 
>더할 데이터가 있는 메모리 주소를 MAR로 전송합니다.

$$t_1 : \mathtt{MAR} \leftarrow \mathtt{IR(addr)}$$ : 
>메모리에서 데이터를 읽어 MBR에 저장합니다.

$$t_2 : \text{AC} \leftarrow \text{AC} + \text{MBR}$$ : 
>기존 AC의 값과 MBR의 값을 ALU에서 더해 다시 AC에 저장합니다.

### 3. STORE 명령어 (AC 레지스터의 데이터를 메모리에 저장하기)
$t_0 : \text{MAR} \leftarrow \text{IR(addr)}$ : 
>데이터를 저장할 메모리 주소를 MAR로 전송합니다.

$$t_1 : \mathtt{M[MAR]} \leftarrow \mathtt{AC}$$ : 
>AC 레지스터의 저장할 데이터를 MBR로 보냅니다.

$$t_2 : \text{M[MAR]} \leftarrow \text{MBR}$$ : 
>MBR의 데이터를 메모리에 씁니다.

### 4. JUMP 명령어 (특정 주소로 분기하여 다음 명령어 실행하기)
$$t_0 : \text{PC} \leftarrow \text{IR(addr)}$$ : 
>명령어의 주소 필드 값을 프로그램 카운터(PC)에 바로 넣어서 다음 인출 사이클 때 해당 주소의 명령어를 가져오도록 만듭니다.