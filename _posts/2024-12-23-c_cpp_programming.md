---
layout: single
title:  "C/C++ 프로그래밍"
categories: autoever
tag:
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "autoever"
#search: false
---

# pro.mincoding

## 기초 문법

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <math.h>

int main() {
	int a = 8;
	printf("%d",(int)pow(a,5));
}
```

## 2,8,10,16

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main()
{
  int a = 0b1111;
  int b = 017; //8진수는 숫자 앞에 0을 붙임
  int c = 15;
  int d = 0xF;

	printf("%d %d %d %d", a, b, c, d);
 }
```

## 16진수 출력

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main()
{
	//10진수 97을 16진수로 출력
	int a = 97;
	printf("%x", a);
 }
```

## 십진법 이진법 전환 코드

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

void print_bin(unsigned char reg) {
	unsigned char tmp = 0x80; // unsigned char를 사용한 이유는 크기가 8bit라서.
  //0x80은 0b100000000임.
	for (int i = 0; i < 8; i++) {
		((reg & (tmp >> i)) == (tmp >> i)) ? putchar('1') : putchar('0');
	}
	printf("\r\n");
}

int main () {
	unsigned char a = 0b10000101; // 10000101
	unsigned char b = 92; // 01011100
	print_bin(a);
	print_bin(b);
	return 0;
}
```

## max, min

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#define max(x,y) (x) > (y) ? (x) : (y) //c에는 max 함수가 없기에 이렇게 정의해야한다.
#define min(x,y) (x) < (y) ? (x) : (y)

int a, b, c, temp, max_n, min_n;

int main() {
	scanf("%d %d %d", &a, &b, &c);
	temp = max(a, b);
	max_n = max(temp, c);

	temp = min(a, b);
	min_n = min(temp, c);

	printf("MAX=%d\n", max_n);
	printf("MIN=%d", min_n);
}
```   
   
# C언어 소개

## 하드웨어에 적합

임베디드는 memory의 주소로 직접가서 값을 넣을 수 있음.   
```c
 *(volatile unsigned int *)0xABCD = 0x1234; // 주소 0xABCD에 0x1234를 넣는단 뜻.
```
✅C장점: 하드웨어 제어에 좋은 언어   
✅C단점:   
1. 하드웨어 제어에 좋음(오류뜨면 그냥 컴퓨터 멈춤. 오류 메시지도 안 보여줌)   
2. 대규모 프로젝트엔 적합하진 않음   
3. 객체 동적할당 후 해제해줘야함 (segment fault: 잘못된 메모리에 접근하는 오류. 많이뜸)   
   
## visual studio

visual stdio는 표준이 아닌걸 추천할 가 있음. scanf는 보안상 사용하지 않기를 권하면서 scanf_s를 추천한다.(참고로 임베디드에서 scanf 잘 안 씀) 하지만 scanf_s는 표준이 아니기에 사용하는 것을 지양해야한다. 또한   
```c
// 표준은 이러하지만
#ifndef _ABC_
#define _ABC_
//blah
#endif

// visual studio는 표준이 아닌 pragma를 추천함
#pragma
//blah
```   
   
# I/O functions

## fgets

c엔 여러 i/o 명령어가 존재한다. 문자열 입력엔 gets, fgets가 존재하는데 fgets를 사용하는 것이 좋다. gets는 입력 길이 제한이 없어서 버퍼 오버플로우를 유발할 수 있기 때문이다.   

```c
fgets(input, sizeof(input), stdin);
```

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>	

int main() {
	char input[100];
	if (fgets(input, sizeof(input), stdin) != NULL) { //fgets는 '\n'까지 입력으로 받아들임
		size_t len = strlen(input); // 입력된 문자열의 길이
		if (len > 0 && input[len - 1] == '\n') {
			input[len - 1] = '\0'; // 줄바꿈 문자를 널 문자로 대체
		}
		puts(input); // 문자열 출력에만 쓰임 & 끝나면 자동 개행
	}
	else {
		printf("입력 오류가 발생했습니다.\n");
	}
}
```

## visual studio

visual stdio는 표준이 아닌걸 추천해줄 때가 있음. scanf는 보안상 사용하지 않기를 권하면서 scanf_s를 추천한다.(참고로 임베디드에서 scanf 잘 안 씀) 하지만 scanf_s는 표준이 아니기에 사용하는 것을 지양해야한다. 또한   
```c
// 표준은 이러하지만
#ifndef _ABC_
#define _ABC_
//blah
#endif

// visual studio는 표준이 아닌 pragma를 추천함
#pragma
//blah
```
   
## fgets

c엔 여러 i/o 명령어가 존재한다. 문자열 입력엔 gets, fgets가 존재하는데 fgets를 사용하는 것이 좋다. gets는 입력 길이 제한이 없어서 버퍼 오버플로우를 유발할 수 있기 때문이다. 보통은 안 일어나지만 문제가 생길 소재가 있다.   

```c
fgets(input, sizeof(input), stdin);
```

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>	

int main() {
	char input[100];
	if (fgets(input, sizeof(input), stdin) != NULL) { //fgets는 '\n'까지 입력으로 받아들임
		size_t len = strlen(input); // 입력된 문자열의 길이
		if (len > 0 && input[len - 1] == '\n') {
			input[len - 1] = '\0'; // 줄바꿈 문자를 널 문자로 대체
		}
		puts(input); // 문자열 출력에만 쓰임 & 끝나면 자동 개행
	}
	else {
		printf("입력 오류가 발생했습니다.\n");
	}
}
```

## sprintf

sprintf는 어디 네트워크로 보낼때 해당 문자열로 보내야하기때문에 종종 사용한다하셨다. (알고있어야한다셨음) 
주로 문자열을 형식화(formatting) 할 때 사용한다.   
printf: 형식화된 문자열을 화면에 출력.   
sprintf: 형식화된 문자열을 변수에 저장. 문자열을 return하며 실패시 음수를 return.   
   
# 자료형

## 무슨 자료형

임베디드는 memory의 주소로 직접가서 값을 넣을 수 있음.   
```c
 *(volatile unsigned int *)0xABCD = 0x1234; // 주소 0xABCD에 0x1234를 넣는단 뜻.
```

변수 이름만 지우면 그 변수의 자료형이 나온다.(강사님께서 열변을 토하심)   
```c
int (*p1)[3];
int *p2[3]; //위아래 둘은 다름
// 2 + 3 * 5에서 3은 *에 붙음. 여기선 *보다 []가 우선순위
// p2는 오른쪽으로 붙어서 배열이 됨. p2는 포인터배열
// p1은 배열을 나타내는 포인터
```

```c
int a // 'int'형
int arr[4] // 'int 배열'형
int*** ptr // 'int 트리플 포인터'형
int (*p)[3] // 'integer 3개짜리를 가리키는 포인터'형
int** p[2][3] // '3개짜리가 2개 있는 배열인데 그 안에 int 더블 포인터가 들어있는 배열'형

int a = 123;
// int : data type
// a : variable name
// = : assignment operator
// 123: literal
```   

## 크기

```c
sizeof(int) = 4
sizeof(char) = 1
sizeof(float) = 4
sizeof(double) = 8
```

## 주소는 unsigned int 타입
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main() {
	int a = 11;
	const int* pa = &a;
	printf("%p\n", &a); // 주소는 'unsigned int' 타입임

	printf("a size is %zu\n", sizeof(a));

	return 0;
}
```

## int

int는 꼭 32bit가 아닐 수 있음. 환경에 따라 다름. 혹여나 데이터 전송 시 깨지더라도 당황하지 말자. 이때, 
C99 표준에서 도입된 stdint.h 헤더 파일은 고정된 크기의 정수형을 제공한다. 이 정수형은 플랫폼에 상관없이 크기가 항상 일정하다.   
uint8_t: 8비트 부호 없는 정수 (0 ~ 255)   
int8_t: 8비트 부호 있는 정수 (-128 ~ 127)   
uint16_t: 16비트 부호 없는 정수   
int32_t: 32비트 부호 있는 정수   
uint64_t: 64비트 부호 없는 정수   
   
## string

"hello": c에는 string이 없음. char[]임. 하지만 편의상 문자열형이라 부름. fgets를 이용하자.   

## literal과 symbolic

둘의 차이는 값으로 사용하냐 이름으로 사용하냐다.   

```c
// 여기서 숫자 3.14와 42는 literal 상수
radius = 10
area = 3.14 * radius ** 2
print(42)

PI = 3.14  // symbolic 상수로 정의 (상징적 상수라 생각하면 이해하기 편함)
MAX_LIMIT = 42

radius = 10
area = PI * radius ** 2
print(MAX_LIMIT)
```

## float와 double

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main()
{
	float f1 = 0.12345f; //경고안뜸
	float f2 = 0.12345; //경고뜸    왜냐면 0.12345는 double형임
	float f3 = (float)0.12345; //경고안뜸

	printf("%5.2f", f1);
 }
