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
# 느낀점

✅ 회로: 회로를 어느정도 알아야한다는 강사님의 말씀이 인상 깊었다. 난 회로를 보면 눈 감아. 가 되면 안 된다고하셨다.   
✅ 어려워: ShieldBuddy 메뉴얼 표를 찾아가며 프로그래밍하는 것이 처음이라 다소 어려웠다. 첫날엔 7시까지 남아서 공부를 하다 집에 와서 더 했다. 복습하며 새로운 것을 소화해서 뿌듯했다.   
✅ 팀워크: 팀원들과 모르는 부분을 서로 채워주어 이해하기 수월했다. 팀워크는 중요하고 소중하다는 것을 가장 많이 느꼈다...(매번 느끼는 중) 우리는 나보다 똑똑하다!!!   

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
   
이 네 가지를 배울 것이다.   

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
<img src="https://github.com/user-attachments/assets/e9ecec48-7570-4573-8432-b4ffac800029" width="400" height="60">   
   
✅ 스위치   
회로 on/off   
   
✅ 그라운드   
그라운드는 전자를 주는 놈. 전자는 음극에서 양극으로 간다. VCC, GND는 한계가 없다.   
전지로 돼있으면 (전해질 등의)한계가 있는데, 어댑터로 할 때는 한계가 없다 보면된다.   
전류: 전자의 흐름 (전류가 +에서 -로 흐른다를 약속한거고 실제 전자는 -에서 +로흐르는거임)   
가변저항: 
사인파의 진동을 만들어 놓고 변형을 하면 된다.   
   
✅ Pull-up, Pull-down   
아무것도 안 했을 때 높은 전압이 읽히냐 낮은 전압이 읽히냐가 pull-up, pull-down의 의미다.

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
```
   
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

## ShieldBuddy 확인법
1. ShieldBuddy p.44를 확인하여 Digital Pin 번호의 TC275T Pin Assignment를 확인한다.
2. 

# 실습
## 파란색 LED 점등 실습
빌드는 맨 처음에 해주고, 디버그 중인 코드들을 멈추고, 디버그하고, resume을 하면 된다.   
initLED() 함수가 존재하는 이유는 GPIO 핀을 출력 모드로 설정한 후에야 P10_OUT을 통해 LED를 제어할 수 있기 때문이다.   
```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#define PCn_2_IDX 19
#define P2_IDX 2

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initLED(void); //가독성 증가

int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);

    initLED();

    while(1) {
        P10_OUT.U = 0x1 << P2_IDX;
    }
    return(1);
}

void initLED(void) {
    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}
```

## P02.1을 읽어와서(D3스위치) 눌렀을 때 D13 LED가 켜지는 실습
1. P02_IOCR0의 15~11bit를 00010으로 setting
2. P02_IN인지 확인하기

```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initLED(void); //가독성 증가

int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);

    initLED();

    while(1) {
        if( (P02_IN.U &= (0x1 << P1_IDX)) == 0) {  // 누르면 0
            P10_OUT.U = 0x1 << P2_IDX;  
        }
        else {
            P10_OUT.U = 0x0 << P2_IDX;
        }
    }
    return(1);
}

void initLED(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}
```

## 안눌파, 눌빠
```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1       //D3스위치는 P2.1이다. 1번이므로 P1_IDX = 1이다.

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initLED(void); //가독성 증가

int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);

    initLED();

    while(1) {
        if( (P02_IN.U &= (0x1 << P1_IDX)) == 0) {
            P10_OUT.U = 0x1 << P1_IDX;
        }
        else {
            P10_OUT.U = 0x1 << P2_IDX;
        }
    }
    return(1);
}

void initLED(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);//
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;   //P02는 00010일때가 켜져있는 것이다.

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;   //P10.2를 output으로 설정 블루

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;   //P10.1를 output으로 설정 레드
}
```

## 위의 실습을 OMR 레지스터로 구현하기
```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1       // D3 스위치는 P2.1이다. 1번이므로 P1_IDX = 1이다.

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initLED(void); // 가독성 증가

int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);

    initLED();

    while(1) {
        if( (P02_IN.U & (0x1 << P1_IDX)) == 0) {  // 버튼이 눌리면
            P10_OMR.U = (1 << P1_IDX) | (1 << (P2_IDX + 16)); // P10.1(레드 LED ON), P10.2(블루 LED OFF). OMR 레지스터의 하위 16비트는 PCL(clear)비트이기 때문이다.
        }
        else {  // 버튼이 눌리지 않으면
            P10_OMR.U = (1 << P2_IDX) | (1 << (P1_IDX + 16)); // P10.2(블루 LED ON), P10.1(레드 LED OFF)
        }
    }
    return(1);
}

void initLED(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;   // P02는 00010일 때 켜짐

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;   // P10.2를 output으로 설정 (블루 LED)

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;   // P10.1을 output으로 설정 (레드 LED)
}
```
