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
✅ 알뜰살뜰: 메모리 사용량을 알뜰살뜰 아끼자!   

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
RAM과 ROM의 차이는, RAM은 힙 영역과 스택 영역이 있다는 것이다.ROM은 코드 영역과 데이터 영역 밖에 없다.   
   
✅ 물리적인 메모리 구조   
<img src="https://github.com/user-attachments/assets/8ab8a7a6-dfa9-49fa-9839-b4ab3ce0cec1" width="500" height="270">   
<img src="https://github.com/user-attachments/assets/9298fb95-6fc7-4f10-a5a9-3282ac637eb2" width="500" height="270">   
ROM은 usb나 ssd 같은거다.   
   
✅ 메모리 리매핑   
<img src="https://github.com/user-attachments/assets/e8324847-8b34-4336-b3b6-7a1534521ce6" width="500" height="270">   

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
   
✅ 리더란?   
프로그램의 시작 문장, 조건부 분기 또는 무조건 분기의 목적지에 있는 문장, 조건부 분기 바로 다음에 위치하는 문장   

# 임베디드 C코드 최적화
## 숫자, define, const
```c
6
디파인된 SIX: 컴파일하는 과정에서 SIX가 0x6으로 완벽하게 대치됨. ROM에 안 올라오고 컴파일하는 도중에 없어짐.
컨스트로 정의한 sixvariable: 컴파일 길이가 길어짐. 변수의 위치를 만들어서 상수로 넣음.(참조하는데 시간 걸림)
```

##  ROM, RAM 관점에서 C코드 최적화
✅ ROM vs RAM   
ROM은 코드 사이즈를 줄인다. RAM은 스택 사이즈를 줄인다.   
   
✅ ROM 최적화   
프로그램 코드, 상수, 초기화된 전역 변수와 정적 변수
코드와 상수를 줄여라!
방법:
1. 데드 코드 제거(은근 컴파일러가 접근하지 못하는 코드들이 있음. 도와주자)   
2. 매크로나 인라인을 사용하지 않음(코드가 증가함. 웬만하면 함수를 사용하여 호출하자. 인라인은 함수를 호출하는 것이 아니라 코드에 대체 되는 것임.)   
3. 전역 변수는 초기화하지 않는다.   
4. 상수나 전역 변수 대신 지역 변수 사용   
5. 표현은 간결하게, 불필요한 중간과정은 생략   
6. 표준 라이브러리를 사용하지 않는다.(ex. iostream을 include를 하는 과정에서 앞에 함수들을 다 선언하기 때문에 코드가 길어짐.)   
   
✅ RAM 최적화   
임베디드 시스템을 만들 때 RAM과 ROM의 사이즈를 정하는데 이들이 작을수록 좋다.   
ROM의 복사본, 초기화되지 않은 전역변수와 정적변수, 지역변수, 함수의 인자 및 함수 호출시 발생하는 context(stack, heap 영역)   
Stack의 사용량을 줄여라!   
방법:
1. 함수 호출은 깊지 않게 한다.(stack overflow 예방, 함수 안에서 함수 호출의 반복)   
2. 함수는 매크로나 인라인으로 대체 (대신 코드는 길어짐)
3. 구조체나 배열 대신 포인터 활용 (스택 영역을 아낄 수 있음)   
4. 비트 플래그 활용 (0 또는 1만을 활용함으로써 자료형을 줄인다.)   
5. 값이 변하지 않는 전역변수의 상수화   
   
✅ 실행속도 관점에서의 C코드 최적화   
1. 인라인 함수 사용: 함수 호출이 자체의 구현 코드로 대체. 속도가 빨라진다. 다만 파일의 크기가 늘어난다.   
2. 참조 테이블 활용   
3. 인라인 어셈블리어 활용   
4. 전역 변수 사용: 지역 변수를 사용하면 stack에 지역변수가 push되고 함수 호출 끝나면 pop됨. 전역 변수를 사용하면 메모리를 잡아놓기 때문에 속도 측면에서 유리
5. 폴링방식 활용: 인터럽트 방식이 CPU 오버헤드가 많으므로 폴링이 효과적 (interrupt는 context switching이 길어서 CPU 오버헤드가 많음. 근데 버튼 event 같은 경우에는 interrupt 방식이 더 효율적...)   
6. 정수 연산: 대부분의 부동소수점 연산은 정수(고정소수점)연산으로 대체 가능함   
<img src="https://github.com/user-attachments/assets/c9478884-0abb-444f-8234-94f6de0c7b34" width="500" height="350">   
왼쪽은 code 사이즈는 크지만, 실행속도는 빠르다. 오른쪽은 반대다. 오른쪽은 메모리를 아끼는 것이다.   

## visual studio disassembly
✅ disassembly 코드 보는 법   
break point를 걸고 코드 창 오른쪽을 클릭하면 확인할 수 있다. 또한 디버그->창->메모리로 가서 메모리 주소도 검색할 수 있다.   
<img src="https://github.com/user-attachments/assets/7c987ab1-1532-4bb2-8214-3cb0620ad267" width="500" height="270">   
   
✅ 코드 길이 2배 -> 어셈블리 2배   
<img src="https://github.com/user-attachments/assets/8359dcd9-06a6-481c-8e1c-85c9dc81a733" width="300" height="70">   

# 최적화영역
## Data Handling
✅ Data Type 사용   
ex) 변수 범위가 0에서 200 사이인 경우 unsigned char 유형의 변수를 사용해야함.   
   
✅ Avoid Type Conversion   
타입 변환은 가능한 피해야함.(시스템 사이클이 낭비됨)   
   
✅ Signed & Unsigned 구분   
Unsigned의 사용: 몫과 나머지, loop counter, 배열 indexing   
Signed의 사용: 사칙연산시   
   
✅ Floats & Doubles   
float의 최대값 = 0x7f7f ffff   
double의 최대값 = 0x7f7f ffff ffff ffff   
불필요한 유형 변환이나 혼동을 피하기 위해, 숫자 값 뒤에 문자 'f'를 지정   
ex) x = y + 0.2f; 괜히 컴파일러가 double로 오해하고 메모리 사이즈 사용량을 늘릴 수 있으니까      
   
✅ Constant & volatile   
Constant:   
1. 데이터를 상수로 정의하여 ROM 공간에 할당   
2. 그렇지 않으면 RAM 공간도 이 데이터를 위해 예약됨   
3. 상수 데이터는 읽기 전용이어야 하므로 이 작업은 불필요   
   
Volatile:   
1. 컴파일러가 변수에 대한 최적화를 수행하지 못하도록 금지   
2. 일반적으로 인터럽트에 의해 변경되는 IO 레지스터와 변수에 사용됨   
3. 변수의 값은 synchronous하게 액세스할 수 있게 때문에 필요   
   
✅ Structure   
예전에 배웠던 것이다. 구조체 선언할 때 앞의 변수들 크기에 따라 알맞게 배치하는 것이 중요하다. 또한 빈 공간은 작은 배열을 넣어주는 등 알뜰살뜰하게 사용하면 된다.   

✅ Pass by Reference   
```c
total(long a, long b, long c, long d);를
struct sum {
long a;
long b;
long c;
long d;
} all;
total(&all);로 넘기면 한번만 참조하므로 굉장히 효율적이다. ram입장에서도 stack에 push, pop되는 것을 아낄 수 있음.
```   

## Flow Control

## Others