```

## 파생자료형

예를들면 구조체 같은 것. 기본형을 바탕으로 새로운 것을 만듦.   
   
# 비트

## 비트연산자 종류
<img src="https://github.com/user-attachments/assets/9f55bfb8-fda4-4fbe-be36-f227bf5a9df8" width="600" height="400">   

## 예시

✅예시1   
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main() {
	unsigned int a = 0b1001; // = 9
	unsigned int b = 0b1011; // = 11

	printf("(1) a & b  = %3d\r\n", a & b);
	printf("(2) a | b  = %3d\r\n", a | b);
	printf("(3) ~a     = %3d\r\n", ~a); // a = 0000 0000 0000 0000 0000 0000 0000 1001가 1111 1111 ~ 1111 0110 이 되기 때문
	printf("(4) a ^ b  = %3d\r\n", a ^ b);
	printf("(5) a << 1 = %3d\r\n", a << 1);
	printf("(6) a >> 1 = %3d\r\n", a >> 1);

	return 0;
}
```   
   
✅예시2   
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main() {

	unsigned int a = 15;// 0 1111
	unsigned int b = 20;// 1 0100

	printf("(1) a & b  = %3d\r\n", a & b);		// 4
	printf("(2) a | b  = %3d\r\n", a | b);		// 31
	printf("(3) ~a     = %3d\r\n", ~a);			// -16
	printf("(4) a ^ b  = %3d\r\n", a ^ b);		// 27
	printf("(5) a << 1 = %3d\r\n", a << 1);		// 30
	printf("(6) a >> 1 = %3d\r\n", a >> 1);		// 7

	return 0;
}
```

## 임베디드 비트연산

mcu에서 특정 회로를 키기 위해 사용   
```c
// ex) 모두 키고싶다
PORTA = 0xFF //PORTA = 0b11111111 : 이런식으로 해도되는데 빠질 수 있기에 보통 16진수로 표현함.
// 상위 4비트만 키고싶다
PORTB = 0xF0
```

# 각종 지식

## bit, byte

1bit = 0 or 1   
4bit = 니블   
8bit = 바이트   
Kilobyte = 1,024 Bytes   
Megabyte = 1,024 KB   
Gigabyte = 1,024 MB   
Terabyte = 1,024 GB   

## 엔퍼센트

'&' 코코노 나마에와 '엔퍼센트'. (오징어 아니라고 열변을 토하심)   

## 선언, 정의

선언: 컴파일러한테 "나 이거 쓸거야"   
정의: 메모리에 존재한다.   
```c
typedef struct _pp {
	int a;
	int b;
} pp; //선언

pp p; //정의
```

## 대입연산시 양쪽 자료형

대입연산시 양쪽의 자료형은 같아야함.   
```c
char a = 11; //은 되지만
int b = 22;
b = a; //는 안 됨

// char에서 int로 대입은 오류 뜨지만
// int에서 char로의 대입은 묵시적으로 허락해줌.

//int a = (int)('c') //이런건 데이터가 좀 변형된다하더라도 내가 우겨넣는거임.
```

## memset

memset(buf2, 0, sizeof(int)*SIZE)  --> 0이 8개 나옴   
memset(buf2, 1, sizeof(int)*SIZE) --> 이상한 값으로 나옴 --> 왜냐면   
0000 0000 / 0000 0000 / 0000 0000 / 0000 0000로 있는데   
0000 0001 / 0000 0001 / 0000 0001 / 0000 0001로 저장되기때문임. 다른 수도 같음   

따라서 memset은 0으로 초기화할 때만 사용   
1) memset   
2) int buf2[8] = {0, } // 이걸 선호   
3) 전역변수는 꼭 필요할 때만 사용   

##  void* function() {}

int든 float이든 자유롭게 올 수 있다는 뜻임.   

## 부동 소수점

IEEE764에서 고정 소수점 대신 부동 소수점을 사용하는 것을 정의한다.   
   
1) 고정 소수점(fixed point) 방식   
실수는 정수부와 소수부로 나눌 수 있다.   
<img src="https://github.com/user-attachments/assets/4854cd2f-e593-460c-bf49-fe93c6c02aae" width="600" height="200">   
그러나, 이 방식은 정수부와 소수부의 자릿수가 크지 않으므로, 표현할 수 있는 범위가 매우 적다는 단점이 있다.   
   
2) 부동 소수점(floating point) 방식   
실수는 정수부 소수부로 표현해도 되지만, 가수부와 지수부로 나누어 표현할 수도 있다. 당연히 매우 큰 실수까지도 표현할 수 있다.   
<img src="https://github.com/user-attachments/assets/90caf36d-eccf-40f0-aea5-c6c85e6dd27d" width="600" height="400">   
   
부동 소수점 오차가 발생하는 예제   
```c
#include <stdio.h>

int main() {
    float sum = 0.1f;
    for (int i = 0; i < 1000; i++) {
        sum += 0.1f;
    }
    printf("0.1을 1000번 더한 합계는 %f입니다.\n", sum);
    return 0;
}
```   
<img src="https://github.com/user-attachments/assets/64a80f51-c192-4991-ba45-99583816ba7f" width="800" height="200">   
따라서 부동 소수점 대소비교를 하면 안 된다. 이것은 C언어 뿐만 아니라 모든 프로그래밍 언어에서 발생하는 기본적인 문제이다. 추가적으로 부동소수점의 이진 표기법 변환 방법도 있는데 
복잡해서 나중에 필요하면 공부하려한다.   

## MSB, LSB

바이트에서 제일 왼쪽을 최상위비트 Most Significant Bit라 하고, 제일 오른쪽을 최하위 비트 LSB Least Significant Bit라 한다.   
<img src="https://github.com/user-attachments/assets/d5336337-f855-4e5d-a90a-5f888027150c" width="200" height="200">   
프로세서가 메모리에 저장할 때 하위비트를 기준으로 저장할건지, 상위 비트를 기준으로 저장할건지가 결정된다. 어떻게 저장하느냐에 따라 빅 엔디안, 리틀 엔디안으로 나뉜다.   

## 엔디안

컴구시간에 배웠었다. 이름이 인디안 같아서 재밌어서 집중 잘 됐던 기억이 있다. (여담이지만 필자는 컴구 수강생 100명이 넘는 시험에서 8등했었다.)   
   
주소가 나올 때   
00 00 00 0B (왜 이렇게 안나오냐)   
0B 00 00 00 (왜 이렇게 나오는가) : 리틀엔디안   
정답: 엔디안 때문   

빅 엔디안 vs 리틀 엔디안 (저장하는 차이); 우리가 앞으로 쓸 '암'에선 둘 다 지원하지만 통상 리틀을 사용함. (내부적으로 일어나는 작용이더라도
임베디드 개발자는 알아야함. 왜냐하면 시스템이 달라질 때 문제가 되기 때문임. 예를들어 윈도우에서 맥으로 데이터를 보낼 때 깨질 수 있음. 모르는 당혹스러울 수 있음)   

## 변수 주소에 저장된 값

✅변수 주소에 저장된 값 직접 찾아볼 땐 어떻게 하나?   

1) 왼쪽에 빨간색점으로 디버그 중단점을 지정한다   
<img src="https://github.com/user-attachments/assets/85cf8536-da8b-4548-ac11-fba248896032" width="100" height="600">   
2) ctrl + f5말고 f5를 누른다   
3) 좌측 하단에 조사식 클릭, 검색에 x, &x 등을 찾아볼 수 있음   
<img src="https://github.com/user-attachments/assets/16eb7df1-91dd-44e2-9f9d-70d906fa40dc" width="400" height="400">   
4) &x를 하고 메모장에 복풋하고 앞에 있는 주소만 복사   
<img src="https://github.com/user-attachments/assets/0a38af6d-0abc-4d70-ab66-5c3c3d06c84d" width="400" height="400">   
5) 우측 상단에서  디버그->창->메모리 클릭하고 치면 됨   
<img src="https://github.com/user-attachments/assets/9f025dbd-5690-44c8-98a7-8e827f1d95cd" width="400" height="400">   
6) 1이 01 00 00 00, 9는 09 00 00 00   
   
## struct 크기

```c
typedef struct _TT {
char // 5개
int // 1개
double // 1개
} TT; // 인데 이때 사이즈는 24임.
// 가장 큰놈 기준으로 두고 다른애들 채우는데 이때 미처 채워지지 못한 애들도 사이즈 가장 큰 거 사용해야해서 8 + (4+1+1+1+1) + 1
```

## 비트 최대값

8비트의 최소, 최대값: 0~255   
16비트 최소, 최대값: 65535   
   
## 음수 표기법

✅[복습] 10진수 35247이 메모리에 어떻게 저장이 되나?   
16진수 89 AF   
00 00 00 00 00 00 89 AF <- 이렇게 저장되지 않고   
AF 89 00 00 <- 리틀 엔디안 형태로 저장   
   
✅-1은 어떻게 저장되나?   
- -1을 구하는 방법   
    - 1을 이진수로 바꾸자!   
    - 1 = 0001   
- 0은 1로, 1은 0으로 바꾸자   
    - 0001 → 1110   
- 여기에 +1하자   
    - 1110 → 1111   
  
✅-3은 어떻게 저장되나?   
- -3을 구하는 방법   
    - 3을 이진수로 바꾸자!   
    - 3 = 0011   
- 0은 1로, 1은 0으로 바꾸자   
    - 0011 → 1100   
- 여기에 +1하자   
    - 1100 → 1101   
   
✅-1이 어떻게 저장되나?   
(1) 음수를 양수로 바꾼후 32비트에 대입   
-1 -> +1 -> 0000 0000 0000 0001   
(2) 1->0, 0->1   
(앞에 생략) 1111 1111 1111 1110   
(3) 바뀐값에 +1   
(앞에 생략) 1111 1111 1111 1111   
   
✅-231 이 어떻게 저장되나?   
(1)   
-231 ->231 -> E7 -> 1110 0111   
(앞에 생략) 0000 0000 1110 0111   
(2)   
(앞에 생략) 1111 1111 0001 1000   
(3) 바뀐값에 +1   
(앞에 생략) 1111 1111 0001 1001   
(앞에 생략) FF FF FF 19   
   
✅-1022는 어떻게 저장되나?
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdint.h>

int main() {
	int16_t b = 0b1111110000000010; // = 11

	printf("%d", b);

	return 0;
}
```   
   
