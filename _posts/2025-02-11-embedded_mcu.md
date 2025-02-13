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
✅ 팀워크: 팀원들과 모르는 부분을 서로 채워주어 이해하기 수월했다. 팀워크는 중요하고 소중하다는 것을 가장 많이 느꼈다...(매번 느끼는 중) 우리는 나보다 똑똑하다.   

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
            P10_OMR.U = (1 << P1_IDX) | (1 << (P2_IDX + 16)); // P10.1(레드 LED ON), P10.2(블루 LED OFF). OMR 레지스터의 하위 16비트는 PCL(clear)비트이기 때문이다. // P10_OMR.U = 0x40002;
        }
        else {  // 버튼이 눌리지 않으면
            P10_OMR.U = (1 << P2_IDX) | (1 << (P1_IDX + 16)); // P10.2(블루 LED ON), P10.1(레드 LED OFF) // P10_OMR = 0x20004;
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

## 스위치 눌렀을 때 상태 토글
스위치를 눌렀을 때 빨간 LED의 상태가 바뀌도록 (on->off, off->on) OMR 레지스터를 활용해서 작성해보세요.

### 이전 상태 현재 상태 저장
✅ 이걸 가장 많이 선호   
```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#define PCn_2_IDX 19
#define P2_IDX 2

#define PCn_1_IDX 11
#define P1_IDX 1

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initLED(void); // 가독성 증가

int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);

    initLED();
    unsigned char preSW = 1;
    unsigned char curSW;
    while(1) { // 1줄에 5클락,  대략 6줄 -> 30클락, 1 클락 200Mhz에 5nano초
        curSW = P02_IN.U & (0x1 << P1_IDX); // 눌리면 P02_IN.U가 0
        if (preSW && !curSW) {
            P10_OMR.U = 0x20002;
        }
        preSW = curSW;
    }
    return(1);
}

void initLED(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}
```

### 현재 0이 되었으면, 1이 될때까지 무한 loop
✅ 데드락의 위험   
```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#define PCn_2_IDX 19
#define P2_IDX 2

#define PCn_1_IDX 11
#define P1_IDX 1

IfxCpu_syncEvent g_cpuSyncEvent = 0;
int flag = 0;
void initLED(void); // 가독성 증가

int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);

    initLED();

    while(1) { // 1줄에 5클락,  대략 6줄 -> 30클락, 1 클락 200Mhz에 5nano초
        flag = 0;
        while((P02_IN.U &= (0x1 << P1_IDX)) == 0)
            if (flag == 0)
                flag = 1;

        if (flag == 1)
            P10_OMR.U = (1 << P1_IDX) | (1 <<(P1_IDX + 16));
    }
    return(1);
}

void initLED(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}
```

### cnt 값이 thr 이상
✅ 데드락 위험   
```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#define PCn_2_IDX 19
#define P2_IDX 2

#define PCn_1_IDX 11
#define P1_IDX 1

IfxCpu_syncEvent g_cpuSyncEvent = 0;
int cnt = 0;
void initLED(void); // 가독성 증가

int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);

    initLED();

    while(1) { // 1줄에 5클락,  대략 6줄 -> 30클락, 1 클락 200Mhz에 5nano초
        cnt = 0; 
        while((P02_IN.U &= (0x1 << P1_IDX)) == 0) {
            cnt++;
        }
        if (cnt > 1000) {
            P10_OMR.U = (1 << P1_IDX) | (1 <<(P1_IDX + 16));
        }
    }
    return(1);
}

void initLED(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}
```

# 외부 인터럽트
## 기본 지식
✅ Datasheet: HW적인 정의   
✅ UM, RM: 기능(SW)적인 정의   
   
✅ 폴링 방식: 우선 순위가 없는 순차적 처리 방식   
✅ 인터럽트 방식: 우선 순위가 높은 코드를 먼저 처리하는 방식   
HW 인터럽트: 하드웨어에 의해 발생   
SW 인터럽트: 운영체재의 커널에 의해 발생   
   
✅ ISR: Interrupt Service Routine   

## ISR 호출 및 실행
✅ ISR 호출을 위한 3가지 조건   
전역적인 인터럽트 활성화 비트 세트   
인터럽트 별 인터럽트 활성화 비트 세트   
인터럽트 발생 조건 충족   
   
✅ 인터럽트 처리 순서   
PC: 다음 실행 위치를 가르켜줌   
ex) 2번 인터럽트 발생   
1. Program Counter(PC)를 인터럽트 벡터 테이블의 2번 위치로 이동   
2. 해당 ISR의 위치를 이동   
3. ISR 실행 (인터럽트 처리)   
4. 인터럽트 발생 전 위치로 복귀   

