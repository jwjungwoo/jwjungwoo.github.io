---
layout: single
title:  "미래 모빌리티 트렌드"
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