## 보수법

보수: 어떤수를 보충하기 위해 필요한 수   
   
수학: 3에 대한 2의 보수: 1   
컴퓨터:   
✅1의 보수: 각 비트를 반전시킴   
원래 수:  0101 0011   
1의 보수: 1010 1100   
✅2의 보수: 주어진 숫자의 1의 보수에 1을 더한 값 (음수 표현할 때 씀)   

# 배열

## 배열의 특징

배열의 이름(=배열명)은 주소다!! 따라서 배열 = 배열; 연산 불가능하다. 또한 배열은 메모리에 연속적으로 저장된다.   
*(parr + 1) 배열이 있을 때 한 칸은 4byte이다. parr에서 4만큼 이동한 값   
*parr + 1 : parr값에서 1을 증가시켜라   

## 배열의 장단점

✅배열의 장점   
1. 빠른 접근 가능   
2. 메모리 사용량 쉽게 관리
   
✅배열의 단점   
1. 고정된 크기 (malloc 통해서 해결가능하긴함)   
2. 자료의 삽입,삭제의 비효율성   
3. 메모리의 낭비가 심하다!!! (200선언하고 5글자만 입력받으면...)   

# 함수

## call by ~

```c
#include <stdio.h>

// 값에 의한 전달 Call by value (10%)
void bts(int x) {
    x = 10; // 복사본만 변경됨
}

// 포인터를 사용한 원본 수정 Call by reference (90%)
void exo(int* x) {
    *x = 10; // 원본 데이터 변경
}

int main() {
    int a = 5;
    bts(a);
    printf("a: %d\n", a); // 값 변경되지 않음

    int b = 10;
    exo(&b);
    printf("b: %d\n", b); // 값이 변경됨
    return 0;
}
```

## 함수의 선언과 정의

함수를 보통 정의하고 main함수를 작성하지만 사실, 선언 먼저하고(=전방선언) main 함수 밑에 정의해도 작동됨. (은근 중요하다 하심)
또한 선언만 해도 컴파일은 됨. 다만 실행이 안 될 뿐.

## 함수 리턴을 여러 개

✅ 전역 변수 (되긴하지만 가장 안 좋은 방법)   
```c
#include <stdio.h>

int x = 0;
int y = 0;

void func(void) {
    x = 11;
    y = 22;
}

int main() {
    func();

    printf("x = %d\r\n", x);
    printf("y = %d\r\n", y);

    return 0;
}
```
✅ 참조 (위의 방법보단 그래도 낫다)   
```c
#include <stdio.h>

//int x = 0; // 전역변수는 정말 정말 필요할때가 아니면 쓰지 말자.
//int y = 0;

void func(int* x, int *y) {
    *x = 33;
    *y = 44;
}

int main() {
    int a = 0;
    int b = 0;
    func(&a, &b);

    printf("a = %d\r\n", a);
    printf("b = %d\r\n", b);

    return 0;
}
```
✅ 구조체 (공간복잡도가 커지면 복사할 때 안 좋다)   
```c
#include <stdio.h>

typedef struct _point_t {
	int x;
	int y;
} point_t;

point_t get_point() {
	point_t tmp_pt;
	tmp_pt.x = 33;
	tmp_pt.y = 44;
	return (tmp_pt);
}

int main() {
	point_t pt1 = { 0,0 };
	pt1 = get_point();

	printf("point(x,y) = (%d,%d)\r\n", pt1.x, pt1.y);

	return (0);
}
```   
   
✅ 구조체 포인터 (그나마 좋은듯)   
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

typedef struct _point_t {
	int x;
	int y;
} point_t;

void get_xy(point_t* point) {
	point->x = 33; // (*point).x = 33;
	point->y = 44; // (*point).y = 44;
}

int main() {
	point_t pt1 = { 0,0 };
	get_xy(&pt1);

	printf("point(x,y) = (%d,%d)\r\n", pt1.x, pt1.y);

	return (0);
}
```   
   
✅ 참조를 리턴하자 (이 방법은 매우 위험함)   
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

typedef struct _point_t {
	int x;
	int y;
} point_t;

point_t* get_xy() {
	point_t tmp_pt;
  //stack에서 저장돼서 tmp가 지워질 수 있음
	//하지만 point_t* tmp = (point_t*)malloc(sizeof(point_t)); 이렇게 한다면
  //heap에 저장돼서 지워지진 않음. 다만 관리하기 까다로움
	tmp_pt.x = 33; // (*point).x = 33;
	tmp_pt.y = 44; // (*point).y = 44;
	return (&tmp_pt);
}

int main() {
	point_t* pt1 = NULL;
	pt1 = get_xy();

	printf("point(x,y) = (%d,%d)\r\n", pt1->x, pt1->y);

	return (0);
}
```
✅ 값 객체(Value Object), 출력 매개변수

## 선언과 정의

함수 선언
```c
void func(); // 중복선언은 가능
```

함수 정의
```c
void func() { //중복정의는 불가능
}
```

## 매크로
매크로 함수란 #define문을 통해서 함수 처럼 동작하는 매크로를 말한다. 일반적인 함수와 전혀 상관 없음

✅#define문이 어떻게 컴파일 되는지 확인하기   
<img src="https://github.com/user-attachments/assets/339bdc04-670a-49d0-bac6-8eab58ff8296" width="500" height="500">   
위의 사진과 같이 프로젝트 속성 -> C/C++ -> 전처리기에 가서 파일로 전처리를 "예"로 바꿔준다. 그리고 프로젝트 파일에서 잘 뒤져서 main.i 파일을 찾는다. (파일 이름은 다를 수 있음) 찾은 main.i 파일을 리소스 파일에 추가하고,
ctrl + f5가 아닌 ctrl + shift + b를 눌러 빌드를 한다.   
   
ctrl+f5: 빌드+실행   
ctrl+shift+b: 컴파일(실행은 안 시키고)   

```c
#define _CRT_SECURE_NO_WARNINGS
#define aaa 111
#define bbb 222
#include <stdio.h>

int main() {

	printf("%d %d", aaa, bbb);

	return 0;
}
```   
그럼 보기 화면처럼 aaa, bbb가 111 222로 들어간 것을 볼 수 있다.   
<img src="https://github.com/user-attachments/assets/485c4a55-9e07-4d83-a540-f5a6946ac098" width="600" height="100">   

## main
main함수엔 여러 종류가 있다. 많이 본 형태는 요러할 것이다.
```c
int main () {
}
```   
   