## 인터럽트 사용 시 주의사항
✅ 중첩된 인터럽트: 가능한 짧게   
✅ 인터럽트 우선순위: User Manual에 큰 번호 or 작은 번호 순이라고 나와있음   
✅ 최적화 방지: ISR은 일반 함수와 달리 코드 내에서 명시적으로 호출하지 않음. 따라서 volatile 사용   

## ERU
✅ ERU: External Request Unit
IR(Interrupt Router): external request unit, internal request unit으로 구성   

## TS275 외부 인터럽트 사용
✅ External Request Selection (ERS) Unit   
Input channel에 연결된 4개의 pin 중 하나를 선택   
Event Trigger Logic(ETL) Unit: 언제 이벤트 트리거를 발생시킬 것인가. input edge, rising edge selection, falling edge selection 등이 있다.   

# 실습
## 누를 때 파란불 키기
```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1

// ERU related
#define EXISO_IDX 4 // 감지할 입력 신호 선택
#define FENO_IDX 8 // 하강 엣지 감지 설정
#define RENO_IDX 9 // 상승 엣지 감지 설정
#define EIENO_IDX 11 
#define INP0_IDX 12  // 인터럽트 요청 라인 연결
#define IGP0_IDX 14

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initGPIO(void);
void initERU(void);

IFX_INTERRUPT(ISR0,0,0x10); // 0x10이 발생했을 때 ISR0를 실행한다.
void ISR0(void) {
    P10_OUT.U = 0x1 << P2_IDX;
}

void initLED(void); // 가독성 증가

int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);

    initLED();
    initGPIO();
    initERU();
    while(1) {
    }
    return(1);
}

void initLED(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}

void initGPIO(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}

void initERU (void) {
    //set EICR: 외부 신호(버튼, 센서 등)를 감지하여 인터럽트를 발생시키도록 설정
    SCU_EICR1.U &= ~(0x7 << EXISO_IDX);
    SCU_EICR1.U |= 0x1 << EXISO_IDX; // 감지할 입력 신호 선택

    SCU_EICR1.U |= 1 << FENO_IDX; // 하강 엣지(신호가 HIGH에서 LOW로 변화할 때) 감지 활성화. 이 설정으로 신호가 하강 엣지에서 변할 때 인터럽트가 발생한다.
    // 떼는 동작은 rising egde
    SCU_EICR1.U |= 1 << EIENO_IDX;  

    SCU_EICR1.U &= ~(0x7 << INP0_IDX);

    //set IGCR: 감지된 신호가 인터럽트를 발생시키는 방식 결정
    SCU_IGCR0.U &= ~(0x3 << IGP0_IDX);
    SCU_IGCR0.U |= 0x1 << IGP0_IDX;

    //set SCUERU: 인터럽트 요청을 CPU로 전달하여 ISR 실행
    SRC_SCU_SCU_ERU0.U &= ~0xff;
    SRC_SCU_SCU_ERU0.U |= 0x10; // 서비스 private number

    SRC_SCU_SCU_ERU0.U |= 1 << SRE_IDX;
    SRC_SCU_SCU_ERU0.U &= ~(0x3 << TOS_IDX);
}
```
## 뗄 때 파란불 키기
```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1

// ERU related
#define EXISO_IDX 4 // 감지할 입력 신호 선택
#define FENO_IDX 8 // 하강 엣지 감지 설정
#define RENO_IDX 9 // 상승 엣지 감지 설정
#define EIENO_IDX 11
#define INP0_IDX 12  // 인터럽트 요청 라인 연결
#define IGP0_IDX 14

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initGPIO(void);
void initERU(void);

IFX_INTERRUPT(ISR0,0,0x10); // 0x10이 발생했을 때 ISR0를 실행한다.
void ISR0(void) {
    P10_OUT.U = 0x1 << P2_IDX;
}

void initLED(void); // 가독성 증가

int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);

    initLED();
    initGPIO();
    initERU();
    while(1) {
    }
    return(1);
}

void initLED(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}

void initGPIO(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}

void initERU (void) {
    //set EICR: 외부 신호(버튼, 센서 등)를 감지하여 인터럽트를 발생시키도록 설정
    SCU_EICR1.U &= ~(0x7 << EXISO_IDX);
    SCU_EICR1.U |= 0x1 << EXISO_IDX; // 감지할 입력 신호 선택
      
    SCU_EICR1.U |= 1 << RENO_IDX; // 떼는 동작은 rising egde
    SCU_EICR1.U |= 1 << EIENO_IDX;

    SCU_EICR1.U &= ~(0x7 << INP0_IDX);

    //set IGCR: 감지된 신호가 인터럽트를 발생시키는 방식 결정
    SCU_IGCR0.U &= ~(0x3 << IGP0_IDX);
    SCU_IGCR0.U |= 0x1 << IGP0_IDX;

    //set SCUERU: 인터럽트 요청을 CPU로 전달하여 ISR 실행
    SRC_SCU_SCU_ERU0.U &= ~0xff;
    SRC_SCU_SCU_ERU0.U |= 0x10; // 서비스 private number

    SRC_SCU_SCU_ERU0.U |= 1 << SRE_IDX;
    SRC_SCU_SCU_ERU0.U &= ~(0x3 << TOS_IDX);
}
```

