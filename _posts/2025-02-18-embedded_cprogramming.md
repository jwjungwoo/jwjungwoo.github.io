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
✅ 맞추자: 내 코드를 국제 표준에 맞추자   

# 프로그래밍 실행을 위한 메모리 구조
## 힙과 스택
✅ 일반적인 메모리 구조   
코드 영역, 데이터 영역, 스택 영역, 힙 영역   
<img src="https://github.com/user-attachments/assets/ba36dcdf-058a-4f4a-91a1-3d5ae4cd7187" width="500" height="300">   
<img src="https://github.com/user-attachments/assets/8dfbba01-f99c-4f97-b633-707d1a17b16f" width="300" height="330">   
   
1. 코드 영역: 코드가 저장되는 영역, 텍스트 영역이라고도 부름. CPU는 코드 영역에 저장된 명령어를 하나씩 가져가서 처리하게 됨.   
2. 데이터 영역: 전역 변수와 정적 변수가 저장되는 영역. 프로그램의 시작과 함께 할당. 프로그램이 종료되면 소멸. 프로그램이 load되면 바뀌지 않는 부분(assign되면 release되지 않는다.)   
3. 힙 영역: 프로그램 실행 중 입력값에 의해 바뀌는 메모리 영역. 사용자에 의해 메모리 공간이 동적으로 할당 및 해제. 메모리의 낮은 주소에서 높은 주소의 방향으로 할당. 상대적으로 느린 엑세스(왜냐하면 시작지점이 매번 달라지기 때문이다.)   
4. 스택 영역: 지역변수와 매개변수가 저장되는 영역. 함수의 호출과 함께 할당, 함수의 호출이 완료되면 소멸. push, pop 동작(LIFO). 메모리의 높은 주소에서 낮은 주소의 방향으로 할당. 매우 빠른 엑세스(왜냐하면 시작주소가 정해져 있기 때문이다.). 변수를 
명시적으로 할당해제 할 필요가 없다.   
   
✅ 메모리 구조 with code   
<img src="https://github.com/user-attachments/assets/052b91ed-588c-4ff0-945a-215e23a66fda" width="700" height="450">   
<img src="https://github.com/user-attachments/assets/0e29dae7-f9bc-470e-9f6b-19d5e1b350ca" width="700" height="430">   
RAM과 ROM의 차이는, RAM은 힙 영역과 스택 영역이 있다는 것이다.ROM은 코드 영역과 데이터 영역 밖에 없다.   
ROM에 적힌 코드를 RAM이 읽어온다. ROM: flash   
   
✅ 물리적인 메모리 구조   
<img src="https://github.com/user-attachments/assets/8ab8a7a6-dfa9-49fa-9839-b4ab3ce0cec1" width="500" height="270">   
<img src="https://github.com/user-attachments/assets/9298fb95-6fc7-4f10-a5a9-3282ac637eb2" width="500" height="270">   
ROM은 usb나 ssd 같은거다.   
   
✅ 메모리 리매핑   
<img src="https://github.com/user-attachments/assets/e8324847-8b34-4336-b3b6-7a1534521ce6" width="800" height="470">   

## 컴퓨터 구조
✅ 버스
버스엔 CPU, Memory, 보조기억장치, I/O가 연결돼있다. CPU엔 ALU, 제어장치, Register, Cache로 구성된다. ALU(Arithmatic and Logical Unit. 알루)는 연산장치다. Register는 ALU와 제어장치 
성능 향샹을 위해 사용되며, 범용레지스터와 용도지점레지스터로 구분된다. Cache는 메모리를 빨리 읽고 싶어서 local(CPU)로 가져온 것이다.   
   
버스는 3종류가 있다.   
1. 주소 버스 (단방향 버스)   
2. 데이터 버스 (양방향 버스)   
3. 제어 버스   