✅그럼 아래와 같은 형태는 무엇일까?   
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(const int argc, const char* const argv[]) {  
    // main함수에 어떤 인자를 받고싶음.
    // 예를들면 ip주소를 받고싶음.
    // argc: 인자로 받는 문자 갯수:  192 168 0 1 
    // test 파일: test argv[0] 192 argv[1] 168 argv[2] 0 argv[3] argv[4]
    int i = 0;
    for (i = 0; i < argc; i++) {
        printf("[%d] argv[%d]: %s\n", (i + 1), i, argv[i]);
    }
    return 0;
}
```
<img src="https://github.com/user-attachments/assets/f688f0ec-5cc9-460f-8fad-4ea15d37b142" width="700" height="200">   
   
✅파일 열어보기
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <stdlib.h>

#define NO_ERR (0)
#define ERR_NO_FILE_NAME (-1)
#define ERR_FILE_NOT_FOUND (-2)

int main(int argc, char* argv[]) {
    //printf("argc: %d\r\n", argc);
    //for (int i = 0; i < argc; i++) {
    //    printf("argv[%d]: %s\r\n", i, argv[i]);
    //}

    FILE* fp = NULL;
    char file_name[32] = { 0, };
    int exit_code = 0;

    //if ((file_name==NULL)||(strlen(file_name)==0)) { // 이렇게 할 필요가 없잖아! ㅋㅋ
    if (argc <= 1) { // 파일 이름을 입력하지 않은 경우... "니가 파일이름 입력 안 한 거야"
        exit_code = ERR_NO_FILE_NAME;
        printf("exit_code= %d: 파일 이름을 입력하지 않았네요.\r\n", exit_code);
    }
    else { // 파일 이름은 입력했으나 파일이 없는 경우
        //if (argv[1] != NULL) { strcpy(file_name, argv[1]); }
        //strcpy(file_name, "test.txt");
        strcpy(file_name, argv[1]);

        fp = fopen(file_name, "r");
        if (fp == NULL) { // 파일 이름 입력은 했지만, 해당 파일이 없음
            exit_code = ERR_FILE_NOT_FOUND;
            printf("exit_code= %d: %s 파일을 찾을수 없어요.\r\n", exit_code, file_name);
        }
        else { // 파일 이름도 입력했고, 파일도 찾아서 정상적으로 open 한 경우
            exit_code = NO_ERR;
            printf("exit_code= %d: %s 파일을 열었습니다.\r\n", exit_code, file_name);
#define BUF_SIZE (128)
            char buf[BUF_SIZE] = { 0, };
            fread(buf, sizeof(char), BUF_SIZE - 1, fp);
            printf("%s\r\n", buf);
        }
    }

    return exit_code;
}
```
   
1)위의 코드를 작성   
2)폴더->x64->Debug로 들어가서 start.bat , hello.txt 파일 만들기   
3)start.bat에는   
```java
@echo off
echo Shell Started...
:repeat
set /p str=input filename...
rem echo string is %str%
ConsoleApplication5.exe %str%
rem echo [Check Exit Code]: %errorlevel%
if %errorlevel% EQU -1 goto repeat
if %errorlevel% EQU -2 goto repeat
echo Shell Terminated...
pause
```
라고 작성하기   
4) hello.txt에는 아무렇게나 출력하고 싶은 문구 작성   
5) 해당 폴더에서 cmd를 검색한다.   
<img src="https://github.com/user-attachments/assets/02ea5807-e237-4023-b22b-f47c13dc7b7c" width="700" height="700">   
6) 뜬 cmd 창에서 start 누르면 새로 또 뜸. 거기서 명령어 입력하면됨   
<img src="https://github.com/user-attachments/assets/5a7f9e33-e119-4026-af7f-5be7da640720" width="700" height="700">   
   
✅정리
```c
// 인자를 받을 필요가 없으면 

int main () {
	return 0;
}//하면 되지만

// 받는다면
int main (int argc, const char* argv[]) { 
	int exit_code = -1;
	return 0;
	
	if (파일열기가 실패하면) {
		return exit_code;
	}
	else {
		작동
	}
}
```

# sht85 온도센서 만들기 실습

## 개요

임베디드 코딩은 특정 장치를 키고 끄는 코딩을 많이 한다. 센서에 해당하는 데이터형을 만들고, 
센서의 동작을 함수로 구현한다. "어떤 동작을 할 것인가?"라는 생각을 갖고 구현하면된다.(온도를 읽겠지, 센서 같은 경우엔 초기화해야할 수 있음)

```c
double temp;
double humi;를 따로 두면 연관성이 없어보이니 구조체로 묶음.

typedef struct _sht85_t { //구조체 이름, 함수 이름 잘 지어야함.
	double temp; // 데이터 사이즈 줄이거나 패킷을 줄이기 위해 int로 설정해도됨. 주고받은 후 나누기 10하면 되기때문.
	double humi;
} sht85_t;
```

## sht85.h
```c
#pragma once
#include <stdio.h>

//구조체 선언
typedef struct _sht85_t { //구조체 이름, 함수 이름 잘 지어야함.
	double temp;
	double humi;
	int cmd;
	int a;
} sht85_t;

sht85_err_t sht85_init(sht85_t * sht85);
void sht85_read(sht85_t* sht85); //main.c 목록 내려보면 함수들이 알파벳순으로 정렬돼서 보기 편함.
void sht85_write(sht85_t* sht85, int cmd);
void sht85_printf(sht85_t* sht85);

typedef enum _sht85_err_t { //mcu가 센서와 연결되고 초기화할 때 문제가 있으면 화면에 알려주기 위해.
	SHT_OK = 0x00, // no error
	SHT_ERR_WRITECMD = 0x81, // I2C write failed
	SHT_ERR_READBYTES = 0x82, // I2C read failed
	SHT_ERR_HEATER_OFF = 0x83 // Could not switch off heater
} sht85_err_t;
```

## sht85.c
```c
#include "sht85.h"
#include <stdio.h>

//멤버 변수 초기화
sht85_err_t sht85_init(sht85_t* sht85) {
	sht85_err_t err = SHT_ERR_HEATER_OFF;
	sht85->temp = 0.0;
	sht85->humi = 0.0;
	sht85->cmd = 0;
	sht85->a = 0;
	return err;
}

// 하드웨어에 있는 값을 구조체로 옮김
void sht85_read(sht85_t* sht85) {
	sht85->temp = 12.34;
	sht85->humi = 56.78;
}

void sht85_write(sht85_t* sht85, int cmd) { //cmd는 mcu에서 센서 모드 바꿀 때 씀.
	//예를들어 저전력으로 구성하고싶다면 
	// 1. mcu에 내재된 clock이 스스로 함수 작동
	// 2. 외부에 연결된 버튼 누르면 함수 작동
	sht85->cmd = cmd;
}

void sht85_print(sht85_t* sht85) {
	printf("(temp,humi)=(%4.2f, %4.2f)", sht85->temp, sht85->humi);
}
```

## main.c
```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include "sht85.h"

int main() {

	sht85_t sht85;

	sht85_err_t sht85_err;
	sht85_err = sht85_init(&sht85);

	switch (sht85_err) {
	case SHT_OK:
		// asdfkjasdklj
		break;
	case SHT_ERR_WRITECMD:
		// asdfkljasdkjlfds
		break;
	case SHT_ERR_READBYTES:
		// asdkfjasldkjfjk
		break;
	case SHT_ERR_HEATER_OFF:
		// klajsdfkjasdfjadkjlf
		break;
	}
	sht85_init(&sht85);
	sht85_read(&sht85);
	sht85_print(&sht85);

	return 0;
}
```

# 구조체

## 개요

## strcpy

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include <string.h>

typedef struct _book_t {
	char title[128];
	int price;
	char author[128];
} book_t;

int main() {
	book_t book1 = { "이런책",1000,"하쿠" };
	book_t book2 = { "저런책",1200,"겨울" };
	strcpy(book2.title, "그런책");
	book2.price = 2000;
	
	printf("%s %d %s\n", book1.title, book1.price, book1.author);
	printf("%s %d %s", book2.title, book2.price, book2.author);
}
```

## 실습: library

<img src="https://github.com/user-attachments/assets/80d6d038-d039-42a7-9ea4-5a425e3d3c71" width="700" height="700">   
   
✅book.c   
```c
#include "book.h"

void book_init(book_t* book, char* title, char* author, int price) {
	strcpy(book->title, title);
	strcpy(book->author, author);
	book->price = price;
}

void book_print(book_t* book) {
	static count = 0;
	printf("[%d] %s / %s / \\%d\r\n", ++count, (*book).title, (*book).author, (*book).price);
}

void books_print(book_t(*books)[], int count) {
	for (int i = 0; i < count;i++) {
		book_print((*books) + i);
	}
}
```   
✅library.c
```c
#include "library.h"

void lib_init(library_t* lib, char* name) { // 도서관을 초기화
	strcpy(lib->name, name);
	lib->count = 0;
	for (int i = 0; i < MAX_BOOKS; i++) {
		lib->books[i] = NULL;
	}
}

void lib_add_book(library_t* lib, book_t* book) { // 도서관에 책을 들여옴(=신규서적)
	lib->books[lib->count++] = book;
}

void lib_remove_book(library_t* lib, book_t* book) { // 도서관에서 책이 나감(=분실됨 or 파기됨)
}

void lib_print_all_books(library_t* lib) { // 도서관의 모든 책을 보여줘
	for (int i = 0; i < lib->count; i++) {
		printf("[%d] ", (i + 1));
		book_print(lib->books[i]);
	}
}

//void borrow_book(book_t* book) { // 책을 빌리다. 
// }