## SW1->파란, SW2->빨강
```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1

// ERU related
#define EXISO_IDX 4 // 감지할 입력 신호 선택
#define FENO_IDX 8 // 하강 엣지 감지 설정
#define RENO_IDX 9 // 상승 엣지 감지 설정
#define EIENO_IDX 11
#define INP0_IDX 12  // 인터럽트 요청 라인 연결
#define IGP0_IDX 14
#define IGP1_IDX 30

#define FENO_IDX1 24
#define RENO_IDX1 25
#define EIENO_IDX1 27
#define INP1_IDX 28

// SRC related
#define SRE_IDX 10 //Service Request Enable
#define TOS_IDX 11 //Type of Service Control

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initGPIO(void);
void initERU(void);

IFX_INTERRUPT(ISR0,0,0x10); // 0x10이 발생했을 때 ISR0를 실행한다.
IFX_INTERRUPT(ISR1,0,0x20); // 0x10이 발생했을 때 ISR0를 실행한다.
void ISR0(void) {
    P10_OUT.U = 0x20002;
}
void ISR1(void) {
    P10_OUT.U = 0x400004;
}
void initLED(void); // 가독성 증가

int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);

    initLED();
    initGPIO();
    initERU();
    while(1) {
    }
    return(1);
}

void initLED(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}

void initGPIO(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}

void initERU (void) {
    //EICR 설정(설정) -> OGU 설정(전달) ->  IGCR 값 설정(IOUT으로 전달하기 위해) -> SRC_SCU_SCU_ERU 설정
    //set EICR: 외부 신호(버튼, 센서 등)를 감지하여 인터럽트를 발생시키도록 설정
    SCU_EICR1.U &= ~(0x7 << EXISO_IDX | 0x7 << (EXISO_IDX + 16)); // 첫번째 레지스터에서 사용할 곳 초기화
    SCU_EICR1.U |= 0x1 << EXISO_IDX; // 아랫버튼. 감지할 입력 신호 선택 001
    SCU_EICR1.U |= 0x2 << (EXISO_IDX + 16); //윗버튼   010

    SCU_EICR1.U |= 1 << RENO_IDX; // 하강 엣지(신호가 HIGH에서 LOW로 변화할 때) 감지 활성화. 이 설정으로 신호가 하강 엣지에서 변할 때 인터럽트가 발생한다.
    SCU_EICR1.U |= 1 << EIENO_IDX;
    SCU_EICR1.U |= 1 << RENO_IDX1; // 사용하는 회로가 눌렸을때 1->0으로 변화하므로 falling edge로 하면 된다.
    SCU_EICR1.U |= 1 << EIENO_IDX1;

    SCU_EICR1.U &= ~(0x7 << INP0_IDX); //여기서부터 OGU
    SCU_EICR1.U &= ~(0x7 << INP1_IDX);  //OGU1 사용하려고 초기화    OGU: ERU 결과를 특정 핀(GPIO)이나 PWM 등의 모듈에 전달
    SCU_EICR1.U |= 0x1 << INP1_IDX;     //OGU1 사용하려고 값 대입

    //set IGCR: 감지된 신호가 인터럽트를 발생시키는 방식 결정
    SCU_IGCR0.U &= ~(0x3 << IGP0_IDX);   //IGP0는 IDX 14부터 시작한다.
    SCU_IGCR0.U |= 0x1 << IGP0_IDX;      //1로 값을 설정 (IOUT)
    SCU_IGCR0.U &= ~(0x3 << IGP1_IDX);
    SCU_IGCR0.U |= 0x1 << IGP1_IDX;

    //set SCUERU: 인터럽트 요청을 CPU로 전달하여 ISR 실행
    SRC_SCU_SCU_ERU0.U &= ~0xff;
    SRC_SCU_SCU_ERU0.U |= 0x10; // 서비스 private number

    SRC_SCU_SCU_ERU0.U |= 1 << SRE_IDX;
    SRC_SCU_SCU_ERU0.U &= ~(0x3 << TOS_IDX);

    SRC_SCU_SCU_ERU1.U &= ~0xff;
    SRC_SCU_SCU_ERU1.U |= 0x20; // 서비스 private number

    SRC_SCU_SCU_ERU1.U |= 1 << SRE_IDX;
    SRC_SCU_SCU_ERU1.U &= ~(0x3 << TOS_IDX);
}
```

