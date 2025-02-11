---
layout: single
title:  "임베디드 MCU 프로그래밍"
categories: autoever
tag:
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "autoever"
#search: false
---

# 임베디드 시스템

## 임베디드 시스템
✅ 임베디드 시스템   
마이크로 컨트롤러가 있으면 임베디드 시스템이라고 생각하면된다. 컴퓨터와 소자 그 중간 어딘가.   
   
✅ 마이크로프로세서(Microprocessor): 연산용   
PC의 CPU   
고속연산과 대용량 메모리 처리   
인텔 펜티엄   
MPU(Micro Processor Unit)   
   
✅ 마이크로컨트롤러(Microcontroller)   
기계나 장치를 제어하기 위해 사용하는 프로그램이 가능한 칩(chip)   
CPU, 기본 메모리, I/O 포트, 타이머, 직렬통신 등의 기능을 하나의 칩에 내장   
8051, AVR 등   
MCU(Micro Control Unit)   
   
✅ 디버거   
보드 안에 디버거가 있냐 없냐에 따라 가격이 달라진다. 우리가 쓸 보드는 디버거가 내재돼있다.   
   
✅ 중요 부분   
1.(DI/DO) GPIO(-> IDE, Interface, Editor). Digital In, Digital Out: 입력으로도 쓸 수 있고 출력으로도 쓸 수 있는 범용적인 핀   
2. Preemptive Shield   
3.(AI) ADC. Analog In   
4.(유사AO) PWM. Analog Out   
   
이 네가지를 배울 것이다.   

## 임베디드 개발을 위한 회로도  
임베디드 개발자는 회로의 모든것을 다 알필요는 없다!   
   
회로도를 보고 내가 제어할 부품이 무엇인지? 어떻게 제어해야 할지   
회로도 상의 부품을 실제 보드에서 찾을 수 있어야 함   
어디가 전원이고, 어디가 그라운드인지 찾을 수 있어야 함   
하드웨어 디버깅 시, 어디에 멀티미터/오실로 스코프를 연결해야할 지 알아야함 (어떤값이 정상값인지? 원인까지는 모르더라도, 문제가 있다 까지는 알아야함)   
   
✅ 저항   
저항을 키우면 전류가 줄어든다.   
   
✅ 다이오드   
전압(or전류) 방향 안정화   
양극이 음극보다 0.7V 이상 커지면 전류가 흐른다.   
<img src="ㅗttps://github.com/user-attachments/assets/e9ecec48-7570-4573-8432-b4ffac800029" width="400" height="60">   
   
✅ 스위치   
회로 on/off   
   
✅ 그라운드   
그라운드는 전자를 주는 놈. 전자는 음극에서 양극으로 간다. VCC, GND는 한계가 없다.   
전지로 돼있으면 (전해질 등의)한계가 있는데, 어댑터로 할 때는 한계가 없다 보면된다.   
전류: 전자의 흐름 (전류가 +에서 -로 흐른다를 약속한거고 실제 전자는 -에서 +로흐르는거임)   
가변저항: 
사인파의 진동을 만들어 놓고 변형을 하면 된다.   

## 실습물품
✅ TC275 보드   
<img src="https://github.com/user-attachments/assets/ca5d9d36-1924-407d-a91f-edc3b2efe06f" width="400" height="600">   
빨간색 보드가 TC275보드이다. TC275보드의 정중앙의 정사각형 보드가 바로 마이크로컨트롤러이다. 테두리엔 확장보드를 끼울 수 있다.

## infineon (GPIO)
<img src="https://github.com/user-attachments/assets/d6ce9181-0bcd-4262-9076-0f697e440816" width="600" height="350">   
위의 경로로 프로젝트를 생성한다. 그리고 빌드를 하고 디버그를 해야한다. 빌드를 먼저하면 elf 파일이 생긴다.   
   
✅ GPIO를 포함한 기능구현 단계   
1. 회로파악 (스위치를 누를때 1인지 혹은 0인지를 파악)   
2. 핀/레지스터 설정 파악 (up의 어떤 핀을 어떻게 설정해야 하는지)   
3. 코드구현   
   
✅ 핀 형식   
P10.2   
(GPIO)(GROUP).(PIN#)   
   
1로 설정하면 출력, 0으로 하면 입력   
   
✅ P10_IOCR0 23~19bit를 10000으로 세팅   
<img src="https://github.com/user-attachments/assets/0c4112d8-9224-4424-91be-e1f54e1bc4a4" width="400" height="460">   
1. 23~19bit을 초기화   
```c
P10_IOCR0&=~(0x1f<<19)
0~0111110~0
1~1000001~1
```   
2. 23~19bit를 10000으로 세팅   
```c
0x10 == 0b10000
P10_IOCR0 |= (0x10)<<19
```   `
   
✅ 레지스터   
Register는 unsigned int로 표현한다. Register 뒤에 .U를 붙이면 unsigned int로 접근할 수 있다. Register를 초기화한 후에 0x10으로 세팅한다.   
   
✅ P10.2에 출력값 설정 코드   
Output register를 1로 설정하면 됨   
```c
#define P2_IDX 2

while(1) {
  P10_OUT.U |= 0x1 << P2_IDX; // 이런식으로 led를 제어할 수 있다. 
}
```   