//void return_book(book_t* book) { // 책을 반납하다.
//}
```   
✅main.c   
```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include <string.h>

#include "book.h"
#include "library.h"

int main(void) {

	// 책
	book_t book[3];

	book_t sel_books[3]; // samsung_elementary_literaure_books 삼성 초등 문학 전집 sel_books
	book_t mel_books[3]; // 민음사.. 

	book_init(&sel_books[0], "15 소년 표류기", "쥘 베른", 5555);
	book_init(&sel_books[1], "장발장", "빅토르 위고", 5555);
	book_init(&sel_books[2], "걸리버 여행기", "조나단 스위프트", 5555);

	book_init(&mel_books[0], "리어왕", "윌리엄 셰익스피어", 4444);
	book_init(&mel_books[1], "동물 농장", "조지 오웰", 4444);
	book_init(&mel_books[2], "파리대왕", "윌리엄 골딩", 4444);

	books_print(&sel_books, 3);
	books_print(&mel_books, 3);
	printf("\n\n\n");

	// 도서관
	library_t lib;
	lib_init(&lib, "남산");

	lib_add_book(&lib, &sel_books[0]); // "야옹이 수영교실" 을 구매함
	lib_add_book(&lib, &sel_books[1]); // "천개산 패밀리 1"을 구매함
	lib_print_all_books(&lib); // 도서관에 있는 모든 책을 보여줘.

	lib_add_book(&lib, &mel_books[2]); // "세상은 이야기로 만들어졌다"을 추가로 구매함
	lib_print_all_books(&lib); // 도서관에 있는 모든 책을 보여줘.

	return 0;
}
```   
✅book.h   
```c
#define _CRT_SECURE_NO_WARNINGS

#ifndef __BOOK_T__
#define __BOOK_T__

#include <stdio.h>
#include <string.h>

typedef struct _book_t {
	char title[128];
	char author[128];
	int price;
} book_t;

void book_init(book_t* book, char* title, char* author, int price);
void book_print(book_t* book);
void books_print(book_t(*books)[], int count);

#endif
```   
✅library.h   
```c
#ifndef __LIBRARY_T__
#define __LIBRARY_T__

#include "book.h"

#define MAX_BOOKS (3)

typedef struct _library_t {
	char name[64]; // 도서관 이름, 예를 들면 남산 도서관
	int count; // 책의 총 갯수
	book_t* books[MAX_BOOKS]; // 책장, 서가?
} library_t;

void lib_init(library_t* lib, char* name); // 도서관을 초기화
void lib_add_book(library_t* lib, book_t* book); // 도서관에 책을 들여옴(=신규서적)
void lib_remove_book(library_t* lib, book_t* book); // 도서관에서 책이 나감(=분실됨 or 파기됨)
void lib_print_all_books(library_t* lib); // 도서관의 모든 책을 보여줘     
//void borrow_book(book_t* book); // 책을 빌리다. 
//void return_book(book_t* book); // 책을 반납하다.

#endif
```   

## union
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

typedef union _jw_t {
	int a;
	int b;
} jw_t;

int main() {

	jw_t jw;
	jw.a = 11; jw.b = 22;

	printf("%d %d", jw.a, jw.b);

	return 0;
}
```

## 구조체 비트 필드

```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include <string.h>
#include <stdint.h>

typedef struct _bts_t {
	uint8_t a : 4;
	uint8_t b : 3;
	uint8_t c : 1; //단, a,b,c 합쳐서 8이어야함.

	uint8_t d;
	uint8_t e;
	uint8_t f;
	//a b c는 세 개 합쳐서 8byte고 d e f는 각각 8byte임.
} bts_t;


int main(void) {
	bts_t bts;
	bts.a = 0x0F;
	bts.b = 0x07;
	bts.c = 0x01; //c는 1bit까지만 저장이 돼서 0~1까지 밖에 안 됨.

	printf("bts.a, bts.b, bts.c= (%d,%d,%d)\r\n", bts.a, bts.b, bts.c);

	// 범위를 넘어가면 어찌될까?
	bts.a = 0x10;
	bts.b = 0x08;
	bts.c = 0x02;
	// 범위를 넘어가는 값은 무시된다.
	printf("bts.a, bts.b, bts.c= (%d,%d,%d)\r\n", bts.a, bts.b, bts.c);

	return 0;
}
```

## 구조체 "="연산
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

typedef struct _jw_t {
	int a;
	int b;

}jw_t;


int main(void) {

	jw_t a;
	a.a = 11;
	a.b = 12;

	jw_t b;
	b.a = 13;
	b.b = 14;

	b = a;

	printf("%d %d", b.a, b.b); // 11 12 출력됨
	return 0;
}
```

## 구조체 안에 배열

구조체 안에 배열을 넣는 것은 좋지 않다. 왜냐하면 padding때문이다. 배열을 넣더라도 배열의 참조를 넣는 것이 좋다!   

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int small_font[5] = { 11, 22, 33, 44 };
int big_font[5] = { 55, 66, 77, 88 };

typedef struct _lcd_t {
    int x;
    int y;
    int* font;  //font_t* font; 포인터 크기는 8byte. 따라서 구조체 크기는 16
} lcd_t;

print_font(lcd_t* lcd) {
    for (int i = 0; i < 4; i++) {
        printf("%d,", lcd->font[i]);
    }
}

int main() {

    lcd_t lcd;

    lcd.x = 3;
    lcd.y = 5;

    lcd.font = small_font;
    print_font(&lcd);

    return 0;
}
```

## 구조체 안의 공용체, 공용체 안의 구조체
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

struct bts_t {
    int a;
    union {
        int b;
        int c;
    };
}; //union은 하나임. 따라서 크기는 12가 아닌 8임.

union ses_t {
    int a;
    struct { //이름 빼도됨. 즉, struct ses 이런식으로 안 하고 그냥 struct라 해도됨. 홍길동이라는 union안에 홍길동의 왼쪽눈이란 구조체라고 만들 필요없이 그냥 왼쪽눈이라 두면됨.
        int b; // a와 b가 겹침.
        int c;
    };
}; //크기는 

int main() {

    struct bts_t bts;

    bts.a = 11;
    bts.b = 22;
    bts.c = 33;
    printf("%d %d %d\r\n", bts.a, bts.b, bts.c); // 11 33 33

    union ses_t ses;
    ses.a = 44;
    ses.b = 55;
    ses.c = 66;
    printf("%d %d %d\r\n", ses.a, ses.b, ses.c); // 55 55 66 

    return 0;
}
```

## 공용체의 유용함: 패킷의 바이트 파싱

```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include <stdlib.h>
#include <stdint.h>

#define CTP_SIZE (7)

typedef union _ctp_t {
    uint8_t all[CTP_SIZE];
    struct {
        uint8_t head;
        uint8_t body[5];
        uint8_t tail;
    } subset;
} ctp_t;

void print_ctp(const ctp_t* ctp) {
    //printf("%02X ", ctp->head);
    for (int i = 0; i < CTP_SIZE; i++) {
        printf("%02X ", ctp->all[i] );
    }
    //printf("%02X ", ctp->tail);
}

int main() {
    uint8_t rx_data[CTP_SIZE] = { 0xAA, 0x12, 0x13, 0x14, 0x15, 0x16, 0xBB };
    ctp_t ctp = { 0, };

    memcpy(ctp.all, rx_data, CTP_SIZE);
    //ctp.head = rx_data[0];
    //ctp.body[0] = rx_data[1];
    //ctp.body[1] = rx_data[2];
    //ctp.body[2] = rx_data[3];
    //ctp.body[3] = rx_data[4];
    //ctp.body[4] = rx_data[5];
    //ctp.tail = rx_data[6];

    print_ctp(&ctp);
    return 0;
}
```

## 공용체의 유용함: 레지스터의 비트 파싱

<img src="https://github.com/user-attachments/assets/34616679-0763-4c1a-ae48-a22e35bf5a09" width="700" height="700">   

```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include <string.h>
#include <stdint.h>

typedef union _uartdr_t {
	uint16_t all; // (1) 요게 뽀인뜨! 중요하닷!
	struct {
		uint16_t reserved : 4; // (2) reserved, oe, be 적는 순서를 신경쓰는게 좋다.
		uint16_t oe : 1; // oeverrun error
		uint16_t be : 1; // break error
		uint16_t pe : 1; // parity error
		uint16_t fe : 1; // framing error
		uint16_t data : 8;
	} bits;
} uartdr_t;

void init_uartdr(uartdr_t* uartdr) {
	uartdr->bits.reserved = 0;
	uartdr->bits.oe = 0;
	uartdr->bits.be = 0;
	uartdr->bits.pe = 0;
	uartdr->bits.fe = 0;
	uartdr->bits.data = 0;

	uartdr->all = 0;
}

void print_all_bits_uartdr(uartdr_t* uartdr) {
	printf("%02X", uartdr->all);
}