## 누르면 정지, 다시 누르면 토글
```c
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1

// ERU related
#define EXISO_IDX 4 // 감지할 입력 신호 선택
#define FENO_IDX 8 // 하강 엣지 감지 설정
#define RENO_IDX 9 // 상승 엣지 감지 설정
#define EIENO_IDX 11
#define INP0_IDX 12  // 인터럽트 요청 라인 연결
#define IGP0_IDX 14
#define IGP1_IDX 30

#define FENO_IDX1 24
#define RENO_IDX1 25
#define EIENO_IDX1 27
#define INP1_IDX 28

// SRC related
#define SRE_IDX 10 //Service Request Enable
#define TOS_IDX 11 //Type of Service Control

IfxCpu_syncEvent g_cpuSyncEvent = 0;

#define BLINK 1
#define STOP 0

void initGPIO(void);
void initERU(void);

IFX_INTERRUPT(ISR0,0,0x10); // 0x10이 발생했을 때 ISR0를 실행한다.
IFX_INTERRUPT(ISR1,0,0x20); // 0x10이 발생했을 때 ISR0를 실행한다.

int state = STOP;
void ISR0(void) {
    switch(state) {
        case STOP:
            state = BLINK;
            break;
        case BLINK:
            state = STOP;
            break;
        default:
            break;
    }
}
void ISR1(void) {
    switch(state) {
        case STOP:
            state = BLINK;
            break;
        case BLINK:
            state = STOP;
            break;
        default:
            break;
    }
}
void initLED(void); // 가독성 증가

int flag1 = 0;
int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);

    initLED();
    initGPIO();
    initERU();
    P10_OUT.U = 0x1 << 1;
    while(1) {
        switch(state) {
            case BLINK:
                P10_OMR.U = 0x60006;
                for(int i =0; i< 10000000;i++);
                break;
            case STOP:
            default:
                break;
        }
    }
    return(1);
}

void initLED(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}

void initGPIO(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}

void initERU (void) {
    //EICR 설정(설정) -> OGU 설정(전달) ->  IGCR 값 설정(IOUT으로 전달하기 위해) -> SRC_SCU_SCU_ERU 설정
    //set EICR: 외부 신호(버튼, 센서 등)를 감지하여 인터럽트를 발생시키도록 설정
    SCU_EICR1.U &= ~(0x7 << EXISO_IDX | 0x7 << (EXISO_IDX + 16)); // 첫번째 레지스터에서 사용할 곳 초기화
    SCU_EICR1.U |= 0x1 << EXISO_IDX; // 아랫버튼. 감지할 입력 신호 선택 001
    SCU_EICR1.U |= 0x2 << (EXISO_IDX + 16); //윗버튼   010

    SCU_EICR1.U |= 1 << RENO_IDX; // 하강 엣지(신호가 HIGH에서 LOW로 변화할 때) 감지 활성화. 이 설정으로 신호가 하강 엣지에서 변할 때 인터럽트가 발생한다.
    SCU_EICR1.U |= 1 << EIENO_IDX;
    SCU_EICR1.U |= 1 << RENO_IDX1; // 사용하는 회로가 눌렸을때 1->0으로 변화하므로 falling edge로 하면 된다.
    SCU_EICR1.U |= 1 << EIENO_IDX1;

    SCU_EICR1.U &= ~(0x7 << INP0_IDX); //여기서부터 OGU
    SCU_EICR1.U &= ~(0x7 << INP1_IDX);  //OGU1 사용하려고 초기화    OGU: ERU 결과를 특정 핀(GPIO)이나 PWM 등의 모듈에 전달
    SCU_EICR1.U |= 0x1 << INP1_IDX;     //OGU1 사용하려고 값 대입

    //set IGCR: 감지된 신호가 인터럽트를 발생시키는 방식 결정
    SCU_IGCR0.U &= ~(0x3 << IGP0_IDX);   //IGP0는 IDX 14부터 시작한다.
    SCU_IGCR0.U |= 0x1 << IGP0_IDX;      //1로 값을 설정 (IOUT)
    SCU_IGCR0.U &= ~(0x3 << IGP1_IDX);
    SCU_IGCR0.U |= 0x1 << IGP1_IDX;

    //set SCUERU: 인터럽트 요청을 CPU로 전달하여 ISR 실행
    SRC_SCU_SCU_ERU0.U &= ~0xff;
    SRC_SCU_SCU_ERU0.U |= 0x10; // 서비스 private number

    SRC_SCU_SCU_ERU0.U |= 1 << SRE_IDX;
    SRC_SCU_SCU_ERU0.U &= ~(0x3 << TOS_IDX);

    SRC_SCU_SCU_ERU1.U &= ~0xff;
    SRC_SCU_SCU_ERU1.U |= 0x20; // 서비스 private number

    SRC_SCU_SCU_ERU1.U |= 1 << SRE_IDX;
    SRC_SCU_SCU_ERU1.U &= ~(0x3 << TOS_IDX);
}
```

