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
	unsigned char tmp = 0x80;
	for (int i = 0; i < 8;i++) {
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
C장점: 하드웨어 제어에 좋은 언어   
C단점: 1.하드웨어 제어에 좋음(오류뜨면 그냥 컴퓨터 멈춤. 오류 메시지도 안 보여줌) 2.대규모 프로젝트엔 적합하진 않음 3. 객체 동적할당 후 해제해줘야함 (segment fault: 잘못된 메모리에 접근하는 오류. 많이뜸)   

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
   
# 자료형

## 무슨 자료형
변수 이름만 지우면 그 변수의 자료형이 나온다.(강사님께서 열변을 토하심. 화둔 쓰시는 줄베디드는 memory의 주소로 직접가서 값을 넣을 수 있음.   
```c
 *(volatile unsigned int *)0xABCD = 0x1234; // 주소 0xABCD에 0x1234를 넣는단 뜻.
```
C장점: 하드웨어 제어에 좋은 언어   
C단점: 1.하드웨어 제어에 좋음(오류뜨면 그냥 컴퓨터 멈춤. 오류 메시지도 안 보여줌) 2.대규모 프로젝트엔 적합하진 않음 3. 객체 동적할당 후 해제해줘야함 (segment fault: 잘못된 메모리에 접근하는 오류. 많이뜸)   

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
   
# I/O functions

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

sprintf는 어디 네트워크로 보낼때 해당 문자열로 보내야하기때문에 종종 사용한다하셨다. (알고있어야한다셨음) 주로 문자열을 형식화(formatting) 할 때 사용한다.   
printf: 형식화된 문자열을 화면에 출력.   
sprintf: 형식화된 문자열을 변수에 저장.   

# 자료형

## 크기

```c
sizeof(int) = 4
sizeof(char) = 1
sizeof(float) = 4
sizeof(double) = 8
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

## 무슨 자료형

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

## literal, symbolic

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

## float과 double

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
   
# 비트 연산자

<img src="https://github.com/user-attachments/assets/9f55bfb8-fda4-4fbe-be36-f227bf5a9df8" width="600" height="400">   

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

## 종류

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
프로세서가 메모리에 저장할 때 하위비트를 기준으로 저장할건지, 상위 비트를 기준으로 저장할건지가 결정된다. 어떻게 저장하느냐에 따라 빅 엔디안, 리틀엔디안으로 나뉜다.   

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
   
1) ctrl + f5말고 f5를 누른다   
2) 좌측 하단에 조사식 클릭, 검색에 x, &x 등을 찾아볼 수 있음   
<img src="https://github.com/user-attachments/assets/16eb7df1-91dd-44e2-9f9d-70d906fa40dc" width="400" height="400">   
3) &x를 하고 메모장에 복풋하고 앞에 있는 주소만 복사   
<img src="https://github.com/user-attachments/assets/0a38af6d-0abc-4d70-ab66-5c3c3d06c84d" width="400" height="400">   
4) 우측 상단에서  디버그->창->메모리 클릭하고 치면 됨   
<img src="https://github.com/user-attachments/assets/9f025dbd-5690-44c8-98a7-8e827f1d95cd" width="400" height="400">   
5) 1이 01 00 00 00   
   
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

✅[복습] 십진수 35247이 메모리에 어떻게 저장이 되나?   
십육진수 89 AF   
00 00 00 00 00 00 89 AF <- 이렇게 저장되지 않고   
AF 89 00 00 <- 리틀 엔디안 형태로 저장   
   
✅1은 어떻게 저장되나?   
- -1을 구하는 방법   
    - 1을 이진수로 바꾸자!   
    - 1 = 0001   
- 0은 1로, 1은 0으로 바꾸자   
    - 0001 → 1110   
- 여기에 +1하자   
    - 1110 → 1111   
  
✅3은 어떻게 저장되나?   
- -3을 구하는 방법   
    - 3을 이진수로 바꾸자!   
    - 3 = 0011   
- 0은 1로, 1은 0으로 바꾸자   
    - 0011 → 1100   
- 여기에 +1하자   
    - 1100 → 1101   
   
✅1이 어떻게 저장되나?   
(1) 음수를 양수로 바꾼후 32비트에 대입   
-1 -> +1 -> 0000 0000 0000 0001   
(2) 1->0, 0->1   
(앞에 생략) 1111 1111 1111 1110   
(3) 바뀐값에 +1   
(앞에 생략) 1111 1111 1111 1111   
   
✅231 이 어떻게 저장되나?   
(1)   
-231 ->231 -> E7 -> 1110 0111   
(앞에 생략) 0000 0000 1110 0111   
(2)   
(앞에 생략) 1111 1111 0001 1000   
(3) 바뀐값에 +1   
(앞에 생략) 1111 1111 0001 1001   
(앞에 생략) FF FF FF 19   
   
## 보수법