CPU, Memory 등이 버스에 연결된 것 처럼 버스들은 주기억장치와 연결돼있다. 주기억장치는 32bit 컴퓨터면 32bit이고, 64bit 컴퓨터면 64bit이다.   
   
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
ROM입장에선 코드 사이즈를 줄여줘야 부담이 적어지고. RAM은 스택 사이즈를 줄여주면 좋아한다.   
   
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
타입 변환은 가능한 피해야함.(시스템 사이클이 낭비됨)->비슷한 애들은 비슷한 애들끼리 놀게끔 사전에 조정하기   
   
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
   
✅ Initialization   
결론부터 말하면 뒤의 방식을 활용해야한다. 메모리가 엄청 줄어든다.   
```c
#include <stdio.h>

typedef struct {
	int element[3][3][3];
}Three3DType;

void main() {
	int a[3][3][3];
	int b[3][3][3];

	for (int i = 0; i < 3;i++) {
		for (int j = 0; j < 3;j++) {
			for (int k = 0; k < 3;k++) {
				a[i][j][k] = 1;
				b[i][j][k] = a[i][j][k];
			}
		}
	}


	for (int i = 0; i < 3;i++)
		for (int j = 0; j < 3;j++)
			for (int k = 0; k < 3;k++)
				a[i][j][k] = 0;

	//b = a;  // 에러 뜸. "식이 수정할 수 있는 Ivalue여야 합니다."

	for (int i = 0; i < 3;i++)
		for (int j = 0; j < 3;j++)
			for (int k = 0; k < 3;k++)
				printf("%d", b[i][j][k]);

	Three3DType aa, bb;

	for (int i = 0; i < 3;i++) {
		for (int j = 0; j < 3;j++) {
			for (int k = 0; k < 3;k++) {
				aa.element[i][j][k] = 1;
				bb.element[i][j][k] = aa.element[i][j][k];
			}
		}
	}

	//for (int i = 0; i < 3;i++)
	//	for (int j = 0; j < 3;j++)
	//		for (int k = 0; k < 3;k++)
	//			aa.element[i][j][k] = 0;
	memset(aa.element, 0, sizeof(aa.element));

	bb = aa; // 에러 안 뜨고 aa에 따라 bb도 값이 바뀜.

	for (int i = 0; i < 3;i++)
		for (int j = 0; j < 3;j++)
			for (int k = 0; k < 3;k++)
				printf("%d", bb.element[i][j][k]);
}
```

✅ return   
함수의 return 값은 레지스터에 저장됨.   
return이 필요 없으면 return은 하지마라.   
이 경우 함수의 추가 처리를 최소화하기 위해 함수를 "void"로 정의해야 함.   
   
✅ 비트 플래그 사용   
```c
const unsigned char option0 = 1 << 0; // 0000 0001
const unsigned char option1 = 1 << 1; // 0000 0010
const unsigned char option2 = 1 << 2; // 0000 0100
const unsigned char option3 = 1 << 3; // 0000 1000
const unsigned char option4 = 1 << 4;
const unsigned char option5 = 1 << 5;
const unsigned char option6 = 1 << 6;
const unsigned char option7 = 1 << 7;

예시
unsigned char myflags = 0;
myflags |= option4;
```   
   
✅ 1 dimensional table   
Char형 배열을 사용할 때, 일정한 길이를 가진 경우 2차원 테이블을 사용하기보다는 1차원 배열을 사용하는 것이 메모리 효율적이다.   
<img src="https://github.com/user-attachments/assets/2a688646-76a6-40e2-ae53-ce031585162d" width="500" height="300">   


✅ trick) case insensitive   
ASCII 코드의 특성을 이용해서 대소문자 구별없이 확인하는 것을 효율적으로 수행   
```c
#include <stdio.h>

void main(void) {
	int a = 10, b = 0;
	char chx = 'A';
	
	chx = chx | 0x20;
	if (chx == 'a') b = a + 5; // if (chx == 'a' || chx == 'A') b = a + 5; 대신

	printf("%d", b);
}
```
<img src="https://github.com/user-attachments/assets/a91782fb-422b-4f9d-aedb-d1039f4579f3" width="500" height="300">   