# 내부 타이머 인터럽트
## 함수
✅ 함수는 궁극적으로 타고타고 들어가면 결국 레지스터를 바꾸는 것이다.   
OUT: 전체를 바꾸는 경우   
OMR: 특정 비트를 바꾸는 경우   
## 클락
✅ Clock을 사용하여 clock의 개수를 센다. Clock은 보통 외부에서 가져온다. TC275도 외부것이 붙어있다.   
   
✅ 보드에서 정확한 clock의생성은?   
cristal oscillator에서 수행, 이 소자는 자세히 들여다보면 20MHz이다.   
   
✅ Timer 클럭 분석   
Timer를 사용하기 위한 모듈 -> UM ch.17   
전체 시스템은 200Mhz인데 STM frequency는 100Mhz이다. 전체시스템 2클락일 때 STM은 1클락이다.   
   
✅ 기능   
모듈 자체 변수: 레지스터   
모듈 config 변수: 설정   
   
✅ 전체코드(IfxStmDemo_init)   
1. config 초기화   
2. config 설정: 타이머에게 줄 정보를 설정   
3. config 적용: 레지스터 변경   

## 파란색 LED 깜빡깜빡 실습
```c
/**********************************************************************************************************************
 * \file Cpu0_Main.c
 * \copyright Copyright (C) Infineon Technologies AG 2019
 *
 * Use of this file is subject to the terms of use agreed between (i) you or the company in which ordinary course of
 * business you are acting and (ii) Infineon Technologies AG or its licensees. If and as long as no such terms of use
 * are agreed, use of this file is subject to following:
 *
 * Boost Software License - Version 1.0 - August 17th, 2003
 *
 * Permission is hereby granted, free of charge, to any person or organization obtaining a copy of the software and
 * accompanying documentation covered by this license (the "Software") to use, reproduce, display, distribute, execute,
 * and transmit the Software, and to prepare derivative works of the Software, and to permit third-parties to whom the
 * Software is furnished to do so, all subject to the following:
 *
 * The copyright notices in the Software and this entire statement, including the above license grant, this restriction
 * and the following disclaimer, must be included in all copies of the Software, in whole or in part, and all
 * derivative works of the Software, unless such copies or derivative works are solely in the form of
 * machine-executable object code generated by a source language processor.
 *
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE
 * WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT. IN NO EVENT SHALL THE
 * COPYRIGHT HOLDERS OR ANYONE DISTRIBUTING THE SOFTWARE BE LIABLE FOR ANY DAMAGES OR OTHER LIABILITY, WHETHER IN
 * CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
 * IN THE SOFTWARE.
 *********************************************************************************************************************/
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#include "IfxPort.h"
#include "IfxPort_PinMap.h"

#include "IfxStm.h"
#include "IfxCpu_Irq.h"

typedef struct
{
    Ifx_STM             *stmSfr;            /**< \brief Pointer to Stm register base */
    IfxStm_CompareConfig stmConfig;         /**< \brief Stm Configuration structure */
    volatile uint8       LedBlink;          /**< \brief LED state variable */
    volatile uint32      counter;           /**< \brief interrupt counter */
} App_Stm;

App_Stm g_Stm; /**< \brief Stm global data */

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void IfxStmDemo_init(void);

int core0_main(void)
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

    IfxStmDemo_init();

    /*P00_5    Digital Output*/
    IfxPort_setPinModeOutput(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinLow(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);

    while(1)
    {

    }
    return (1);
}

IFX_INTERRUPT(STM_Int0Handler, 0, 100);

void STM_Int0Handler(void)
{
    static int flag = 0;
    static int cnt = 0;

    IfxStm_clearCompareFlag(g_Stm.stmSfr, g_Stm.stmConfig.comparator); // compare 관련 flag 초기화
    IfxStm_increaseCompare(g_Stm.stmSfr, g_Stm.stmConfig.comparator, 100000000u); // compare 관련 register 다시 세팅 // 한번 하고나서의 간격

    cnt++;
    
    if(flag == 0)
    {
        IfxPort_setPinLow(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);
        flag = 1;
    }
    else
    {
        IfxPort_setPinHigh(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);
        flag = 0;
    }

    IfxCpu_enableInterrupts();
}

void IfxStmDemo_init(void)
{
    /* disable interrupts */
    boolean interruptState = IfxCpu_disableInterrupts(); // 인터럽트 중지

    IfxStm_enableOcdsSuspend(&MODULE_STM0);

    g_Stm.stmSfr = &MODULE_STM0;
    IfxStm_initCompareConfig(&g_Stm.stmConfig); // config를 초기화.. 초기 간격
    
    // config 설정
    // 멤버변수들 값 설정
    g_Stm.stmConfig.triggerPriority = 100u;
    g_Stm.stmConfig.typeOfService   = IfxSrc_Tos_cpu0;
    g_Stm.stmConfig.ticks           = 100000000; // 1초를 세겠다. (100M)

    // config 적용... 이 config로 시작하자!! compare를 시작하자!!
    IfxStm_initCompare(g_Stm.stmSfr, &g_Stm.stmConfig); // config의 주소를 전달한다.

    /* enable interrupts again */
    IfxCpu_restoreInterrupts(interruptState); // 인터럽트 활성화
}
```