int main(void) {
	uartdr_t uartdr;

	init_uartdr(&uartdr);

	//uartdr.bits.oe = 1;
	//uartdr.bits.be = 1;
	//uartdr.bits.pe = 1;
	//uartdr.bits.fe = 1;
	//uartdr.bits.data = 0x42; // 'B' 
	// 0000 1111 0100 0001 = 0F 42
	
	uint16_t rx_data = 0b0000111101000010;
	uartdr.all = rx_data;

	print_all_bits_uartdr(&uartdr);

	return 0;
}
```

## 패딩

✅ 패딩 예시
```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include <stddef.h>
#include <stdint.h>

// 컴파일할 때 _node 전체를 한번 훑고 최대 사이즈가 4인걸 보고 구조체 사이즈를 조정한다.
// 변수 사이즈 순서에 따라 크기가 달라진다.
// abcd -> 16
// acbd -> 12
struct _node {
	uint16_t a; // 2
	uint8_t  c;  // 1
	uint32_t b; // 4
	uint32_t d; // 4
};
int main(void) {

	struct _node node = { 1,2,3,4 };
	printf("%d, %d, %d, %d\n", node.a, node.b, node.c, node.d);

	printf("%d\n", offsetof(struct _node, a));
	printf("%d\n", offsetof(struct _node, b));
	printf("%d\n", offsetof(struct _node, c));
	printf("%d\n", offsetof(struct _node, d));
	printf("%d\n", sizeof(node));

	return 0;
}
```

✅ pragma pack   
```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include <stddef.h>
#include <stdint.h>

// 컴파일할 때 _node 전체를 한번 훑고 최대 사이즈가 4인걸 보고 구조체 사이즈를 조정한다.
// 변수 사이즈 순서에 따라 크기가 달라진다.
// abcd -> 16
// acbd -> 12

#pragma pack(push, 1) // 1은 정렬단위 숫자다. 여기엔 2 3 4...등 숫자가 자유롭게 들어갈 수 있다.
// 이렇게 하면 1단위로 padding이 사라진다. 즉, padding이 아예 없어져 순서를 어떻게 바꾸든 size는 11이다.
struct _node {
	uint16_t a; // 2
	uint32_t b; // 4
	uint8_t  c;  // 1
	uint32_t d; // 4
};
#pragma pack(pop)

int main(void) {

	struct _node node = { 1,2,3,4 };
	printf("%d, %d, %d, %d\n", node.a, node.b, node.c, node.d);
	
	printf("%d\n", offsetof(struct _node, a));
	printf("%d\n", offsetof(struct _node, b));
	printf("%d\n", offsetof(struct _node, c));
	printf("%d\n", offsetof(struct _node, d));
	printf("%d\n", sizeof(node));

	return 0;
}
```

# 문자열

## 첫번째 문자열 예제: char[] vs char*
```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h.>
#include <string.h>

int main() {
#define STR_SIZE 32
	unsigned char  str1[STR_SIZE + 1] = { 'a', 'b', 'c', 'd', 'e', 0 }; // (1) 배열로 만들기
	unsigned char* str2 = "xyz"; // (2) 포인터로 만들기

	printf("%s\r\n", str1);
	printf("%s\r\n", str2);

	printf("%zu\r\n", strlen(str1));
	printf("%zu\r\n", strlen(str2));

	// 문자열 내의 요소 출력하기
	printf("%c %c %c\r\n", str1[0], str1[2], str1[4]);
	printf("%c %c %c\r\n", str2[0], str2[1], str2[2]);
	printf("%c %c %c\r\n", *(str2 + 0), *(str2 + 1), *(str2 + 2));
	
	return 0;
}
```

✅ 어렵지는 않지만 알아두어야 할점   
1. 1차원 배열의 경우 배열 요소 갯수를 생략할수 있다.   
2. 2차원 배열에서도 첫 요소 갯수는 생략가능하다.   
  - 예를 들어 str_arr[10][16]에서 10은 생략 가능. 16은 생략 불가능!   
  - 근데 보통 문자열은 2차원을 쓰는 경우가 드물다.   

✅ 초기화 안 하고 strcpy  
```c
	char dst[10] = { 0, };
	char scr[10];

	strcpy(dst, src); // 캐릭터 배열은 맨 마지막에 0이어서 size 안 넘겨줘도 됨. 초기화 안 했을 경우는 0 만날 때까지 strcpy됨.
```

✅ 문자열 다룰때 주의할점   
   
🔹 널 종료 (\0)   
문자열은 반드시 \0로 끝나야 한다.   
문자열 작업 시 이를 잊지 말자.   
   
🔹 버퍼 오버플로우   
문자열 작업 시 배열 크기를 초과하지 않도록 주의해야 한다.   
이를 위해 안전한 함수 (strncpy, snprintf 등)를 사용하는것도 방법이다.   
   
🔹 문자열 상수 변경 금지   
문자열 상수는 수정할 수 없습니다.   
만일 변경하려면 배열로 복사해서 사용해야 한다.   
   
🔹 메모리 관리   
동적 메모리를 사용할 경우, malloc으로 할당한 메모리는 작업 후 반드시 free로 해제하자.   
   
🔹 문자열 생성시 항상 +1개 로 만들어라   

🔹 문자열 중간에 ‘\0’이 있다면?
나의 의도와는 다르게 중간까지만 읽는다. 주의하자. 문자열을 결합할때 종종 생긴다. 이전 문자열의 0을 떼버리고 연결해야 한다.   

## 문자열 끝의 0은 뭔가? 0, ‘\0’, ‘0’

✅ 부저 소리를 출력해보자   
요새 컴퓨터는 “띠로링” 소리가 난다.   
```c
#include <stdio.h>

int main() {
	//char beep1 = 7;
	//printf("%c", beep1);

	char beep2 = '\007';
	printf("%c", beep2);

	return (0);
}
```   
   
✅ 아니 그럼 편하게 7이라고 쓰지 왜 번거롭게 ‘\007’이라고 쓰나요?   
```c
#include <stdio.h>

int main() {
	char* str1 = "[1] 4567789";
	printf("%s\r\n", str1);

	return (0);
}
```   
그냥 4567789가 나온다. 내가 원한게 아니야. 
여기서 7은 BEL beep 소리 = 0x07이 아니고 0x37로 인식하기 때문이다.   
   
✅ 문자열 중간에는 \xxx 이렇게 출력하면 편하다.   
```c
#include <stdio.h>

int main() {
	char* str3 = "[1] 456\007789";
	printf("%s\r\n", str3);

	printf("[2] 456\007789\r\n");

	return (0);
}
```   
   
✅ 결론: 7이나 \007이나 똑같다.   
걍 7 써라. 문자열 내에 있지 않다면   
   
✅ 똑같은 원리로 0이나 \0이나 똑같다.   
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main() {
	char str1[8] = { 'a', 'b', 'c', '\0' }; // (1)
	char str2[8] = { 'a', 'b', 'c', 0 }; // (2)
	char str3[8] = { 0x61, 0x62, 0x63, 0x00 }; //(3)
	char str4[8] = { 0x61, 0x62, 0x63, 0 }; //(4)
	char str5[8] = { 0x61, 0x62, 0x63, '\0' }; // (5)
	char str6[8] = { 0x61, 0x62, 0x63, '0' }; // (6)
	char str7[8] = "abc";

	printf("[1] %s\r\n", str1);
	printf("[2] %s\r\n", str2);
	printf("[3] %s\r\n", str3);
	printf("[4] %s\r\n", str4);
	printf("[5] %s\r\n", str5);
	printf("[6] %s\r\n", str6);
	printf("[7] %s\r\n", str7);

	return (0);
}
```   
   
✅ ‘0’은 0과 아무 상관없다.   
0x30이다.   

## string.h

✅ strcpy   

```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include <string.h>

void my_strcpy(char* dst, char* src) {
	if (dst == NULL) return;
	if (src == NULL) return;

	while (*src != '\0') {
		*dst = *src;
		src += 1;
		dst += 1;
	}
	*dst = '\0';
	return;
}

int main(void) {

	char dst[32] = { 0, };
	char src[32] = "aaa yyy zzzz aaa";

	my_strcpy(dst, src);
	printf("%s\n", dst);
	printf("%s\n", src);

	return 0;
}
```

✅ strcat   
c++의 append() 기능을 한다.   
   
✅ strcmp, strncmp   
   
strcmp   
```c
#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "hello";
    char str2[] = "world";
    char str3[] = "hello";

    printf("strcmp(str1, str2): %d\n", strcmp(str1, str2));  // 음수
    printf("strcmp(str1, str3): %d\n", strcmp(str1, str3));  // 0

    return 0;
}
```   
   
