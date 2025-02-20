---
layout: single
title:  "임베디드 C 프로그래밍 코드 최적화 기법"
categories: autoever
tag:
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "autoever"
#search: false
---

# 느낀점
✅ 깊다.

# 프로그래밍 실행을 위한 메모리 구조
## 힙과 스택
✅ 일반적인 메모리 구조   
코드 영역, 데이터 영역, 스택 영역, 힙 영역   
<img src="https://github.com/user-attachments/assets/ba36dcdf-058a-4f4a-91a1-3d5ae4cd7187" width="500" height="300">   
<img src="https://github.com/user-attachments/assets/8dfbba01-f99c-4f97-b633-707d1a17b16f" width="300" height="330">   
   
1. 코드 영역: 코드가 저장되는 영역, 텍스트 영역이라고도 부름. CPU는 코드 영역에 저장된 명령어를 하나씩 가져가서 처리하게 됨.   
2. 데이터 영역: 전역 변수와 정적 변수가 저장되는 영역. 프로그램의 시작과 함께 할당. 프로그램이 종료되면 소멸. 프로그램이 load되면 바뀌지 않는 부분(assign되면 release되지 않는다.)   
3. 힙 영역: 사용자가 직접 관리할 수 있는 메모리 영역. 사용자에 의해 메모리 공간이 동적으로 할당 및 해제. 메모리의 낮은 주소에서 높은 주소의 방향으로 할당. 상대적으로 느린 엑세스(왜냐하면 시작지점이 매번 달라지기 때문이다.)   
4. 스택 영역: 지역변수와 매개변수가 저장되는 영역. 함수의 호출과 함께 할당, 함수의 호출이 완료되면 소멸. push, pop 동작(LIFO). 메모리의 높은 주소에서 낮은 주소의 방향으로 할당. 매우 빠른 엑세스(왜냐하면 시작주소가 정해져 있기 때문이다.). 변수를
명시적으로 할당해제 할 필요가 없다.   
   
✅ 메모리 구조 with code   
<img src="https://github.com/user-attachments/assets/052b91ed-588c-4ff0-945a-215e23a66fda" width="700" height="450">   
<img src="https://github.com/user-attachments/assets/0e29dae7-f9bc-470e-9f6b-19d5e1b350ca" width="700" height="430">   
   
✅ 물리적인 메모리 구조   
<img src="https://github.com/user-attachments/assets/8ab8a7a6-dfa9-49fa-9839-b4ab3ce0cec1" width="500" height="270">   
   
✅ 메모리 리매핑   

## 컴퓨터 구조
✅ 버스: single
title:  "임베디드 C 프로그래밍 코드 최적화 기법"
categories: autoever
tag:
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "autoever"
#search: false
---

# 프로그래밍 실행을 위한 메모리 구조
## 힙과 스택
✅ 일반적인 메모리 구조   
코드 영역, 데이터 영역, 스택 영역, 힙 영역   
<img src="https://github.com/user-attachments/assets/ba36dcdf-058a-4f4a-91a1-3d5ae4cd7187" width="500" height="300">   
<img src="https://github.com/user-attachments/assets/8dfbba01-f99c-4f97-b633-707d1a17b16f" width="300" height="330">   
   
1. 코드 영역: 코드가 저장되는 영역, 텍스트 영역이라고도 부름. CPU는 코드 영역에 저장된 명령어를 하나씩 가져가서 처리하게 됨.   
2. 데이터 영역: 전역 변수와 정적 변수가 저장되는 영역. 프로그램의 시작과 함께 할당. 프로그램이 종료되면 소멸. 프로그램이 load되면 바뀌지 않는 부분(assign되면 release되지 않는다.)   
3. 힙 영역: 사용자가 직접 관리할 수 있는 메모리 영역. 사용자에 의해 메모리 공간이 동적으로 할당 및 해제. 메모리의 낮은 주소에서 높은 주소의 방향으로 할당. 상대적으로 느린 엑세스(왜냐하면 시작지점이 매번 달라지기 때문이다.)   
4. 스택 영역: 지역변수와 매개변수가 저장되는 영역. 함수의 호출과 함께 할당, 함수의 호출이 완료되면 소멸. push, pop 동작(LIFO). 메모리의 높은 주소에서 낮은 주소의 방향으로 할당. 매우 빠른 엑세스(왜냐하면 시작주소가 정해져 있기 때문이다.). 변수를
명시적으로 할당해제 할 필요가 없다.   
   
✅ 메모리 구조 with code   
<img src="https://github.com/user-attachments/assets/052b91ed-588c-4ff0-945a-215e23a66fda" width="700" height="450">   
<img src="https://github.com/user-attachments/assets/0e29dae7-f9bc-470e-9f6b-19d5e1b350ca" width="700" height="430">   
   