## 신호등 만들기(state: 파, 빨, 파 toggle). switch사용 X

```c
/**********************************************************************************************************************
 * \file Cpu0_Main.c
 * \copyright Copyright (C) Infineon Technologies AG 2019
 * 
 * Use of this file is subject to the terms of use agreed between (i) you or the company in which ordinary course of 
 * business you are acting and (ii) Infineon Technologies AG or its licensees. If and as long as no such terms of use
 * are agreed, use of this file is subject to following:
 * 
 * Boost Software License - Version 1.0 - August 17th, 2003
 * 
 * Permission is hereby granted, free of charge, to any person or organization obtaining a copy of the software and 
 * accompanying documentation covered by this license (the "Software") to use, reproduce, display, distribute, execute,
 * and transmit the Software, and to prepare derivative works of the Software, and to permit third-parties to whom the
 * Software is furnished to do so, all subject to the following:
 * 
 * The copyright notices in the Software and this entire statement, including the above license grant, this restriction
 * and the following disclaimer, must be included in all copies of the Software, in whole or in part, and all 
 * derivative works of the Software, unless such copies or derivative works are solely in the form of 
 * machine-executable object code generated by a source language processor.
 * 
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE
 * WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT. IN NO EVENT SHALL THE 
 * COPYRIGHT HOLDERS OR ANYONE DISTRIBUTING THE SOFTWARE BE LIABLE FOR ANY DAMAGES OR OTHER LIABILITY, WHETHER IN 
 * CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS 
 * IN THE SOFTWARE.
 *********************************************************************************************************************/
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#include "IfxPort.h"
#include "IfxPort_PinMap.h"

#include "IfxStm.h"
#include "IfxCpu_Irq.h"

typedef struct
{
    Ifx_STM             *stmSfr;            /**< \brief Pointer to Stm register base */
    IfxStm_CompareConfig stmConfig;         /**< \brief Stm Configuration structure */
    volatile uint8       LedBlink;          /**< \brief LED state variable */
    volatile uint32      counter;           /**< \brief interrupt counter */
} App_Stm;

App_Stm g_Stm; /**< \brief Stm global data */

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void IfxStmDemo_init(void);

int core0_main(void)
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

    IfxStmDemo_init();

    /*P00_5    Digital Output*/
    IfxPort_setPinModeOutput(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinLow(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);

    IfxPort_setPinModeOutput(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinLow(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);

    while(1)
    {

    }
    return (1);
}

IFX_INTERRUPT(STM_Int0Handler, 0, 100);

void STM_Int0Handler(void)
{
    static int flag = 0;
    static int cnt = 0;

    IfxStm_clearCompareFlag(g_Stm.stmSfr, g_Stm.stmConfig.comparator);

    cnt++;
    if (flag > 11) flag = 0;
    if(flag == 0)
    {
        IfxStm_increaseCompare(g_Stm.stmSfr, g_Stm.stmConfig.comparator, 500000000u); // 5초 뒤에 interrupt를 걸거다.
        IfxPort_setPinLow(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);
        IfxPort_setPinHigh(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);
        flag = 1;
    }
    else if(flag == 1)
    {
        IfxStm_increaseCompare(g_Stm.stmSfr, g_Stm.stmConfig.comparator, 500000000u);
        IfxPort_setPinLow(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);
        IfxPort_setPinHigh(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);
        flag = 2;
    }
    else{
        IfxStm_increaseCompare(g_Stm.stmSfr, g_Stm.stmConfig.comparator, 50000000u);
        IfxPort_setPinState(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex, IfxPort_State_toggled );

        flag++;
    }

    IfxCpu_enableInterrupts();
}

void IfxStmDemo_init(void)
{
    /* disable interrupts */
    boolean interruptState = IfxCpu_disableInterrupts();

    IfxStm_enableOcdsSuspend(&MODULE_STM0);

    g_Stm.stmSfr = &MODULE_STM0;    //Stm에 0번 모듈을 사용하겠다.
    IfxStm_initCompareConfig(&g_Stm.stmConfig); //stmConfig에 필요한 값 세팅. config 초기화

    //config 설정
    g_Stm.stmConfig.triggerPriority = 100u; //interruptpriority 세팅 (0~0xff = 255)
    g_Stm.stmConfig.typeOfService   = IfxSrc_Tos_cpu0;  //사용할 CPU 선택
    g_Stm.stmConfig.ticks           = 100000000;    //100,000,000 edges = 1s에 한번씩 인터럽트 발생

    IfxStm_initCompare(g_Stm.stmSfr, &g_Stm.stmConfig); //앞의 변수를 사용해 stm 레지스터 초기화. 구조체 적용 및 레지스터 설정

    /* enable interrupts again */
    IfxCpu_restoreInterrupts(interruptState);   //인터럽트 활성화. (config 적용)
}
```