strncmp
```c
#include <stdio.h>
#include <string.h>

int main() {
    char str1[] = "hello";
    char str2[] = "helicopter";

    printf("strncmp(str1, str2, 3): %d\n", strncmp(str1, str2, 3));  // 0 (앞 3글자가 같음)
    printf("strncmp(str1, str2, 5): %d\n", strncmp(str1, str2, 5));  // 양수 (4번째 문자부터 다름)

    return 0;
}
```

## 얕은 복사, 깊은 복사

✅ 얕은 복사   
```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include <stdint.h>
#include <string.h>

typedef struct _stormtrooper_t {
	char* code_name; //= class: sand, magma, dewback...
	uint16_t year; // production year
} stormtrooper_t;

void print_stormtrooper(stormtrooper_t* s) {
	printf("%s, %d\r\n", s->code_name, s->year);
}

int main() {
	char sand_code[8] = "s100";
	char magma_code[8] = "m100";

	printf("샌드트루퍼를 하나 만들자.\r\n");
	stormtrooper_t s1;
	s1.code_name = sand_code;
	s1.year = 23; // 23년식
	
	print_stormtrooper(&s1);

	printf("마그마트루퍼를 하나 만들자.\r\n");
	stormtrooper_t s2;// = { "m100", "magma-trooper", 24 };
	s2.code_name = magma_code;
	s2.year = 24; // 24년식, 더 최신식
	print_stormtrooper(&s2);

	printf("마그마트루퍼를 샌드트루퍼로 복사하자.\r\n");
	//s1 <- s2 복사하자.
	s1 = s2;

	print_stormtrooper(&s1);
	print_stormtrooper(&s2);

	printf("마그마트루퍼 병과를 파이어트루퍼 병과로 바꾼다.\r\n");;
	strcpy(magma_code, "f100");

	print_stormtrooper(&s1);
	print_stormtrooper(&s2);

	return 0;
}

```   
   
✅ 깊은 복사   
```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include <stdint.h>
#include <string.h>

typedef struct _stormtrooper_t {
	char* code_name; //= class: sand, magma, dewback...
	uint16_t year; // production year
} stormtrooper_t;

void print_stormtrooper(stormtrooper_t* s) {
	printf("%s, %d\r\n", s->code_name, s->year);
}

int main() {
	char sand_code[8] = "s100";
	char magma_code[8] = "m100";

	printf("샌드트루퍼를 하나 만들자.\r\n");
	stormtrooper_t s1;
	s1.code_name = sand_code;
	s1.year = 23; // 23년식

	print_stormtrooper(&s1);

	printf("마그마트루퍼를 하나 만들자.\r\n");
	stormtrooper_t s2;
	s2.code_name = magma_code;
	s2.year = 24; // 24년식, 더 최신식
	print_stormtrooper(&s2);

	printf("마그마트루퍼를 샌드트루퍼로 복사하자.\r\n");
	// s1 = s2; 이렇게 하지말고, 내용을 복사하자.
	strcpy(s1.code_name, s2.code_name);
	s1.year = s2.year;

	print_stormtrooper(&s1); 
	print_stormtrooper(&s2); 

	printf("마그마트루퍼 병과를 파이어트루퍼 병과로 바꾼다.\r\n");
	strcpy(magma_code, "f100");

	print_stormtrooper(&s1);
	print_stormtrooper(&s2); 

	return 0;
}
```   
   
✅ 깔끔한 해결책   
```c
#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>
#include <stdint.h>
#include <string.h>

typedef struct _stormtrooper_t {
	char* code_name; //= class: sand, magma, dewback...
	uint16_t year; // production year
} stormtrooper_t;

void print_stormtrooper(stormtrooper_t* s) {
	printf("%s, %d\r\n", s->code_name, s->year);
}

int main() {
	char sand_code[8] = "s100";
	char magma_code[8] = "m100";

	printf("샌드트루퍼를 하나 만들자.\r\n");
	stormtrooper_t s1 = { sand_code, 23 };
	print_stormtrooper(&s1);

	printf("마그마트루퍼를 하나 만들자.\r\n");
	stormtrooper_t s2 = { "m100", 24 };
	print_stormtrooper(&s2);

	printf("마그마트루퍼를 샌드트루퍼로 복사하자.\r\n");
	// 대입 연산자 말고 그냥 메모리 카피해..
	memcpy(&s1, &s2, sizeof(s2));
	print_stormtrooper(&s1);
	print_stormtrooper(&s2);

	printf("마그마트루퍼 병과를 파이어트루퍼 병과로 바꾼다.\r\n");
	strcpy(magma_code, "f100");

	print_stormtrooper(&s1); //m100, 23
	print_stormtrooper(&s2); //m100, 24
	return 0;
}
```

# 포인터

## 포인터란?

✅ 포인터   
포인터 = 주소. 자료형은 int형이 아니라 int*형이다. 포인터 변수는 주소가 저장된 변수다.   
   
✅ 첫번째 포인터 예제   
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main(void) {

  char* name; // wild pointer: 초기화하지 않은 포인터
	int* name = NULL; 
	// name이 0을 가리키고있다. (x)
	// name이 유효하지 않은 번지를 가리키고 있다. (o)

	int a = 999;
	int* pa = &a; // pa에 a의 주소를 대입(=저장)함, 이제 *pa는 a와 쌤쌤
	// 즉 a를 쓰는 대신, *pa를 써도 무방하다.

	printf("a= %d\r\n", a); // 11
	printf("&a= %p\r\n", &a); // a의 주소
	printf("*pa= %d\r\n", *pa); // 11    역참조
  printf("*pa= %p\r\n", &a);

  // 포인터 변수 *pa가 a를 가리키다.
  // a 대신에 *pa를 써도 상관없다.
  // scanf("%d", &a);
  // scanf("%d",&(*pa));

	// 포인터는 변수이므로 다른 값을 대입해도 된다.
	int b = 22;
	pa = &b;
	printf("*pb= %d\r\n", *pa);

	return 0;
}
```   
   
✅ 포인터 변수의 선언과 정의   
```c
int a; // 선언이자 정의임. 실제 메모리 공간할당 O, 실제 메모리 주소를 잡아먹으나 그 값이 쓰레기일뿐.
int* pa; // 포인터 변수의 선언, 실제 메모리 공간할당 X, 단순히 int*를 쓸꺼야 라는걸 컴파일러에게 알려준다.

int b= 22;
int* *pb= &b; // 포인터 변수의 선언 및 정의, 실제 메모리 공간 할당 O, 
```   
   
✅ 이렇게 선언하면 안된다   
```c
int* pa, pb; // 하나는 int 포인터요, 다른 하나는 int 형이다.
int* pa, *pb;
```   
   
아마도 int를 이렇게 선언하는게 습관이 되다 보니 위와 같이 하는것 같다.   
```c
int a, b;
```   
   
정신건강에 좋게 이렇게 쓰자.   
```c
int* pa;
int* pb;
```

## 역참조 연산자 vs 주소 연산자

✅ 역 참조 연산자 *는 이름이 많다   
참조 연산자, 역참조 연산자, 간접 참조 연산자, 간접 지정 연산자, Dereference Operator.   
영어 표현을 가장 정확하게 표현하는거 같아서 우리도 주로 역참조 연산자라 부르자.   

## 주소의 정확한 데이터형은 void*, intptr_t, uintptr_t

✅ 주소의 정확한 데이터형은 void*   
근데 void*는 어떤 타입의 메모리 주소도 담을수 있다. 즉, 함수 인자에 void*를 쓰면 int*, char* 등 아무거나 다 넣을 수 있다. 하지만 읽거나 쓰려면 반드시 특정 데이터로 형변환 해야 한다.   
```c
#include <stdio.h>

int main(void) {
	int a = 11;
	void* pa = &a; // 주소를 담겠다.

	printf("*pa= %p\r\n", (uintptr_t *)pa); // 출력하려면(=읽으려면) 형변환 해야 한다.

	return 0;
}
```   
      
✅ 특별히 문제가 없다면...   
주소를 int*, unsigned int* 라고 생각해도 무방하다.   
   
## int*는 32비트일까? 64비트일까?

✅ 플랫폼에 따라 다르다   
<img src="https://github.com/user-attachments/assets/af7525a6-c2cb-4341-9e8f-a23e30552a36" width="100" height="100">   
하지만 윈도우상 visual studio에서 보이는건 64비트였다.   
   
## swap 함수: call by value vs call by reference

✅ 값에 의한 호출 (call by value)   
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

void swap(int a, int b) {
	printf("in swap() before (a,b)=(%d,%d)\r\n", a, b);
	int tmp = a;
	a = b;
	b = tmp;
	printf("in swap() after  (a,b)=(%d,%d)\r\n", a, b);
}

int main(void) {
	int a = 11;
	int b = 22;

	printf("in main() before (a,b)=(%d,%d)\r\n", a, b);
	swap(a, b);
	printf("in main() after  (a,b)=(%d,%d)\r\n", a, b);

	return 0;
}
```   
   
