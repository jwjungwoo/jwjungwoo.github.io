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

## int

int는 꼭 32bit가 아닐 수 있음. 환경에 따라 다름. 혹여나 데이터 전송 시 깨지더라도 당황하지 말자. 

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

## 엔디안

컴구시간에 배웠었다. 이름이 인디안 같아서 재밌어서 집중 잘 됐던 기억이 있다. (여담이지만 필자는 컴구 수강생 100명이 넘는 시험에서 8등했었다.)   
   
주소가 나올 때   
00 00 00 0B (왜 이렇게 안나오냐)   
0B 00 00 00 (왜 이렇게 나오는가) : 리틀엔디안   
정답: 엔디안 때문   

빅 엔디안 vs 리틀 엔디안 (저장하는 차이); 우리가 앞으로 쓸 '암'에선 둘 다 지원하지만 통상 리틀을 사용함. (내부적으로 일어나는 작용이더라도
임베디드 개발자는 알아야함. 왜냐하면 시스템이 달라질 때 문제가 되기 때문임. 예를들어 윈도우에서 맥으로 데이터를 보낼 때 깨질 수 있음. 모르는 당혹스러울 수 있음)   

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

## 