## 신호등 만들기(state: 파, 빨, 파 toggle). switch사용 O
```c
/**********************************************************************************************************************
 * \file Cpu0_Main.c
 * \copyright Copyright (C) Infineon Technologies AG 2019
 * 
 * Use of this file is subject to the terms of use agreed between (i) you or the company in which ordinary course of 
 * business you are acting and (ii) Infineon Technologies AG or its licensees. If and as long as no such terms of use
 * are agreed, use of this file is subject to following:
 * 
 * Boost Software License - Version 1.0 - August 17th, 2003
 * 
 * Permission is hereby granted, free of charge, to any person or organization obtaining a copy of the software and 
 * accompanying documentation covered by this license (the "Software") to use, reproduce, display, distribute, execute,
 * and transmit the Software, and to prepare derivative works of the Software, and to permit third-parties to whom the
 * Software is furnished to do so, all subject to the following:
 * 
 * The copyright notices in the Software and this entire statement, including the above license grant, this restriction
 * and the following disclaimer, must be included in all copies of the Software, in whole or in part, and all 
 * derivative works of the Software, unless such copies or derivative works are solely in the form of 
 * machine-executable object code generated by a source language processor.
 * 
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE
 * WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE, TITLE AND NON-INFRINGEMENT. IN NO EVENT SHALL THE 
 * COPYRIGHT HOLDERS OR ANYONE DISTRIBUTING THE SOFTWARE BE LIABLE FOR ANY DAMAGES OR OTHER LIABILITY, WHETHER IN 
 * CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS 
 * IN THE SOFTWARE.
 *********************************************************************************************************************/
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"

#include "IfxPort.h"
#include "IfxPort_PinMap.h"

#include "IfxStm.h"
#include "IfxCpu_Irq.h"

#define RED 0
#define BLUE 1
#define BLINK 2

typedef struct
{
    Ifx_STM             *stmSfr;            /**< \brief Pointer to Stm register base */
    IfxStm_CompareConfig stmConfig;         /**< \brief Stm Configuration structure */
    volatile uint8       LedBlink;          /**< \brief LED state variable */
    volatile uint32      counter;           /**< \brief interrupt counter */
} App_Stm;

App_Stm g_Stm; /**< \brief Stm global data */

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void IfxStmDemo_init(void);
int state = RED;
int core0_main(void)
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

    IfxStmDemo_init();

    /*P00_5    Digital Output*/
    IfxPort_setPinModeOutput(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinLow(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);

    IfxPort_setPinModeOutput(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinLow(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);

    while(1) {
    }
    return (1);
}

IFX_INTERRUPT(STM_Int0Handler, 0, 100);

void STM_Int0Handler(void)
{
    static int flag = 0;
    static int cnt = 0;

    IfxStm_clearCompareFlag(g_Stm.stmSfr, g_Stm.stmConfig.comparator);
    IfxStm_increaseCompare(g_Stm.stmSfr, g_Stm.stmConfig.comparator, 50000000u);
    cnt++;
    //state transition
    if (cnt % 10 == 0) {
        switch (state) {
            case RED:
                state = BLUE;
                break;
            case BLUE:
                state = BLINK;
                break;
            case BLINK:
                state = RED;
                break;
            default:
                state = RED;
                break;
        }
    }
    //state action
    switch (state) {
        case RED:
            IfxPort_setPinLow(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);
            IfxPort_setPinHigh(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);
            break;
        case BLUE:
            IfxPort_setPinLow(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);
            IfxPort_setPinHigh(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);
            break;
        case BLINK:
            IfxPort_togglePin(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);
            break;
        default:
            IfxPort_setPinLow(IfxPort_P10_2.port, IfxPort_P10_2.pinIndex);
            IfxPort_setPinHigh(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);
            break;
    }
    IfxCpu_enableInterrupts();
}

void IfxStmDemo_init(void)
{
    /* disable interrupts */
    boolean interruptState = IfxCpu_disableInterrupts();

    IfxStm_enableOcdsSuspend(&MODULE_STM0);

    g_Stm.stmSfr = &MODULE_STM0;    //Stm에 0번 모듈을 사용하겠다.
    IfxStm_initCompareConfig(&g_Stm.stmConfig); //stmConfig에 필요한 값 세팅. config 초기화

    //config 설정
    g_Stm.stmConfig.triggerPriority = 100u; //interruptpriority 세팅 (0~0xff = 255)
    g_Stm.stmConfig.typeOfService   = IfxSrc_Tos_cpu0;  //사용할 CPU 선택
    g_Stm.stmConfig.ticks           = 100000000;    //100,000,000 edges = 1s에 한번씩 인터럽트 발생

    IfxStm_initCompare(g_Stm.stmSfr, &g_Stm.stmConfig); //앞의 변수를 사용해 stm 레지스터 초기화. 구조체 적용 및 레지스터 설정

    /* enable interrupts again */
    IfxCpu_restoreInterrupts(interruptState);   //인터럽트 활성화. (config 적용)
}
```