✅ 참조에 의한 호출 (call by reference)   
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

void swap(int* a, int* b) {
	printf("in swap() before (a,b)=(%d,%d)\r\n", *a, *b);
	int tmp = *a;
	*a = *b;
	*b = tmp;
	printf("in swap() after  (a,b)=(%d,%d)\r\n", *a, *b);
}

int main(void) {
	int a = 11;
	int b = 22;

	printf("in main() before (a,b)=(%d,%d)\r\n", a, b);
	swap(&a, &b);
	printf("in main() after  (a,b)=(%d,%d)\r\n", a, b);

	return 0;
}
```

## 배열 포인터

✅ 배열 포인터란?   
배열 포인터라고 흔히들 말하는데, Pointer to an Array 표현이 더 정확하다.   
```c
int arr[4]= {11, 22, 33, 44};

int* parr2= arr; // (1)
int* parr1= &arr[0]; // (2) C린이는 가급적 (2)와 같이 쓰자.
```

```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>

int main() {

	int arr[] = { 11,22,33,44 };
	const int size = sizeof(arr) / sizeof(arr[0]);
	const int* parr = arr; // arr을 가리키는 배열 포인터, *(parr+4) 부터는 쓰레기값

	for (int i = 0; i < size;i++) {
		printf("[%d] %d\r\n", i + 1, *(parr + i)); // 배열 포인터 연산
	}
	printf("\r\n");

	for (int i = 0; i < size;i++) {
		printf("[%d] %d\r\n", i + 1, parr[i]); // 배열 포인터 연산
	}
	printf("\r\n");

	return (0);
}
```

## 함수의 인자로 배열을 받아보자

✅ 인자로 배열 및 포인터로 받아보자   
```c
#include <stdio.h>

int sum_of_arr_by_array(const int arr[], int size) {
	int sum = 0;
	for (int i = 0; i < size; i++) {
		sum += arr[i];
	}

	return sum;
}

int sum_of_arr_by_pointer(const int* arr, int size) {
	int sum = 0;
	for (int i = 0; i < size; i++) {
		//sum += *(arr + i);
		sum += arr[i];
	}
	return sum;
}

int main() {
	int arr1[3] = { 1,2,3, };
	int sum1 = 0;
	sum1 = sum_of_arr_by_array(arr1, 3);
	printf("배열 arr1의 합은 %d 입니다.\r\n", sum1);

	int arr2[3] = { 3,3,4, };
	int sum2 = 0;
	sum2 = sum_of_arr_by_pointer(arr2, (sizeof(arr2)/sizeof(arr2[0])) );
	printf("배열 arr2의 합은 %d 입니다.\r\n", sum2);

	return 0;
}
```
   
✅ 결론   
인자로 배열이라 적어도 배열 포인터 형태로 받는다! 즉, 포인터로 받으나 배열로 받으나 똑같다는 말이다. 배열로 받는다 해도 컴파일러는 포인터로 바꾸어 버린다. 포인터를 보낼때는 사이즈도 같이 보내야 한다.   
   
✅ [퀴즈] 포인터로 넘기는데, 사이즈를 안보내도 되는 경우가 있다? 문자열

## 다차원 배열을 인자로 받기

★ 에는 무엇이 들어가야 하나?   
```c
// 1차 부분배열의 포인터를 적으면 된다. 즉, 제일 바깥에 있는걸 넘기면 된다. 
#include <stdio.h>

int func(★ arr) {

}

int main() {
	int arr[2][3] = {
		{11, 22, 33},
		{44, 55, 66},
	};
	func(arr);
}
```
정답은 int (*arr)[3]

```c
  int a = 1;
  int b = 2;
  int c = 3;

  int* pa = &a;
  int* pb = &b;
  int* pc = &c;

  int* arr[3] = {pa, pb, pc};
```

```c
  int arr3[4][3] = {
    {1,2,3},
    {4,5,6},
    {7,8,9},
    {1,2,3},
};
// 얘를 가리키는 배열 포인터는 int* (arr)[3] 이다.

  int arr4[2][3][2];
// 얘를 가리키는 배열 포인터는 int* (arr)[3][2] 이다.

  int arr5[3][2][4][8][7];
// 애를 가리키는 배열 포인터는 int* (arr)[2][4][8][7] 이다.
```

## 보이드 포인터 활용: 정렬
✅ 여러 데이터형(int, char, double)을 정렬하려면?   
```c
void sort(int* arr, int count);
```   
   
```c
void sort_by_int(int* arr, int count);
void sort_by_char(char* arr, int count);
void sort_by_float(float* arr, int count);
```   
   
함수를 1개로 줄일수는 없을까?   
   
✅ C에서 해결책: void* (+형 정보도 같이 보내야)   
```c
#define INT_TYPE 0
#define CHAR_TYPE 1
#define LONG_TYPE 2

void sort(void* arr, int count, int type);
```   
하지만 type 값에 define되지 않은 값도 잘못 들어오게 되면 위험하기 때문에 아래의 방식을 더 많이 사용한다.   
   
```c
typedef enum _type_t {
	char_type,
	int_type,
	long_type,
} type_t;

void sort(void* arr, int count, type_t type);
```   
   
✅ 그렇담 현재 상태에서 가장 좋은건   
```c
typedef enum _type_t {
	char_type,
	int_type,
	float_type,
	double_type
} type_t;

void sort(void* arr, int count, type_t type) {
	switch (type) {
	case char_type:
		// void*를 char*로 형변환
		break;
	case int_type:
		// void*를 int*로 형변환
		break;
	case float_type:
		//
		break;
	case double_type:
		//
		break;
	}
}
```   

## 당구선수 정렬
✅ player.h   
```c
#ifndef __PLAYER_H__
#define __PLAYER_H__

#define MAX_PLAYER (10)
#define woman 0
#define man 1
#include <stdio.h>
#include <stdint.h>
#include <stdbool.h>

typedef struct _player_t {
    uint8_t rank;   // 랭킹 순위
    char name[64];  // 이름
    bool gender;    // 성별
    double avg;     // 에버리지
    uint8_t hr;     // 하이런 갯수
    uint8_t  bsp;   // 뱅크샷 갯수
    double bsp_rate;// 뱅크샷 성공 비율(%)
} player_t;

void print_all_player(char* title, player_t* player);
void print_player(player_t* player);
#endif
```
   
✅ player.c   
```c
#include "player.h"

void print_player(player_t* player) {
    printf("%3d ", player->rank);
    printf("%12s ", player->name);
    switch (player->gender) {
    case woman:
        printf(" woman ");
        break;
    case man:
        printf("   man ");
        break;
    }
    printf("%6.3f ", player->avg);
    printf("%2d ", player->hr);
    printf("%3d/", player->bsp);
    printf("%5.2f(%%)", player->bsp_rate);
    printf("\r\n");
}

void print_line() {
    printf("--------------------------------------------------\r\n");
}

void print_all_player(char* title, player_t* player) {
    printf("               %s\r\n", title);
    print_line();
    for (int i = 0; i < MAX_PLAYER; i++) {
        print_player(&player[i]);
    }
    print_line();
}
```
   
✅ main.c   
```c
#define _CRT_SECURE_NO_WARNINGS
#include <stdio.h>
#include "player.h"

void sort_by_rank(player_t* dst, const player_t* src, int cnt) {
    player_t temp;
    memcpy(dst, src, sizeof(player_t) * cnt);

    for (int i = 0; i < cnt - 1; i++) {
        for (int j = 0; j < cnt - 1 - i; j++) {
            if (dst[j].rank > dst[j + 1].rank) {
                temp = dst[j];
                dst[j] = dst[j + 1];
                dst[j + 1] = temp;
            }
        }
    }
}

int main() {

    player_t players[MAX_PLAYER] = {
        { 1, "김가영",     woman, 1.024, 9, 60, 23.810 },
        { 2, "김세연",     woman, 1.010, 7, 26, 26.530 },
        { 4, "김보미",     woman, 0.979, 6, 46, 24.210 },
        { 3, "스롱 피아비",woman, 0.983, 7, 54, 22.880 },
        { 5, "김영민",     woman, 0.915, 8, 36, 27.270 },
        { 7, "백주민",     woman, 0.830, 8, 50, 37.310 },
        { 6, "임정숙",     woman, 0.906, 5, 28, 35.900 },
        { 8, "오정수",     woman, 0.819, 6, 22, 32.350 },
        { 9, "임공진",     woman, 0.819, 5, 18, 23.380 },
        { 10, "오정수",    woman, 0.815, 8, 30, 30.610 },
    };

    print_all_player("랭킹별 당구 선수", players);

    player_t sorted_players[MAX_PLAYER];
    sort_by_rank(sorted_players, players, MAX_PLAYER);
    print_all_player("랭킹별 당구 선수", sorted_players);

    return (0);
}
```
