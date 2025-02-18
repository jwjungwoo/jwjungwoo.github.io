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
<img src="KakaoTalk_20250218_163143211](https://github.com/user-attachments/assets/052b91ed-588c-4ff0-945a-215e23a66fda" width="500" height="360">   
<img src="KakaoTalk_20250218_163528110](https://github.com/user-attachments/assets/0e29dae7-f9bc-470e-9f6b-19d5e1b350ca" width="500" height="330">   


## 컴퓨터 구조

## ARM 임베디드 시스템의 메모리 구조



## TC275 보드의 메모리 구조

# TC275 부트시퀀스
## ARM에서의 build, linker script
✅ TC275 프로젝트 빌드 결과물   
1. Makefile   
2. .o 파일들   
3. .hex, elf 파일들
4.  
