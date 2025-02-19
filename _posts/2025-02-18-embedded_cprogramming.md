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
   
✅ 메모리 리매핑   

## 컴퓨터 구조
✅ 버스
## ARM 임베디드 시스템의 메모리 구조



## TC275 보드의 메모리 구조

# TC275 부트시퀀스
## ARM에서의 build, linker script
✅ TC275 프로젝트 빌드 결과물   
1. Makefile   
2. .o 파일들   
3. .hex, elf 파일들
4.  

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

✅ 상수 폴딩
```c
0x1 << 2 같은 경우는 컴파일러에서 바로 0x4로 넣어준다. "너도 알고 나도 알아." 이런건 코드에서 0x4로 한다해도 실행 속도엔 변화가 없으므로 최적화가 아니다.
이를 상수 폴딩이라한다.
```