## Flow Control
✅ if vs switch   
가능한 하나의 변수만으로 판단하면 switch case문이 효율적이다.   
if는 나오는 값에 따라 시간이 달라지지만, switch는 lookup table 방식을 채택한다.   
<img src="https://github.com/user-attachments/assets/1944c83f-ca62-4514-8758-51f658c31ed2" width="700" height="300">   
if문의 조건문 중 앞부분에 대부분 걸린다면 if else를 쓰자.   
어셈블리어는 길이 차이가 거의 없다. 다만 실행시간은 차이 많음.   
   
✅ inline 코드 사용   
함수가 자주 호출되지만 코드가 몇 줄만 포함된 경우에 가장 효과적   
큰 함수를 인라인하면 실행 파일의 크기가 너무 커짐   
inline을 안 쓰면 함수 위치가 정해져서 불러올 때 call 해야함.   
근데 실습할 때 1년전과는 다르게 컴파일러가 업데이트 됐는지 inline을 해도 call을 한다. ㅋㅋㅋ   
```c
이렇게 선언하면 인라인이 된다.
inline __attribute__((always_inline)) int doubleabs(int x) {
    x = x << 2;
    return (x < 0) ? -x : x;
}
```   
   
✅ Loop Hoisting   
for문을 돌면서 계속 체크할 필요 없이 한번만 체크하고 for문 돌리게 하자. runtime시 실행시간에 영향을 준다.   
<img src="https://github.com/user-attachments/assets/58da5cba-7450-4048-a328-448fc407d54a" width="500" height="300">   

✅ Loop overhead   
```c
for (int i =0; i < 10; i++)보다
for (int i = 10; i > 0; i--)가 더 빠르다.
0이랑 비교하는 것이 더 빠르기 때문이다.   

DEC R1
CMP 10
BNE L1 과

DEC R1
BNZ L1 의 차이
```   
✅ if-else   
if 구문에서 else가 반드시 필요하지 않은 경우에는 생략   
```c
if(a==0) b = 0; else b = (a-c)*100;
위를 아래처럼 바꾸면 좋다.
b = 0; if(a) b = (a-c)*100;
```
   
✅ 함수 인자의 개수 제한   
시스템적으로 함수 인자는 4개 이하로 하는 것이 빠르다. 4개 이하로 하면 register를 사용하는데 나머지는 stack 영역에 할당되어, 느리게 load된다.   

## Others
✅ 기존 연산자를 잘 활용하자   
```c
int n, flag;
flag = (n > 10) ? 1 : 0;
```   
   
✅ int 나눗셈은 곱셈으로   
정수 나눗셈은 모든 정수 산술 연산 중 가장 느림. 따라서 식에 여러 나눗셈이 있는 경우 정수 나눗셈을 곱셈으로 바꾸자.   
   
✅ 대수학적인 간소화   
이미 컴파일러가 다 해줌. 따라서 gcc같은 성능좋은 컴파일러에선 신경 쓸 필욘 없을 듯하다.   
```c
z = x * a + x * b; 는 컴파일러가 알아서 z = x * (a + b)로 해줌

for (int i = 0; i < 10; i ++)
  printf("%d", i*10); 을 컴파일러는 이미 알아서
for (int i = 0; i < 100; i+=10)
  printf("%d,i);로 해주고 있다.
```   
   
✅ inline assembly   
```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"
#include "Bsp.h"
#include "IfxPort_pinMap.h"
IfxCpu_syncEvent g_cpuSyncEvent = 0;
typedef signed char int8_t;
typedef unsigned char uint8_t;
typedef signed short int16_t;
typedef unsigned short uint16_t;
typedef signed long int32_t;
typedef unsigned long uint32_t;
typedef float float32_t;
typedef double float64_t;
void core0_main(void)
{
    IfxCpu_enableInterrupts();
    
    /* !!WATCHDOG0 AND SAFETY WATCHDOG ARE DISABLED HERE!!
     * Enable the watchdogs and service them periodically if it is required
     */
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    /* Wait for CPU sync event */
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent, 1);

//    P10_IOCR0.U &= ~(0x1f << 19);
    __asm__("movh.a  a15,#0xF004\n\t"
               "ld.w  d15,[a15]-0x4FF0\n\t"
               "insert  d15,d15,#0x00,#0x13,#0x05\n\t"
               "st.w  [a15]-0x4FF0,d15\n\t"
               :
               :
               :"a15","d15"
                );

    P10_IOCR0.U |= 0x10 << 19;
    while(1)
    {
//        P10_OMR.U = 0x60006;
        P10_OUT.U = 0x4;
//        P10_OUT.
//        IfxPort_setPinHigh(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);
    }
}
```   