✅ 물리적인 메모리 구조   
<img src="https://github.com/user-attachments/assets/8ab8a7a6-dfa9-49fa-9839-b4ab3ce0cec1" width="500" height="270">   
RAM과 ROM의 차이는, RAM은 힙 영역과 스택 영역이 있다는 것이다.ROM은 코드 영역과 데이터 영역 밖에 없다.
✅ 메모리 리매핑   

## 컴퓨터 구조
✅ 버스
버스엔 CPU, Memory, 보조기억장치, I/O가 연결돼있다. CPU엔 ALU, 제어장치, Register, Cache로 구성된다. ALU(Arithmatic and Logical Unit. 알루)는 연산장치다. Register는 ALU와 제어장치 
성능 향샹을 위해 사용되며, 범용레지스터와 용도지점레지스터로 구분된다. Cache는 메모리를 빨리 읽고 싶어서 local(CPU)로 가져온 것이다.   
   
버스는 3종류가 있다.   
1. 주소 버스 (단방향 버스)   
2. 데이터 버스 (양방향 버스)   
3. 제어 버스   
버스들은 주기억장치와 연결돼있다. 주기억장치는 32bit 컴퓨터면 32bit이고, 64bit 컴퓨터면 64bit이다.   
   
✅ CISC vs RISC   
Complex Instruction Set Computing: 일반 컴퓨터(64bit). 편리, 강력한 다수의 명령   
Reduced Instruction Set Computing: 임베디드. 단순, 빠름, 효율적인 소수의 명령   

✅ 프로그램 실행주기   
인출 -> 해석 -> 실행 -> 인출 ...   
인출: 메모리에서 다음 명령을 가져오는 작업.   
이들을 빠르게 하기 위해 파이프라이닝을 사용한다.(연속적인 명령어시 사용 가능하다. 단, 분기문, if문, interrupt 시 사용 불가하다.)   

# TC275 부트시퀀스
## ARM에서의 build, linker script
✅ 링커 역할   
<img src="https://github.com/user-attachments/assets/58f310cd-e9b1-435b-8dde-c97aca99bfd0" width="500" height="480">   
<img src="https://github.com/user-attachments/assets/52770c2e-738b-44a9-807a-3a3fd8e9f895" width="500" height="440">   
링커: 불완전한 object 파일들을 합쳐 모든 코드와 데이터를 포함하는 새로운 object 파일을 생성해 내는 도구이다. 필요에 따라 라이브러리와 초기화(startup)파일을 같이 링크한다.
   
✅ 링커 스크립트   
TC275 Linker Script:   
1. core에 대한 내용을 입력한다. (내가 해줄건 크게 없다.)   
2. 메모리설정을 해준다.   
내가 사용할 메모리 주소와 사이즈와 속성을 입력한다.   
Core 0,1,2가 사용할 Data RAM, Program RAM을 정의한다.   
Program Flash, Data Flash, Trap, Interrupt 영역을 정의한다.   

# 코드 프로파일링
## Disassembly
✅ 코드 성능 분석을 위해 어셈블리어를 분석해볼 필요가 있다.   
원래는 TC275 프로파일링 툴이 있는데 우리 보드에서는 지원하지 않는다. 우린 유료버전까지 살 필요는 없었다. disassembly로 파악할 수 있는 것 1. 사용된 명령어 트리 2. 메모리 사용량 3. 연산복잡도. 이다.   
   
✅ 레지스터 vs API(=iLLD)   
API 사용보다 레지스터로의 직접 접근이 훨씬 빠르다!!!   
<img src="https://github.com/user-attachments/assets/55222fd7-800e-4952-a627-5e947da11ca5" width="500" height="200">   
실제로 다른 코드를 분석해도 둘 다 주소 8000006C에서 시작돼도 reg로 하면 800000C8에서 끝났는데 iLLD는 800000E6에서 끝난다. 즉 0x5C(=92)줄과 0x86(=134)줄 차이다. 
라이브러리 사용보다 직접 접근을 사용하자!! 우리 수업을 관통하는 말이다.
   
✅ Disassembly 분석   
<img src="https://github.com/user-attachments/assets/1c501428-8e9d-4cf6-8509-10419f5a3a7d" width="400" height="100">   
```c
1. movh.a -> 0xf004를 a15레지스터에 저장

2. ld.w -> a[15]에서 0x4ff0을 더하는게 아니라 빼는 이유는
더했을 때 맨 앞이 1로 바뀌는데 맨 앞 비트는 부호기때문에 접근을 못해서 빼는 것이다. 만약 자리수가 넘지 않는다면 더했을 것이다.
 이 연산은 고정된 메모리 주소에 위치한 GPIO 설정 레지스터에 접근하기위해 스택포인터(a15)를 기준으로 메모리 위치를 계산하는 연산이다.
계산하면 0x0f003b010 이 되는데 이는 IOCR0의 주소이다.
 ld도 종류가 다양한데, memory 창에서 ld.w가 적힌 주소를 입력하면 어떤 ld인지 알 수 있다. 19H라고 한다.

3. 0x0을 pos(#0x19 = 19)만큰 shift시키고, 5개 bit만큼 초기화

4. d15에 저장된 값을 [a15]-0x4ff0에 저장한다.
```   
<img src="https://github.com/user-attachments/assets/d123911d-cad1-4e31-a3a9-d0ba01213a74" width="500" height="70">   
   