✅ 부동소수점 연산 제거   

✅ 조건문 최적화   
```c
if( a==b && c==d && e==f )   ->   if( ((a-b) | (c-d) | (e-f)) == 0)
if( x>=0 && x<8 && y >=0 && y <8 )  ->  if( ((unsigned)(x|y)) < 8 )
if( (x==1) || (x==2) || (x==4) || (x==8) )  ->  if( x&(x-1)==0 && x!=0)
```   
   
# 신뢰성 있는 코드를 위한 가이드라인
## MISRA C
✅ Motor Industry Software Reliability Association: 자동차 산업 소프트웨어 신뢰성 협회   
규칙의 종류   
1. Mandatory Rule: 예외가 허용되지 않는 명확한 규칙   
2. Required  Rule: 정당한 사유가 있을경우는 예외 허용   
3. Advisory Rule: 권장사항, 합리적으로 준수되어야함   

✅ BARR-C: 2018   
MISRA-C 최신판인 MISRA-C:2012보다 넓은(널널한) 범위의 코딩 표준을 정의함.   
```c
1. C면 C답게 쓰자. 이상하게 { }를 #define begin {, #define end } 이런식으로 하지마라
2. 줄은 최대 80자
3. goto 키워드를 사용하지 않는 것이 좋음
4. goto를 사용하는 경우 같은 블록 또는 둘러싸는 블록에서 나중에 선언된 레이블로만 이동해야 함
5. continue 키워드를 사용하지 않는 것이 좋음
6. 아래와 같이 하자
if
{
코드
}
7. 논리연산자(&&, ||)의 각 피연산자에 괄호 쓰자(귀찮아서 안 했던 그거)
8. 약어는 일관 되게 이해되지 않는 한 일반적으로 피해야 함
9. const: 되도록 사용, 숫자 상수에 대해 #define 대신 사용
10. static: 선언된 모듈외부에 표시라

#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"
#include "Bsp.h"
#include "IfxPort_pinMap.h"
IfxCpu_syncEvent g_cpuSyncEvent = 0;
typedef signed char int8_t;
typedef unsigned char uint8_t;
typedef signed short int16_t;
typedef unsigned short uint16_t;
typedef signed long int32_t;
typedef unsigned long uint32_t;
typedef float float32_t;
typedef double float64_t;

#define ISOMR  1
void core0_main(void)
{
    IfxCpu_enableInterrupts();
    
    /* !!WATCHDOG0 AND SAFETY WATCHDOG ARE DISABLED HERE!!
     * Enable the watchdogs and service them periodically if it is required
     */
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    /* Wait for CPU sync event */
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent, 1);

    P10_IOCR0.U &= ~(0x1f << 19);
    P10_IOCR0.U |= 0x10 << 19;

    while(1)
    {

#if ISOMR
        P10_OUT.U ^= 0x4;
#else   //여기서부터

        P10_OMR.U = 0x40004;
#endif  //여기까지 바탕이 회색임. 즉 실행 되는지 안 되는지 알 수 있음
        waitTime(50000000);
    }
}

# ifdef ISOMR  // 정의만 돼있으면
# ifndef ISOMR // 정의가 안돼있으면
# if 0         // 안락사   ->  이걸 주석으로 사용하라는 것 같음
```

```c
12. 많이 사용되는 주석 키워드
// WARNING
이 코드를 변경하는 데 위험이 있음을 유지 관리자에게 경고함
예를 들어 내가 시간을 계산해서 1초에 한번씩 돌게 loop를 1000만번 걸어놨는데 즉, 경험적으로
최적화해놨는데 코드 수정할 때 조심해라. (보통 이 예시처럼 loop로 시간 세진 않음)

// NOTE
how와 why를 설명해줌

// TODO 코드의 특정 영역이 아직 작성 중임을 나타내며 아직 완료되지 않은 작업을 설명함. 이니셜 ㄱㄱ
BJW TODO: 
```

```c
13. 화이트 스페이스 (공백)
i) 삼항 연산자를 구성하는 ? 및 : 문자는 항상 앞뒤에 공백이 하나씩 있어야 함
ii) 괄호 안의 표현식은 왼쪽 및 오른쪽 괄호 문자와 인접하여 공백이 없어야 함
iii) 전처리기 지시문의 #은 항상 한 줄의 시작 부분에 위치해야 하지만 지시문 자체는
     #if 또는 #ifdef 시퀀스 내에 들여쓰기 할 수 있음
#ifdef USE_UNICODE_STRINGS
#  define BUFFER_BYTES  128
#else
#  define BUFFER_BYTES 64
#endif 
```

```c
14. Header Files
i) 각 소스 파일에는 항상 정확히 하나의 헤더 파일이 있어야 하며, 헤더 파일은 항상 동일한 경로와 이름을 가져야 함
ii) include, define 등을 순서대로 정의하기 위해 템플릿을 만들어라
iii) public 헤더 파일에선 private 헤더 파일을 include 말고, private 헤더파일에서 public 헤더파일을 include 하는 방향으로 해라
public 헤더파일: "Ifx_Types.h", "iostream" 일반적으로 존재하는 파일
private 헤더파일: 내가 만든 파일
캡슐화(+보안)적으로도 문제가 되고, 충돌이 날 수도 있다.
```

```c
15. structures, unions, enumerations 등 모든 data types의 이름은 소문자와 밑줄로만 구성되어야 하며 '_t'로 끝나야 함.
단, 밑줄로 시작하면 안됨.
15-1. structures, unions, enumerations은 typedef 를 통해 이름을 지정해야 함
15-2. 관련된 모듈의 이름을 적자.
typdef struct
{
  uint16_t count;
  uint16_t max_count;
} timer_reg_t; // timer라고 모듈의 이름을 적어둠.
```
   
16. fixed-width integers. 길이를 적어주자.   
<img src="https://github.com/user-attachments/assets/383211b4-5331-4648-8c4e-bbd1703be8ec" width="500" height="260">   

```c
17. signed and unsigned integer
비트 연산자는 signed integer 연산에 사용해서는 안 됨

18. float 변수간의 == 는 사용하지 말고(부동소수점 이슈) "abs(b-c) < 0.00001" 이런식으로 하자.

19. 함수 이름 선언시 함수의 이름만 보고도 무슨 기능인지 알 수 있도록 해줘라. 특히 동사를 사용하자
DHT11_READ()
DHT11_START()

20. return은 한 곳에서만 하자.. if else문에 각각 return 하는건 좋은 습관이 아녀...

21. 매크로 함수를 할 때 피연산자에 각각 괄호를 사용해주자
#define MAX(A, B)  ((A) > (B) ? (A) : (B))
왜냐면 MUL(a+b, c+d)를 하면 a+b*c+d가 되기 때문이다. 원하던 계산이 안 됨.
근데 매크로 함수는 되도록 사용하지 말라네.
```

```c
22. naming
i) 전역 변수는 'g'로 시작 (g_zero_offset)
ii) pointer 변수, pointer to pointer 변수는 'p', 'pp'로 시작 (pp_vector_table)
iii) boolean를 포함하는 모든 ㅂinterger 변수는 'b'로 시작
```

```c
23. 초기화
i) 모든 변수는 사용하기 전에 초기화해야함
ii) 지역변수는 함수 맨 위에 모두 정의하는 것보다 필요할 때마다 정의하는 것이 좋음

24. 변수 선언은 한 줄에 하나씩

25. magic number 사용금지! for(;i< 100;)말고 for(;i<NUM_COLS;)로 하라. "변수 선인 해놔"

26. Jumps
goto, abort(), exit(), setjmp() 및 longjmp()는 사용하면 안 됨

27. == 왼쪽에 상수 써야함
if( NULL == p_object)
```