```c
OMR을 했을 땐
D[15] = M(A[15] - 0x4ffc)를 한다.
      = M(0xf003b004) 인데 이는 P10_OMR의 주소다.
ctrl + h를 하고 0xf003b004를 하면
```
   
✅ 상수 폴딩   
```c
0x1 << 2 같은 경우는 컴파일러에서 바로 0x4로 넣어준다. "너도 알고 나도 알아." 이런건 코드에서 0x4로 한다해도 실행 속도엔 변화가 없으므로
최적화가 아니다. 이렇게 바로 넣어주는걸 상수 폴딩이라한다.
```   

# 컴파일러의 최적화
## 코드 최적화
✅ 코드 최적화   
주어진 코드에 대해 동등한 의미를 가지면서 실행 시간을 줄이거나 메모리를 줄이는 것.   

✅ 코드 최적화 분류   
여러 관점에서 분류할 수 있다.   
1. 영역   
지역 최적화, 전역 최적화, 프로시저 간 최적화   
2. 기능
실행시간 최적화, 메모리 최적화   
   
90%는 루프 최적화가 차지한다.

## 코드 최적화 종류
✅ 공부법   
막 이거저거 기법별로 분류할 필요는 없다. 이런 방법들이 있다는 것을 알고 혹시나 컴파일러가 안 해준다면 내가 하면된다.   
   
✅ 코드 최적화 종류   
1. 기계 독립적인 최적화   
핍홀 최적화, 지역 최적화, 전역 최적화, 루프 최적화, 프로시저 간 최적화   
2. 기계 종속적인 최적화   
   
✅ 핍홀 최적화   
1. 중복 명령어 제거 (이미 저장됐는데 쓸데없이 또 저장한다? 한번 로딩한 것을 두번 로딩하지 않겠다.)   
2. 도달 불가능한 코드 제거 (흐름그래프를 봤을 때 도달할 수 없는 코드가 있다. 지역이든 루프든 전역이든 어디서나 존재 가능)   
3. 제어 흐름 최적화   
4. 대수학적 간소화   
5. 세기 감축( 나누기 > 곱하기 > 덧셈, 뺄셈 순서로 연산시 클락 수가 많다.)   
6. 하드웨어 명령어 사용   
   
✅ 지역 최적화   
1. 공통 부분식 제거(예를들어 b+c가 여러줄에서 사용되면 t1 = b+c로 해두고 t1을 대신 사용하면 된다.)   
2. 상수 폴딩   
근데 상수 폴딩은 어디서나 다 쓰인다. 루프든, 전역이든. 또한 컴파일러가 알아서 함으로 우리는 신경 안 써도됨.   
   
✅ 루프 최적화   
1. 코드 이동   
```c
for (k = 1; k <= 1000; k++)
  c[k] = 2 * (p - q) * (n - k + 1) / (sqrt(n) + n)
에서 k 빼고 p, q, n은 다 고정이다. 따라서 p-q, n-k+1을 밖에서 미리 변수에 계산해놓고 가져와서 쓰면 된다.
```   
2. 귀납 변수 최적화 (루프를 돌 때마다 값이 일률적으로 변하면 세기 감축으로 최적화 할 수 있다.)   
```c

```   
3. 루프 융합 (둘의 조건문이 같으면 하나의 루프로 같이 묶어라.)   
4. 루프 교환   
5. 루프 전개   
```c
for (k = 1; k <= 100; k++) delete(x) 대신

for (k = 1; k <= 100; k+=5)
  delete(x);
  delete(x + 1);
  delete(x + 2);
  delete(x + 3);
  delete(x + 4);
```
   
✅ 전역 최적화   

✅ 기계 종속적 최적화   
1. 효율적인 명령어 선택
```c
x = x + 1의 경우
load r1, x
add r1, 1
store x, r1로 구성되는데,
일부 컴퓨터에선 inc1 만 해도된다.
```   
2. 레지스터 할당과 배정 (범용레지스터의 사용)   
서로 인접해있는 코드들은 사용하는 레지스터가 서로 겹치지 않게한다.   
3. 명령어 스케줄링(a*b - c*d 같은 경우 a*b보다 c*d를 먼저하면 더 빠르다.)   

## 리더
✅ 리더란?   
프로그램의 시작 문장, 조건부 분기 또는 무조건 분기의 목적지에 있는 문장, 조건부 분기 바로 다음에 위치하는 문
