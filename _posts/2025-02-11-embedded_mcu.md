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
✅ 어려워: ShieldBuddy 메뉴얼 표를 찾아가며 프로그래밍하는 것이 처음이라 다소 어려웠다. 첫날엔 7시까지 남아서 공부를 하다 집에 와서 더 했다. 잘 배워야겠다 생각했다.   
✅ 팀워크: 팀원들과 모르는 부분을 서로 채워주어 이해하기 수월했다. 팀워크는 중요하고 소중하다는 것을 가장 많이 느꼈다...(매번 느끼는 중) 우리는 나보다 똑똑하다.   

# 임베디드 시스템

## 임베디드 시스템
✅ 임베디드 시스템   
마이크로 컨트롤러가 있으면 임베디드 시스템이라고 생각하면된다. 컴퓨터와 소자 그 중간 어딘가.   
   
✅ 마이크로 프로세서(Microprocessor): 연산용   
PC의 CPU   
고속연산과 대용량 메모리 처리   
인텔 펜티엄   
MPU(Micro Processor Unit)   
   
✅ 마이크로 컨트롤러(Microcontroller)   
기계나 장치를 제어하기 위해 사용하는 프로그램이 가능한 칩(chip)   
CPU, 기본 메모리, I/O 포트, 타이머, 직렬통신 등의 기능을 하나의 칩에 내장   
8051, AVR 등   
MCU(Micro Control Unit)   
   
✅ 디버거   
보드 안에 디버거가 있냐 없냐에 따라 가격이 달라진다. 우리가 쓸 보드는 디버거가 내재돼있다.   
   
✅ 중요 부분   
1. (DI/DO) GPIO(-> IDE, Interface, Editor). Digital In, Digital Out: 입력으로도 쓸 수 있고 출력으로도 쓸 수 있는 범용적인 핀   
2. Preemptive Shiel
3. (AI) ADC. Analog In   
4. (유사AO) PWM. Analog Out   
   
이 네 가지를 배울 것이다.   

## 임베디드 개발을 위한 회로도  
임베디드 개발자는 회로의 모든 것을 다 알필요는 없다!   
   
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
ctrl+클릭: 세부사항 확인 가능   
ctrl+스페이스: 자동완성 가능   
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
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;  // 인풋이니까 10 아님. User Manual p.1089에 나옴

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
        curSW = P02_IN.U & (0x1 << P1_IDX); // 눌리면 P02_IN.U가 0   User Manual p.1105
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
✅ IR(Interrupt Router): external request unit, internal request unit으로 구성   

## TS275 외부 인터럽트 사용
✅ External Request Selection Unit(ERS):   
input channel에 연결된 4개의 pin 중 하나를 선택   
✅ Event Trigger Logic Unit(ETL): 
언제 이벤트 트리거를 발생시킬 것인가. input edge, rising edge selection, falling edge selection 등이 있다. EICR에 작성된 내용을 바탕으로 rising edge, falling edge를 선택하여 trigger 이벤트를 발생시킨다.   
✅ Output Gating Unit(OGU):   
input channel에서 생성된 trigger pulser를 조합하여 external request oupt을 생성한다.   
✅ Control register:
EICR 레지스터, IGCR 레지스터
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
    //EICR -> OGU -> IGCR -> ERU
    //set EICR: 외부 신호(버튼, 센서 등)를 감지하여 인터럽트를 발생시키도록 설정
    SCU_EICR1.U &= ~(0x7 << EXISO_IDX);
    SCU_EICR1.U |= 0x1 << EXISO_IDX; // 감지할 입력 신호 선택

    SCU_EICR1.U |= 1 << FENO_IDX; // 하강 엣지(신호가 HIGH에서 LOW로 변화할 때) 감지 활성화. 이 설정으로 신호가 하강 엣지에서 변할 때 인터럽트가 발생한다.
    // 떼는 동작은 rising egde
    SCU_EICR1.U |= 1 << EIENO_IDX;  

    SCU_EICR1.U &= ~(0x7 << INP0_IDX); // OGU0를 사용하기 위한 초기화이자 000 값 설정. 0x7 -> OGU, INP0_IDX -> ERS2 사용이랑 관련. 즉 SW랑 OGU랑 연관되는 부분

    //set IGCR: 감지된 신호가 인터럽트를 발생시키는 방식 결정
    SCU_IGCR0.U &= ~(0x3 << IGP0_IDX); // IOUT으로 전달하기 위해 초기화.  OGU0 -> IOUT1 으로 추측
    SCU_IGCR0.U |= 0x1 << IGP0_IDX; // 0b01로 설정

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

int core0_main(void) {
    IfxCpu_enableInterrupts();
    
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent,1);
    //initLED가 겹친다고 여기에 둠. 
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U &= ~(0X10 << PCn_2_IDX);          // 이 부분만 바뀜

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
    initGPIO();
    initERU();
    while(1) {
    }
    return(1);
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
    P10_OUT.U = 0x40004;
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
    SCU_EICR1.U |= 0x1 << EXISO_IDX; // 아래버튼. 감지할 입력 신호 선택 001
    SCU_EICR1.U |= 0x2 << (EXISO_IDX + 16); // 윗버튼  010

    SCU_EICR1.U |= 1 << RENO_IDX; // 하강 엣지(신호가 HIGH에서 LOW로 변화할 때) 감지 활성화. 이 설정으로 신호가 하강 엣지에서 변할 때 인터럽트가 발생한다.
    SCU_EICR1.U |= 1 << EIENO_IDX;
    SCU_EICR1.U |= 1 << RENO_IDX1; // 사용하는 회로가 눌렸을때 1->0으로 변화하므로 falling edge로 하면 된다.
    SCU_EICR1.U |= 1 << EIENO_IDX1;

    SCU_EICR1.U &= ~(0x7 << INP0_IDX);  //007 -> OGU0, INP0_IDX -> ERS2(아래버튼)
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

    SCU_EICR1.U &= ~(0x7 << INP0_IDX);  //여기서부터 OGU
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
   
✅ 보드에서 정확한 clock의 생성은?   
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

# 실습
## 파란색 LED 깜빡깜빡
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

## 위에 실습에서 버튼 누르면 state변화 없애기
✅ interrupt를 추가하면 된다.   
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

uint8 bStateTrans = 1; // 표준대로 uint8 변수는 이름이 앞에 b이다.
IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initGPIO(void);
void initERU(void);

void STM_Int0Handler(void);

IFX_INTERRUPT(ISR0, 0, 0x10); // 0x10이 발생했을 때 ISR0를 실행한다.
IFX_INTERRUPT(ISR1, 0, 0x20); // 0x20이 발생했을 때 ISR0를 실행한다.

void ISR0(void) {
    bStateTrans = ! bStateTrans;
}

void ISR1(void) {
    bStateTrans = ! bStateTrans;
}

typedef struct
{
    Ifx_STM             *stmSfr;            /**< \brief Pointer to Stm register base */
    IfxStm_CompareConfig stmConfig;         /**< \brief Stm Configuration structure */
    volatile uint8       LedBlink;          /**< \brief LED state variable */
    volatile uint32      counter;           /**< \brief interrupt counter */
} App_Stm;

App_Stm g_Stm; /**< \brief Stm global data */


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

    initERU();
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
    if (bStateTrans) {
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

void initGPIO(void) {
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;
}

void initERU(void) {
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

# Task scheduling
## Task scheduling이 필요한 이유
✅ Multi Programming이 가능하도록 하기 위해   
non-preemtive(비선점형): FIFO   
preemtive(선점형): 우선순위   
   
우리는 non-preemtive를 만들어 볼 것이다.   
임베디드 sw는 센서의 값을 획득, 판단, 수행 하는 역할이다. 획득, 판단, 수행는 각각의 task다.   
Measure는 1ms마다 Control이 10ms마다, Action이 100ms마다 이루어져야하는 경우->Timing을 맞추기가 어렵다.   

# 실습
## 빨강0.1초, 파랑 1초마다 깜빡
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

#include "Driver_Stm.h"

//GPIO related
#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1

// ERU related
#define EXIS0_IDX 4
#define FEN0_IDX 8
#define EIEN0_IDX 11
#define INP0_IDX 12
#define IGP0_IDX 14

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11

typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

// Task scheduling related
void AppScheduling(void);
void AppTask1ms(void);
void AppTask10ms(void);
void AppTask100ms(void);
void AppTask1000ms(void);


/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

IfxCpu_syncEvent g_cpuSyncEvent = 0;


void initGPIO(void);
void initERU(void);


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

    initGPIO();
    Driver_Stm_Init();

    while(1)
    {
        AppScheduling();
    }
    return (1);
}

void initGPIO(void){
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;
}

void AppTask1ms(void)
{
    stTestCnt.u32nuCnt1ms++;
}

void AppTask10ms(void)
{
    stTestCnt.u32nuCnt10ms++;

}

void AppTask100ms(void)
{
    static int flag = 0;
    if(flag == 0)
    {
        IfxPort_setPinLow(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);
        flag = 1;
    }
    else
    {
        IfxPort_setPinHigh(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);
        flag = 0;
    }
    stTestCnt.u32nuCnt100ms++;
}

void AppTask1000ms(void)
{
    static int flag = 0;
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
    stTestCnt.u32nuCnt1000ms++;

}

void AppScheduling(void)
{
    if(stSchedulingInfo.u8nuScheduling1msFlag == 1u)
    {
        stSchedulingInfo.u8nuScheduling1msFlag = 0u;
        AppTask1ms();

        if(stSchedulingInfo.u8nuScheduling10msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling10msFlag = 0u;
            AppTask10ms();
        }

        if(stSchedulingInfo.u8nuScheduling100msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling100msFlag = 0u;
            AppTask100ms();
        }
        if(stSchedulingInfo.u8nuScheduling1000msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling1000msFlag = 0u;
            AppTask1000ms();
        }
    }
}
```

# SPI를 활용한FND제어
## 개요
✅ 복잡한7 Segment FND 회로를SPI 통신방법을 활용하여 제어해보자.   
✅ 개발순서   
▪ 회로파악(동작파악)   
▪ GPIO핀세팅   
▪ 동작코드코딩   

✅ IC(Integrated Circuit)   
뒷면의 검은색 두개의 칩. 여러가지가 집적돼있음.   
<img src="https://github.com/user-attachments/assets/0e677be4-bb1b-4c02-b37a-3634a9e85d1b" width="400" height="440">   
메인칩(TM74HC595) + 최소한의회로= 모듈(쪽보드)   

✅ 통신을왜사용하는가?   
up 0과1의 이진값을 기본으로 함.   
비트 단위의 데이터 교환   
주변장치에 따라 많은 핀이 필요하므로 176개 핀으로 부족   
   
✅ 시리얼통신 vs 병렬통신   
병렬 방식   
▪ 여러 개의 핀을 사용   
▪ 바이트 단위 이상의 데이터를 동시에 전송   
시리얼(또는직렬) 방식   
▪ 적은 수의 핀을 사용   
▪ 바이트 단위 이상의 데이터를 여러 번에 나누어 전송   
▪ 핀수가 제한된 마이크로 컨트롤러에서 선호   
   
✅ 동기 전송 방식   
<img src="https://github.com/user-attachments/assets/ea4c5e5f-710b-4638-acd0-8346c4605b87" width="440" height="440">   
▪ 데이터와는 별도로 동기화를 위한 클록 사용   
▪ SPI, I2C (또는 2-Wire)   
   
✅ 비동기 전송 방식   
▪ 전송속도를 약속하여 사용하고 별도의 클록을 전송하지 않음   
▪ 동일한 속도로 송수신하지 않으면 잘못된 데이터 전달   
▪ UART   
데이터 보낼 때 서로 데이터 전송 시간(싱크)을 맞춤   
   
✅ 시리얼 통신: 전이중 방식 vs 반이중 방식   
<img src="https://github.com/user-attachments/assets/a492e994-5847-4fdd-aaca-03891eebefd4" width="500" height="60">   
 전이중방식(full duplex)   
▪ 송수신을 위해 2개의 데이터 연결선 사용   
▪ 송수신 동시에 가능   
반이중방식(half-duplex)   
▪ 송수신을 위해 1개의 데이터 연결선 사용   
▪ 송수신 동시에 불가능   
   
✅ 대표적 시리얼 통신 연결 방식   
<img src="https://github.com/user-attachments/assets/d6f6e7e0-496b-4725-b7fe-287aaeb91929" width="500" height="300">   
<img src="https://github.com/user-attachments/assets/6ed389c8-73db-4231-8d94-57e562f39bf0" width="500" height="250">   
<img src="https://github.com/user-attachments/assets/6ed389c8-73db-4231-8d94-57e562f39bf0" width="500" height="250">   

✅ SPI(Serial Peripheral Interface) 연결   
▪ UART와 같은 전이중 방식 통신   
MOSI : Mater Out Slave In   
MISO : Master In Slave Out   
   
▪ UART와 달리 일대다(1:n) 통신 지원, 마스터-슬레이브 구조   
마스터가 슬레이브를 제어하고 통신을 관장   
n개 슬레이브 중 하나를 선택하기 위해SS(Slave Select) 신호를 슬레이브 당 하나씩 사용   
모든 슬레이브는 MOSI와 MISO 연결선을 공유   
SS 신호로 선택된 슬레이브만 데이터를 송수신함   
   
▪ 동기 방식으로 동기화를 위해 별도의 SCK 신호 사용   

<img src="https://github.com/user-attachments/assets/602745ac-cce7-4258-a94d-a9ca616bc917" width="500" height="330">   
<img src="https://github.com/user-attachments/assets/602745ac-cce7-4258-a94d-a9ca616bc917" width="330" height="500">   
손에 조그맣게 나있는 상처는 운동하다 다친거다.   

# 실습
## FND 4개 1234 출력
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

#include "Driver_Stm.h"

//GPIO related
#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1
#define PCn_0_IDX 3

#define SCLK IfxPort_P00_0
#define RCLK IfxPort_P00_1
#define DIO IfxPort_P00_2
// ERU related
#define EXIS0_IDX 4
#define FEN0_IDX 8
#define EIEN0_IDX 11
#define INP0_IDX 12
#define IGP0_IDX 14

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11
void send(unsigned char X);
void send_port(unsigned char X, unsigned char port);
uint8_t _LED_OF[10] = {0xC0, 0xf9, 0xa4, 0xb0, 0x99, 0x92, 0x82, 0xf8, 0x80, 0x90};

typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

// Task scheduling related
void AppScheduling(void);
void AppTask1ms(void);
void AppTask10ms(void);
void AppTask100ms(void);
void AppTask1000ms(void);


/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

IfxCpu_syncEvent g_cpuSyncEvent = 0;


void initGPIO(void);
void initERU(void);


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

    initGPIO();
    Driver_Stm_Init();

    while(1)
    {
//        AppScheduling();
        send_port(_LED_OF[4], 0x1);
        send_port(_LED_OF[3], 0x2);
        send_port(_LED_OF[2], 0x4);
        send_port(_LED_OF[1], 0x8);
    }
    return (1);
}

void initGPIO(void){
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    IfxPort_setPinModeOutput(SCLK.port, SCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(RCLK.port, RCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(DIO.port, DIO.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
}
void send(unsigned char X)
{

  for (int i = 8; i >= 1; i--)
  {
    if (X & 0x80)
    {
        IfxPort_setPinHigh(DIO.port, DIO.pinIndex);
    }
    else
    {
        IfxPort_setPinLow(DIO.port, DIO.pinIndex);
    }
    X <<= 1;
    IfxPort_setPinLow(SCLK.port, SCLK.pinIndex);
    IfxPort_setPinHigh(SCLK.port, SCLK.pinIndex);
  }
}

void send_port(unsigned char X, unsigned char port) // 4개의 led중에서 선택해서 송신하는 함수
{
  send(X);
  send(port);
  IfxPort_setPinLow(RCLK.port, RCLK.pinIndex);
  IfxPort_setPinHigh(RCLK.port, RCLK.pinIndex);
}
void AppTask1ms(void)
{
    stTestCnt.u32nuCnt1ms++;
}

void AppTask10ms(void)
{
    stTestCnt.u32nuCnt10ms++;

}

void AppTask100ms(void)
{
    static int flag = 0;
    if(flag == 0)
    {
        IfxPort_setPinLow(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);
        flag = 1;
    }
    else
    {
        IfxPort_setPinHigh(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);
        flag = 0;
    }
    stTestCnt.u32nuCnt100ms++;
}

void AppTask1000ms(void)
{
    static int flag = 0;
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
    stTestCnt.u32nuCnt1000ms++;

}

void AppScheduling(void)
{
    if(stSchedulingInfo.u8nuScheduling1msFlag == 1u)
    {
        stSchedulingInfo.u8nuScheduling1msFlag = 0u;
        AppTask1ms();

        if(stSchedulingInfo.u8nuScheduling10msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling10msFlag = 0u;
            AppTask10ms();
        }

        if(stSchedulingInfo.u8nuScheduling100msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling100msFlag = 0u;
            AppTask100ms();
        }
        if(stSchedulingInfo.u8nuScheduling1000msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling1000msFlag = 0u;
            AppTask1000ms();
        }
    }
}
```

## 시간 반영한 카운터 & 정지 버튼 내가 만든거
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

#include "Driver_Stm.h"

//GPIO related
#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1
#define PCn_0_IDX 3

#define SCLK IfxPort_P00_0
#define RCLK IfxPort_P00_1
#define DIO IfxPort_P00_2
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
#define SRE_IDX 10
#define TOS_IDX 11
void send(unsigned char X);
void send_port(unsigned char X, unsigned char port);
uint8_t _LED_OF[10] = {0xC0, 0xf9, 0xa4, 0xb0, 0x99, 0x92, 0x82, 0xf8, 0x80, 0x90};

#define BLINK 1
#define STOP 0

int cnt;
uint8_t n1, n2, n3, n4 = 0;
uint8_t state = STOP;
typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

// Task scheduling related
void AppScheduling(void);
void AppTask1ms(void);
void AppTask10ms(void);
void AppTask100ms(void);
void AppTask1000ms(void);

/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initGPIO(void);
void initERU(void);

IFX_INTERRUPT(ISR0,0,0x10); // 0x10이 발생했을 때 ISR0를 실행한다.
IFX_INTERRUPT(ISR1,0,0x20); // 0x10이 발생했을 때 ISR0를 실행한다.

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
    P10_OMR.U = 0x20002;
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

    initGPIO();
    Driver_Stm_Init();
    initERU();

    while(1)
    {
        AppScheduling();

    }
    return (1);
}

void initGPIO(void){
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    IfxPort_setPinModeOutput(SCLK.port, SCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(RCLK.port, RCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(DIO.port, DIO.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
}
void send(unsigned char X)
{
  for (int i = 8; i >= 1; i--)
  {
    if (X & 0x80)
    {
        IfxPort_setPinHigh(DIO.port, DIO.pinIndex);
    }
    else
    {
        IfxPort_setPinLow(DIO.port, DIO.pinIndex);
    }
    X <<= 1;
    IfxPort_setPinLow(SCLK.port, SCLK.pinIndex);
    IfxPort_setPinHigh(SCLK.port, SCLK.pinIndex);
  }
}

void send_port(unsigned char X, unsigned char port) // 4개의 led중에서 선택해서 송신하는 함수
{
  send(X);
  send(port);
  IfxPort_setPinLow(RCLK.port, RCLK.pinIndex);
  IfxPort_setPinHigh(RCLK.port, RCLK.pinIndex);
}
void AppTask1ms(void)
{
    stTestCnt.u32nuCnt1ms++;
}

void AppTask10ms(void)
{
    if(state == BLINK)
        cnt++;
    stTestCnt.u32nuCnt10ms++;

}

void AppTask100ms(void)
{
    stTestCnt.u32nuCnt100ms++;
}

void AppTask1000ms(void)
{
    static int flag = 0;
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
    stTestCnt.u32nuCnt1000ms++;

}

void AppScheduling(void)
{
    n1 = (int) cnt % 10;
    n2 = (int) (cnt % 100) / 10;
    n3 = (int) (cnt % 1000) / 100;
    n4 = (int) (cnt % 10000) / 1000;

    send_port(_LED_OF[n1], 0x1);
    send_port(_LED_OF[n2], 0x2);
    send_port(_LED_OF[n3], 0x4);
    send_port(_LED_OF[n4], 0x8);

    if(cnt == 10000) cnt = 0;

    if(stSchedulingInfo.u8nuScheduling1msFlag == 1u)
    {
        stSchedulingInfo.u8nuScheduling1msFlag = 0u;
        AppTask1ms();

        if(stSchedulingInfo.u8nuScheduling10msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling10msFlag = 0u;
            AppTask10ms();
        }

        if(stSchedulingInfo.u8nuScheduling100msFlag == 1u)
        {

            stSchedulingInfo.u8nuScheduling100msFlag = 0u;
            AppTask100ms();
        }
        if(stSchedulingInfo.u8nuScheduling1000msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling1000msFlag = 0u;
            AppTask1000ms();
        }
    }
}

void initERU (void) {
    //EICR 설정(설정) -> OGU 설정(전달) ->  IGCR 값 설정(IOUT으로 전달하기 위해) -> SRC_SCU_SCU_ERU 설정
    //set EICR: 외부 신호(버튼, 센서 등)를 감지하여 인터럽트를 발생시키도록 설정
    SCU_EICR1.U &= ~(0x7 << EXISO_IDX | 0x7 << (EXISO_IDX + 16)); // 1번레지스터에서 사용할 곳 초기화. ERS2, ERS3가 사용한다.
    SCU_EICR1.U |= 0x1 << EXISO_IDX; // 아랫버튼. 감지할 입력 신호 선택 001
    SCU_EICR1.U |= 0x2 << (EXISO_IDX + 16); //윗버튼   010

    SCU_EICR1.U |= 1 << RENO_IDX; // 하강 엣지(신호가 HIGH에서 LOW로 변화할 때) 감지 활성화. 이 설정으로 신호가 하강 엣지에서 변할 때 인터럽트가 발생한다.
    SCU_EICR1.U |= 1 << EIENO_IDX;
    SCU_EICR1.U |= 1 << RENO_IDX1; // 사용하는 회로가 눌렸을때 1->0으로 변화하므로 falling edge로 하면 된다.
    SCU_EICR1.U |= 1 << EIENO_IDX1;

    SCU_EICR1.U &= ~(0x7 << INP0_IDX); //여기서부터 OGU.. 이건 OGU 초기화이자 값 대입 // 아래 버튼이 OGU0로 간다는걸 정해줌
    SCU_EICR1.U &= ~(0x7 << INP1_IDX);  //OGU1 사용하려고 초기화    OGU: ERU 결과를 특정 핀(GPIO)이나 PWM 등의 모듈에 전달
    SCU_EICR1.U |= 0x1 << INP1_IDX;     //OGU1 사용하려고 값 대입

    //set IGCR: 감지된 신호가 인터럽트를 발생시키는 방식 결정
    SCU_IGCR0.U &= ~(0x3 << IGP0_IDX);   //IGP0는 IDX 14부터 시작한다... OGU 0
    SCU_IGCR0.U |= 0x1 << IGP0_IDX;      //1로 값을 설정 (IOUT)
    SCU_IGCR0.U &= ~(0x3 << IGP1_IDX);   //OGU 1
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

## 위에 실습에서 교수님께서 하신 것
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

#include "Driver_Stm.h"

//GPIO related
#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1
#define PCn_0_IDX 3

#define SCLK IfxPort_P00_0
#define RCLK IfxPort_P00_1
#define DIO IfxPort_P00_2
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
#define SRE_IDX 10
#define TOS_IDX 11
void send(unsigned char X);
void send_port(unsigned char X, unsigned char port);
uint8_t _LED_OF[10] = {0xC0, 0xf9, 0xa4, 0xb0, 0x99, 0x92, 0x82, 0xf8, 0x80, 0x90};

#define BLINK 1
#define STOP 0

int cnt;
uint8_t n1, n2, n3, n4 = 0;
uint8_t state = STOP;
typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

// Task scheduling related
void AppScheduling(void);
void AppTask1ms(void);
void AppTask10ms(void);
void AppTask100ms(void);
void AppTask1000ms(void);

/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initGPIO(void);
void initERU(void);

IFX_INTERRUPT(ISR0,0,0x10); // 0x10이 발생했을 때 ISR0를 실행한다.
IFX_INTERRUPT(ISR1,0,0x20); // 0x10이 발생했을 때 ISR0를 실행한다.

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
    P10_OMR.U = 0x20002;
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

    initGPIO();
    Driver_Stm_Init();
    initERU();

    while(1)
    {
        AppScheduling();

        n1 = (uint8_t) stTestCnt.u32nuCnt10ms %10;
        n2 = (uint8_t) stTestCnt.u32nuCnt100ms % 10;
        n3 = (uint8_t) stTestCnt.u32nuCnt1000ms % 10;
        n4 = (uint8_t) (stTestCnt.u32nuCnt1000ms % 100) / 10;

        send_port(_LED_OF[n1], 0x1);
        send_port(_LED_OF[n2], 0x2);
        send_port(_LED_OF[n3]&0x7f, 0x4); //점 찍기
        send_port(_LED_OF[n4], 0x8);
    }
    return (1);
}

void initGPIO(void){
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    IfxPort_setPinModeOutput(SCLK.port, SCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(RCLK.port, RCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(DIO.port, DIO.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
}
void send(unsigned char X)
{
  for (int i = 8; i >= 1; i--)
  {
    if (X & 0x80)
    {
        IfxPort_setPinHigh(DIO.port, DIO.pinIndex);
    }
    else
    {
        IfxPort_setPinLow(DIO.port, DIO.pinIndex);
    }
    X <<= 1;
    IfxPort_setPinLow(SCLK.port, SCLK.pinIndex);
    IfxPort_setPinHigh(SCLK.port, SCLK.pinIndex);
  }
}

void send_port(unsigned char X, unsigned char port) // 4개의 led중에서 선택해서 송신하는 함수
{
  send(X);
  send(port);
  IfxPort_setPinLow(RCLK.port, RCLK.pinIndex);
  IfxPort_setPinHigh(RCLK.port, RCLK.pinIndex);
}
void AppTask1ms(void)
{
    stTestCnt.u32nuCnt1ms++;
}

void AppTask10ms(void)
{
    stTestCnt.u32nuCnt10ms++;
}

void AppTask100ms(void)
{
    stTestCnt.u32nuCnt100ms++;
}

void AppTask1000ms(void)
{
    static int flag = 0;
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
    stTestCnt.u32nuCnt1000ms++;

}

void AppScheduling(void)
{

    if(stSchedulingInfo.u8nuScheduling1msFlag == 1u)
    {
        stSchedulingInfo.u8nuScheduling1msFlag = 0u;
        AppTask1ms();

        if(stSchedulingInfo.u8nuScheduling10msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling10msFlag = 0u;
            AppTask10ms();
        }

        if(stSchedulingInfo.u8nuScheduling100msFlag == 1u)
        {

            stSchedulingInfo.u8nuScheduling100msFlag = 0u;
            AppTask100ms();
        }
        if(stSchedulingInfo.u8nuScheduling1000msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling1000msFlag = 0u;
            AppTask1000ms();
        }
    }
}

void initERU (void) {
    //EICR 설정(설정) -> OGU 설정(전달) ->  IGCR 값 설정(IOUT으로 전달하기 위해) -> SRC_SCU_SCU_ERU 설정
    //set EICR: 외부 신호(버튼, 센서 등)를 감지하여 인터럽트를 발생시키도록 설정
    SCU_EICR1.U &= ~(0x7 << EXISO_IDX | 0x7 << (EXISO_IDX + 16)); // 1번레지스터에서 사용할 곳 초기화. ERS2, ERS3가 사용한다.
    SCU_EICR1.U |= 0x1 << EXISO_IDX; // 아랫버튼. 감지할 입력 신호 선택 001
    SCU_EICR1.U |= 0x2 << (EXISO_IDX + 16); //윗버튼   010

    SCU_EICR1.U |= 1 << RENO_IDX; // 하강 엣지(신호가 HIGH에서 LOW로 변화할 때) 감지 활성화. 이 설정으로 신호가 하강 엣지에서 변할 때 인터럽트가 발생한다.
    SCU_EICR1.U |= 1 << EIENO_IDX;
    SCU_EICR1.U |= 1 << RENO_IDX1; // 사용하는 회로가 눌렸을때 1->0으로 변화하므로 falling edge로 하면 된다.
    SCU_EICR1.U |= 1 << EIENO_IDX1;

    SCU_EICR1.U &= ~(0x7 << INP0_IDX); //여기서부터 OGU.. 이건 OGU 초기화이자 값 대입 // 아래 버튼이 OGU0로 간다는걸 정해줌
    SCU_EICR1.U &= ~(0x7 << INP1_IDX);  //OGU1 사용하려고 초기화    OGU: ERU 결과를 특정 핀(GPIO)이나 PWM 등의 모듈에 전달
    SCU_EICR1.U |= 0x1 << INP1_IDX;     //OGU1 사용하려고 값 대입

    //set IGCR: 감지된 신호가 인터럽트를 발생시키는 방식 결정
    SCU_IGCR0.U &= ~(0x3 << IGP0_IDX);   //IGP0는 IDX 14부터 시작한다... OGU 0
    SCU_IGCR0.U |= 0x1 << IGP0_IDX;      //1로 값을 설정 (IOUT)
    SCU_IGCR0.U &= ~(0x3 << IGP1_IDX);   //OGU 1
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

# ADC
## ADC란?
✅ Analog to Digital Converter   

## Dedug창이 안 보일땐?
이제부터 Debug창을 많이 볼 것이다. 그전까진 Debug창을 볼 필요가 없어서 지워놨었는데 찾느라 고생했다...(사실 교수님께서 알려주셨다.)   
   
✅ 우선 현재 디버그 중인 파일을 멈추기 위해 우측상단에 가장 오른쪽에 있는 버튼으로 들어가 중앙에서 좌측에 놓인 빨강네모로 디버그를 멈춘다.   
<img src="https://github.com/user-attachments/assets/ee19064f-2fb0-4b60-b8b0-f222bbd017ba" width="350" height="250">   
   
✅ Window -> Showview -> Expression에 들어가 우리가 보고싶은 adcResult를 검색한다. 이때 adcResult = Driver_Adc()_DataObtain(); 이 적힌 코드에 breakpoint를 걸어야한다. 그러면 
값이 바뀔 때 마다 값이 바뀌는 것을 expression 창에서 볼 수 있다.   
<img src="https://github.com/user-attachments/assets/ff4f4c1e-0705-4c72-8ec4-cb7b048e6682" width="350" height="250">   
✅ 디버그 중인거 멈추려면 Window -> Show View -> Debug 하면된다.   
   
✅ ctrl + alt + f 안 먹힐 때?   
Debug를 멈추자!!   

# 실습
## ADC 값 확인하기
✅ 소스 코드
```c
// STM+FND

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

#include "Driver_Stm.h"
#include "Driver_Adc.h"

//GPIO related
#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1
#define PCn_0_IDX 3

#define STOP 0
#define RUNNING 1

#define SCLK IfxPort_P00_0
#define RCLK IfxPort_P00_1
#define DIO IfxPort_P00_2

// ERU related
#define EXIS0_IDX 4
#define FEN0_IDX 8
#define EIEN0_IDX 11
#define INP0_IDX 12
#define IGP0_IDX 14

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11

uint32_t adcResult = 0;

typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

// Task scheduling related
void AppScheduling(void);
void AppTask1ms(void);
void AppTask10ms(void);
void AppTask100ms(void);
void AppTask1000ms(void);


/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

IfxCpu_syncEvent g_cpuSyncEvent = 0;

uint8_t _LED_0F[10] = {0xC0,0xf9,0xa4,0xb0,0x99,0x92,0x82,0xf8,0x80,0x90};
int n = 0;
uint8_t state = STOP;

uint8_t n1, n2, n3, n4;

IFX_INTERRUPT(ISR0, 0, 0x10);
void ISR0 (void){
    state = (state==STOP)? RUNNING: STOP;
    P10_OMR.U = 0x20002;
}

IFX_INTERRUPT(ISR1, 0, 0x20);
void ISR1 (void){
    n=0;
}
void initGPIO(void);
void initERU(void);
void send(unsigned char X);
void send_port(unsigned char X, unsigned char port);

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

    initGPIO();
    initERU();
    Driver_Stm_Init();
    Driver_Adc_Init();

    while(1)
    {
        AppScheduling();

        n1 = (uint8_t) stTestCnt.u32nuCnt10ms%10;
        n2 = (uint8_t) stTestCnt.u32nuCnt100ms%10;
        n3 = (uint8_t) stTestCnt.u32nuCnt1000ms%10;
        n4 = (uint8_t) (stTestCnt.u32nuCnt1000ms%100)/10;

        send_port(_LED_0F[n1],0x1);
        send_port(_LED_0F[n2],0x2);
        send_port(_LED_0F[n3]&0x7f,0x4);
        send_port(_LED_0F[n4],0x8);

    }
    return (1);
}

void initGPIO(void){
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    IfxPort_setPinModeOutput(SCLK.port, SCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
//    P00_IOCR0.U &= ~(0x1F << PCn_0_IDX);
//    P00_IOCR0.U |= 0x10 << PCn_0_IDX;
    IfxPort_setPinModeOutput(RCLK.port, RCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(DIO.port, DIO.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
}

void initERU(void){
    // set EICR
       SCU_EICR1.U &= ~(0x7 << EXIS0_IDX);
       SCU_EICR1.U |= 0x1 << EXIS0_IDX;

       SCU_EICR1.U |= 1 << FEN0_IDX;
       SCU_EICR1.U |= 1 << EIEN0_IDX;

       SCU_EICR1.U &= ~(0x7 << INP0_IDX);

       // set IGCR
       SCU_IGCR0.U &= ~(0x3 << IGP0_IDX );
       SCU_IGCR0.U |= 0x1 << IGP0_IDX;

       // set SCUERU
       SRC_SCU_SCU_ERU0.U &= ~0xff;
       SRC_SCU_SCU_ERU0.U |= 0x10;

       SRC_SCU_SCU_ERU0.U |= 1<<SRE_IDX;
       SRC_SCU_SCU_ERU0.U &= ~(0x3 <<TOS_IDX);

       // set EICR
           SCU_EICR1.U &= ~(0x7 << EXIS0_IDX+16);
           SCU_EICR1.U |= 0x2 << EXIS0_IDX+16;

           SCU_EICR1.U |= 1 << FEN0_IDX+16;
           SCU_EICR1.U |= 1 << EIEN0_IDX+16;

           SCU_EICR1.U &= ~(0x7 << INP0_IDX+16);
           SCU_EICR1.U |= 0x1 << INP0_IDX+16;

           // set IGCR
           SCU_IGCR0.U &= ~(0x3 << IGP0_IDX+16 );
           SCU_IGCR0.U |= 0x1 << IGP0_IDX+16;

           // set SCUERU
           SRC_SCU_SCU_ERU1.U &= ~0xff;
           SRC_SCU_SCU_ERU1.U |= 0x20;

           SRC_SCU_SCU_ERU1.U |= 1<<SRE_IDX;
           SRC_SCU_SCU_ERU1.U &= ~(0x3 <<TOS_IDX);
}

void send(unsigned char X)
{

  for (int i = 8; i >= 1; i--)
  {
    if (X & 0x80)
    {
      //digitalWrite(_DIO, HIGH);
      IfxPort_setPinHigh(DIO.port, DIO.pinIndex);
    }
    else
    {
//      digitalWrite(_DIO, LOW);
      IfxPort_setPinLow(DIO.port, DIO.pinIndex);
    }
    X <<= 1;
//    digitalWrite(_SCLK, LOW);
//    digitalWrite(_SCLK, HIGH);
    IfxPort_setPinLow(SCLK.port, SCLK.pinIndex);
    IfxPort_setPinHigh(SCLK.port, SCLK.pinIndex);

  }
}

void send_port(unsigned char X, unsigned char port)
{
  send(X);
  send(port);
//  digitalWrite(_RCLK, LOW);
//  digitalWrite(_RCLK, HIGH);
  IfxPort_setPinLow(RCLK.port, RCLK.pinIndex);
  IfxPort_setPinHigh(RCLK.port, RCLK.pinIndex);
}

void AppTask1ms(void)
{
    stTestCnt.u32nuCnt1ms++;
}

void AppTask10ms(void)
{
    stTestCnt.u32nuCnt10ms++;
    //ADC Test
    adcResult = Driver_Adc0_DataObtain();
    Driver_Adc0_ConvStart();
}

void AppTask100ms(void)
{
    stTestCnt.u32nuCnt100ms++;
    IfxPort_togglePin(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);
}

void AppTask1000ms(void)
{
    static int flag = 0;
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
    stTestCnt.u32nuCnt1000ms++;

}

void AppScheduling(void)
{
    if(stSchedulingInfo.u8nuScheduling1msFlag == 1u)
    {
        stSchedulingInfo.u8nuScheduling1msFlag = 0u;
        AppTask1ms();

        if(stSchedulingInfo.u8nuScheduling10msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling10msFlag = 0u;
            AppTask10ms();
        }

        if(stSchedulingInfo.u8nuScheduling100msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling100msFlag = 0u;
            AppTask100ms();
        }
        if(stSchedulingInfo.u8nuScheduling1000msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling1000msFlag = 0u;
            AppTask1000ms();
        }
    }
}
```
✅ 위의코드로 어차피 확인만 할거라 코드를 짧게 줄여봤다.   
```c
// STM+FND

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

#include "Driver_Stm.h"
#include "Driver_Adc.h"

//GPIO related
#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1
#define PCn_0_IDX 3

#define STOP 0
#define RUNNING 1

#define SCLK IfxPort_P00_0
#define RCLK IfxPort_P00_1
#define DIO IfxPort_P00_2

// ERU related
#define EXIS0_IDX 4
#define FEN0_IDX 8
#define EIEN0_IDX 11
#define INP0_IDX 12
#define IGP0_IDX 14

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11

uint32_t adcResult = 0;

typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

// Task scheduling related
void AppScheduling(void);
void AppTask1ms(void);
void AppTask10ms(void);
void AppTask100ms(void);
void AppTask1000ms(void);


/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void send(unsigned char X);
void send_port(unsigned char X, unsigned char port);

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


    Driver_Stm_Init();
    Driver_Adc_Init();

    while(1)
    {
        AppScheduling();
    }
    return (1);
}

void AppTask1ms(void)
{
    stTestCnt.u32nuCnt1ms++;
}

void AppTask10ms(void)
{
    stTestCnt.u32nuCnt10ms++;
    //ADC Test
    adcResult = Driver_Adc0_DataObtain();
    Driver_Adc0_ConvStart();
}

void AppTask100ms(void)
{
    stTestCnt.u32nuCnt100ms++;
    IfxPort_togglePin(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);
}

void AppTask1000ms(void)
{
    static int flag = 0;
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
    stTestCnt.u32nuCnt1000ms++;
    flag++;
}

void AppScheduling(void)
{
    if(stSchedulingInfo.u8nuScheduling1msFlag == 1u)
    {
        stSchedulingInfo.u8nuScheduling1msFlag = 0u;
        AppTask1ms();

        if(stSchedulingInfo.u8nuScheduling10msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling10msFlag = 0u;
            AppTask10ms();
        }

        if(stSchedulingInfo.u8nuScheduling100msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling100msFlag = 0u;
            AppTask100ms();
        }
        if(stSchedulingInfo.u8nuScheduling1000msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling1000msFlag = 0u;
            AppTask1000ms();
        }
    }
}
```
## ADC 결과값에 따른 RGB LED 제어
```c
// STM+FND

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

#include "Driver_Stm.h"
#include "Driver_Adc.h"

//GPIO related
#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1
#define PCn_0_IDX 3
#define PCn_3_IDX 27 // .3
#define P3_IDX 3
#define PCn_5_IDX 11 // .5
#define P5_IDX 5
#define PCn_7_IDX 27
#define P7_IDX 7

#define STOP 0
#define RUNNING 1

#define SCLK IfxPort_P00_0
#define RCLK IfxPort_P00_1
#define DIO IfxPort_P00_2

#define LEDR IfxPort_P02_7
#define LEDG IfxPort_P10_5
#define LEDB IfxPort_P10_3

// ERU related
#define EXIS0_IDX 4
#define FEN0_IDX 8
#define EIEN0_IDX 11
#define INP0_IDX 12
#define IGP0_IDX 14

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11

uint32_t adcResult = 0;

typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

// Task scheduling related
void AppScheduling(void);
void AppTask1ms(void);
void AppTask10ms(void);
void AppTask100ms(void);
void AppTask1000ms(void);


/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initGPIO(void);
void initERU(void);
void send(unsigned char X);
void send_port(unsigned char X, unsigned char port);

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


    initGPIO();

    Driver_Stm_Init();
    Driver_Adc_Init();

    while(1)
    {
        AppScheduling();
    }
    return (1);
}

void initGPIO(void){
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    IfxPort_setPinModeOutput(SCLK.port, SCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
//    P00_IOCR0.U &= ~(0x1F << PCn_0_IDX);
//    P00_IOCR0.U |= 0x10 << PCn_0_IDX;
    IfxPort_setPinModeOutput(RCLK.port, RCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(DIO.port, DIO.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(LEDR.port, LEDR.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(LEDG.port, LEDG.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(LEDB.port, LEDB.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
}

void initERU(void){
    // set EICR
       SCU_EICR1.U &= ~(0x7 << EXIS0_IDX);
       SCU_EICR1.U |= 0x1 << EXIS0_IDX;

       SCU_EICR1.U |= 1 << FEN0_IDX;
       SCU_EICR1.U |= 1 << EIEN0_IDX;

       SCU_EICR1.U &= ~(0x7 << INP0_IDX);

       // set IGCR
       SCU_IGCR0.U &= ~(0x3 << IGP0_IDX );
       SCU_IGCR0.U |= 0x1 << IGP0_IDX;

       // set SCUERU
       SRC_SCU_SCU_ERU0.U &= ~0xff;
       SRC_SCU_SCU_ERU0.U |= 0x10;

       SRC_SCU_SCU_ERU0.U |= 1<<SRE_IDX;
       SRC_SCU_SCU_ERU0.U &= ~(0x3 <<TOS_IDX);

       // set EICR
           SCU_EICR1.U &= ~(0x7 << EXIS0_IDX+16);
           SCU_EICR1.U |= 0x2 << EXIS0_IDX+16;

           SCU_EICR1.U |= 1 << FEN0_IDX+16;
           SCU_EICR1.U |= 1 << EIEN0_IDX+16;

           SCU_EICR1.U &= ~(0x7 << INP0_IDX+16);
           SCU_EICR1.U |= 0x1 << INP0_IDX+16;

           // set IGCR
           SCU_IGCR0.U &= ~(0x3 << IGP0_IDX+16 );
           SCU_IGCR0.U |= 0x1 << IGP0_IDX+16;

           // set SCUERU
           SRC_SCU_SCU_ERU1.U &= ~0xff;
           SRC_SCU_SCU_ERU1.U |= 0x20;

           SRC_SCU_SCU_ERU1.U |= 1<<SRE_IDX;
           SRC_SCU_SCU_ERU1.U &= ~(0x3 <<TOS_IDX);
}

void send(unsigned char X)
{

  for (int i = 8; i >= 1; i--)
  {
    if (X & 0x80)
    {
      //digitalWrite(_DIO, HIGH);
      IfxPort_setPinHigh(DIO.port, DIO.pinIndex);
    }
    else
    {
//      digitalWrite(_DIO, LOW);
      IfxPort_setPinLow(DIO.port, DIO.pinIndex);
    }
    X <<= 1;
//    digitalWrite(_SCLK, LOW);
//    digitalWrite(_SCLK, HIGH);
    IfxPort_setPinLow(SCLK.port, SCLK.pinIndex);
    IfxPort_setPinHigh(SCLK.port, SCLK.pinIndex);

  }
}

void send_port(unsigned char X, unsigned char port)
{
  send(X);
  send(port);
//  digitalWrite(_RCLK, LOW);
//  digitalWrite(_RCLK, HIGH);
  IfxPort_setPinLow(RCLK.port, RCLK.pinIndex);
  IfxPort_setPinHigh(RCLK.port, RCLK.pinIndex);
}

void AppTask1ms(void)
{
    stTestCnt.u32nuCnt1ms++;
}

void AppTask10ms(void)
{
    stTestCnt.u32nuCnt10ms++;
    //ADC Test
    adcResult = Driver_Adc0_DataObtain();
    Driver_Adc0_ConvStart();
}

void AppTask100ms(void)
{
    stTestCnt.u32nuCnt100ms++;
    if(adcResult < 1300) {
        IfxPort_setPinHigh(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinLow(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinLow(LEDB.port, LEDB.pinIndex);
    }
    else if(adcResult >= 1300 && adcResult < 2600) {
        IfxPort_setPinLow(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinHigh(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinLow(LEDB.port, LEDB.pinIndex);
    }
    if(adcResult >= 2600) {
        IfxPort_setPinLow(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinLow(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinHigh(LEDB.port, LEDB.pinIndex);
    }
}

void AppTask1000ms(void)
{
    stTestCnt.u32nuCnt1000ms++;
}

void AppScheduling(void)
{
    if(stSchedulingInfo.u8nuScheduling1msFlag == 1u)
    {
        stSchedulingInfo.u8nuScheduling1msFlag = 0u;
        AppTask1ms();

        if(stSchedulingInfo.u8nuScheduling10msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling10msFlag = 0u;
            AppTask10ms();
        }

        if(stSchedulingInfo.u8nuScheduling100msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling100msFlag = 0u;
            AppTask100ms();
        }
        if(stSchedulingInfo.u8nuScheduling1000msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling1000msFlag = 0u;
            AppTask1000ms();
        }
    }
}
```

✅ 짧게 줄인 코드   
```c
// STM+FND

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

#include "Driver_Stm.h"
#include "Driver_Adc.h"

//GPIO related
#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1
#define PCn_0_IDX 3

#define STOP 0
#define RUNNING 1

#define SCLK IfxPort_P00_0
#define RCLK IfxPort_P00_1
#define DIO IfxPort_P00_2

// ERU related
#define EXIS0_IDX 4
#define FEN0_IDX 8
#define EIEN0_IDX 11
#define INP0_IDX 12
#define IGP0_IDX 14

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11

#define LEDR IfxPort_P02_7
#define LEDG IfxPort_P10_5
#define LEDB IfxPort_P10_3

uint32_t adcResult = 0;

typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

// Task scheduling related
void AppScheduling(void);
void AppTask1ms(void);
void AppTask10ms(void);
void AppTask100ms(void);
void AppTask1000ms(void);


/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initGPIO(void);
void send(unsigned char X);
void send_port(unsigned char X, unsigned char port);

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

    initGPIO();
    Driver_Stm_Init();
    Driver_Adc_Init();

    while(1)
    {
        AppScheduling();
    }
    return (1);
}
void initGPIO(void){
    IfxPort_setPinModeOutput(LEDR.port, LEDR.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(LEDG.port, LEDG.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(LEDB.port, LEDB.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
}
void AppTask1ms(void)
{
    stTestCnt.u32nuCnt1ms++;
}

void AppTask10ms(void)
{
    stTestCnt.u32nuCnt10ms++;
    //ADC Test
    adcResult = Driver_Adc0_DataObtain();
    Driver_Adc0_ConvStart();
}

void AppTask100ms(void)
{
    stTestCnt.u32nuCnt100ms++;
    if(adcResult < 1300) {
        IfxPort_setPinHigh(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinLow(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinLow(LEDB.port, LEDB.pinIndex);
    }
    else if(adcResult >= 1300 && adcResult < 2600) {
        IfxPort_setPinLow(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinHigh(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinLow(LEDB.port, LEDB.pinIndex);
    }
    if(adcResult >= 2600) {
        IfxPort_setPinLow(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinLow(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinHigh(LEDB.port, LEDB.pinIndex);
    }
}

void AppTask1000ms(void)
{
    static int flag = 0;
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
    stTestCnt.u32nuCnt1000ms++;
    flag++;
}

void AppScheduling(void)
{
    if(stSchedulingInfo.u8nuScheduling1msFlag == 1u)
    {
        stSchedulingInfo.u8nuScheduling1msFlag = 0u;
        AppTask1ms();

        if(stSchedulingInfo.u8nuScheduling10msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling10msFlag = 0u;
            AppTask10ms();
        }

        if(stSchedulingInfo.u8nuScheduling100msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling100msFlag = 0u;
            AppTask100ms();
        }
        if(stSchedulingInfo.u8nuScheduling1000msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling1000msFlag = 0u;
            AppTask1000ms();
        }
    }
}
```

## 조도센서
```c
Driver_Adc.c 파일에서 chnIx가 두 번 나오는데 7을 6으로 바뀌주면 된다.
7->6 : 가변저항->조도센서
그리고 가려지면 빛 값이 낮아져서 2600미만일 때 레드, 아닐 땐 블루로 코딩하면 된다.
여담으로 내 조도센서는 고장나서 잘 안 됐다. 팀원분과 바꿔서 해보니 잘 됐다. ㅋㅋㅋ
```

# PWM
## PWM이란?
✅ Pulse Width Modulation   
출력의 너비를 조정한다. 임베디드에서 출력을 제어할 때 자주 사용한다.   
   
✅ P10.2가 1이면 켜지고 0이면 꺼지는데 그럼, 밝기는 어떻게 제어하게?   
전압은 2.7V로 고정이고, 밝기는 전류에 의해 결정된다. 전류는 단위시간 당 흐르는 전자의 양으로 정의된다.   
   
주기 내에서 HIGH 구간의 비율을 달리하여 나타내는 디지털 신호.   
HIGH와 LOW구간에 개별적으로 반응하면 안 됨. (PWM 신호 주파수는 일반적으로 높음. 사람이 반응하게 하면 안 됨.   
아날로그 신호와유사한 효과를 얻을 수 있음.   
   
✅ 듀티 사이클: 1인 구간의 비율   

## 세팅
✅ file -> import -> infinion -> aurix 머시기 -> shield 검색 -> Blinky 머시기 TC275 설치 -> 실행   

# 실습
## 파란색 LED 밝기 조절
```c
// STM+FND

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

#include "Driver_Stm.h"
#include "Driver_Adc.h"

//GPIO related
#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1
#define PCn_0_IDX 3

#define STOP 0
#define RUNNING 1

#define SCLK IfxPort_P00_0
#define RCLK IfxPort_P00_1
#define DIO IfxPort_P00_2

// ERU related
#define EXIS0_IDX 4
#define FEN0_IDX 8
#define EIEN0_IDX 11
#define INP0_IDX 12
#define IGP0_IDX 14

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11

#define LEDR IfxPort_P02_7
#define LEDG IfxPort_P10_5
#define LEDB IfxPort_P10_3
#define SLEDR IfxPort_P10_1
#define SLEDB IfxPort_P10_2
uint32_t adcResult = 0;

typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

// Task scheduling related
void AppScheduling(void);
void AppTask1ms(void);
void AppTask10ms(void);
void AppTask100ms(void);
void AppTask1000ms(void);


/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

IfxCpu_syncEvent g_cpuSyncEvent = 0;

void initGPIO(void);
void send(unsigned char X);
void send_port(unsigned char X, unsigned char port);

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

    initGPIO();
    Driver_Stm_Init();
    Driver_Adc_Init();

    while(1)
    {
        AppScheduling();
    }
    return (1);
}
void initGPIO(void){
    //삼원색 LED
    IfxPort_setPinModeOutput(LEDR.port, LEDR.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(LEDG.port, LEDG.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(LEDB.port, LEDB.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    //파빨
    IfxPort_setPinModeOutput(SLEDR.port, SLEDR.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(SLEDB.port, SLEDB.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
}
void AppTask1ms(void)
{
    stTestCnt.u32nuCnt1ms++;
}

void AppTask10ms(void)
{
    stTestCnt.u32nuCnt10ms++;
    //ADC Test
    adcResult = Driver_Adc0_DataObtain();
    Driver_Adc0_ConvStart();
}

void AppTask100ms(void)
{
    stTestCnt.u32nuCnt100ms++;
    if(adcResult < 2600) {
        IfxPort_setPinLow(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinLow(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinHigh(LEDB.port, LEDB.pinIndex);
    }
    else if(adcResult >= 2600) {
        IfxPort_setPinHigh(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinLow(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinLow(LEDB.port, LEDB.pinIndex);
    }
}

void AppTask1000ms(void)
{
    static int flag = 0;
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
    stTestCnt.u32nuCnt1000ms++;
    flag++;
}

void AppScheduling(void)
{
    if(stSchedulingInfo.u8nuScheduling1msFlag == 1u)
    {
        stSchedulingInfo.u8nuScheduling1msFlag = 0u;
        AppTask1ms();

        if(stSchedulingInfo.u8nuScheduling10msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling10msFlag = 0u;
            AppTask10ms();
        }

        if(stSchedulingInfo.u8nuScheduling100msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling100msFlag = 0u;
            AppTask100ms();
        }
        if(stSchedulingInfo.u8nuScheduling1000msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling1000msFlag = 0u;
            AppTask1000ms();
        }
    }
}
```

## 가변저항에 따른 빨간 LED(P10) 밝기 조절
```c
fadeLED(adcREsult); 를 main의 while에서 쓸 거면 waitTime을 같이 써야한다.
waitTime(ticksFor10ms);
왜냐면 보통 while문 한줄이 5클락정도 걸리는데, fadeLED는 신호를 보내기까지 5만클락이 필요하다.
따라서 fadeLED가 신호를 미처 보내기전에 다시 또 보내야하는 상황이 발생해서 제대로 동작하지 않는다.
```   
✅ Cpu0_Main.c   
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
 /*\title GTM TOM PWM generation
 * \abstract GTM TOM is used to generate a PWM signal, which is driving the intensity of an LED.
 * \description The LED is driven by pin 2 of the port 10. The state of the pin is controlled by the PWM signal
 *              generated by the TOM timer of GTM.
 *
 * \name GTM_TOM_PWM_1_KIT_TC275_SB
 * \version V1.0.1
 * \board hitex ShieldBuddy, KIT_AURIX_TC275_ARD_SB, TC27xTP_D-Step
 * \keywords AURIX, GTM, GTM_TOM_PWM_1, PWM, general timer, timer
 * \documents https://www.infineon.com/aurix-expert-training/Infineon-AURIX_GTM_TOM_PWM_1_KIT_TC275_SB-TR-v01_00_01-EN.pdf
 * \documents https://www.infineon.com/aurix-expert-training/TC27D_iLLD_UM_1_0_1_12_0.chm
 * \lastUpdated 2020-12-18
 *********************************************************************************************************************/
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"
#include "GTM_TOM_PWM.h"
#include "Bsp.h"

/*********************************************************************************************************************/
/*------------------------------------------------------Macros-------------------------------------------------------*/
/*********************************************************************************************************************/
#define WAIT_TIME   10              /* Number of milliseconds to wait between each duty cycle change                */

IfxCpu_syncEvent g_cpuSyncEvent = 0;

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
    
    /* Initialize a time variable */
    Ifx_TickTime ticksFor10ms = IfxStm_getTicksFromMilliseconds(BSP_DEFAULT_TIMER, WAIT_TIME);

    /* Initialize GTM TOM module */
    initGtmTomPwm();
    Driver_Adc_Init();
    while(1)
    {
        fadeLED();                  /* Change the intensity of the LED  */
        waitTime(ticksFor10ms);     /* Delay of 10ms                    */
    }
    return (1);
}
```   
✅ GTM_TOM_PWM.c   
```c
/**********************************************************************************************************************
 * \file GTM_TOM_PWM.c
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

/*********************************************************************************************************************/
/*-----------------------------------------------------Includes------------------------------------------------------*/
/*********************************************************************************************************************/
#include "GTM_TOM_PWM.h"
#include "Ifx_Types.h"
#include "IfxGtm_Tom_Pwm.h"

#include "Driver_Adc.h"
/*********************************************************************************************************************/
/*------------------------------------------------------Macros-------------------------------------------------------*/
/*********************************************************************************************************************/
#define ISR_PRIORITY_TOM    20                                      /* Interrupt priority number                    */
//#define LED                 IfxGtm_TOM0_2_TOUT104_P10_2_OUT         /* LED which will be driven by the PWM          */
#define LED                 IfxGtm_TOM2_9_TOUT103_P10_1_OUT         /* RED LED                                      */
#define PWM_PERIOD          50000                                   /* PWM period for the TOM                       */
#define FADE_STEP           PWM_PERIOD / 100                        /* PWM duty cycle for the TOM                   */

/*********************************************************************************************************************/
/*-------------------------------------------------Global variables--------------------------------------------------*/
/*********************************************************************************************************************/
IfxGtm_Tom_Pwm_Config g_tomConfig;                                  /* Timer configuration structure                */
IfxGtm_Tom_Pwm_Driver g_tomDriver;                                  /* Timer Driver structure                       */
uint32 g_fadeValue = 0;                                             /* Fade value, starting from 0                  */
sint8 g_fadeDir = 1;                                                /* Fade direction variable                      */

/*********************************************************************************************************************/
/*-----------------------------------------------Function Prototypes-------------------------------------------------*/
/*********************************************************************************************************************/
void setDutyCycle(uint32 dutyCycle);                                /* Function to set the duty cycle of the PWM    */

/*********************************************************************************************************************/
/*--------------------------------------------Function Implementations-----------------------------------------------*/
/*********************************************************************************************************************/
/* This function initializes the TOM */
void initGtmTomPwm(void)
{
    IfxGtm_enable(&MODULE_GTM);                                     /* Enable GTM                                   */

    IfxGtm_Cmu_enableClocks(&MODULE_GTM, IFXGTM_CMU_CLKEN_FXCLK);   /* Enable the FXU clock                         */

    /* Initialize the configuration structure with default parameters */
    IfxGtm_Tom_Pwm_initConfig(&g_tomConfig, &MODULE_GTM);

    g_tomConfig.tom = LED.tom;                                      /* Select the TOM depending on the LED          */
    g_tomConfig.tomChannel = LED.channel;                           /* Select the channel depending on the LED      */
    g_tomConfig.period = PWM_PERIOD;                                /* Set the timer period                         */
    g_tomConfig.pin.outputPin = &LED;                               /* Set the LED port pin as output               */
    g_tomConfig.synchronousUpdateEnabled = TRUE;                    /* Enable synchronous update                    */

    IfxGtm_Tom_Pwm_init(&g_tomDriver, &g_tomConfig);                /* Initialize the GTM TOM                       */
    IfxGtm_Tom_Pwm_start(&g_tomDriver, TRUE);                       /* Start the PWM                                */
}

/* This function creates the fade effect for the LED */
void fadeLED(void)
{
//    if((g_fadeValue + FADE_STEP) >= PWM_PERIOD)
//    {
//        g_fadeDir = -1;                                             /* Set the direction of the fade                */
//    }
//    else if((g_fadeValue - FADE_STEP) <= 0)
//    {
//        g_fadeDir = 1;                                              /* Set the direction of the fade                */
//    }
//    g_fadeValue += g_fadeDir * FADE_STEP;                           /* Calculation of the new duty cycle            */
//    setDutyCycle(g_fadeValue);                                      /* Set the duty cycle of the PWM                */
    g_fadeValue = Driver_Adc0_DataObtain();
    Driver_Adc0_ConvStart();
    g_fadeValue = (uint32)((g_fadeValue / 4095.0) * 50000); // 가변저항은 최대값이 4095고 LED밝기는 5만까지 변하게 할 수 있기 때문이다.
    setDutyCycle(g_fadeValue);                                      /* Set the duty cycle of the PWM                */
}

/* This function sets the duty cycle of the PWM */
void setDutyCycle(uint32 dutyCycle)
{
    g_tomConfig.dutyCycle = dutyCycle;                              /* Change the value of the duty cycle           */
    IfxGtm_Tom_Pwm_init(&g_tomDriver, &g_tomConfig);                /* Re-initialize the PWM                        */
}
```   
✅ Driver_Adc.h   
```c
#ifndef DRIVER_ADC
#define DRIVER_ADC

/***********************************************************************/
/*Include*/ 
/***********************************************************************/
#include "Ifx_Types.h"
#include "IfxVadc.h"
#include "IfxVadc_Adc.h"
typedef signed char int8_t;
typedef unsigned char uint8_t;
typedef signed short int16_t;
typedef unsigned short uint16_t;
typedef signed long int32_t;
typedef unsigned long uint32_t;
typedef float float32_t;
typedef double float64_t;
/***********************************************************************/
/*Typedef*/ 
/***********************************************************************/
typedef struct
{
    IfxVadc_Adc vadc; /* VADC handle */
    IfxVadc_Adc_Group adcGroup;
} App_VadcAutoScan;


/***********************************************************************/
/*Define*/ 
/***********************************************************************/

/***********************************************************************/
/*External Variable*/ 
/***********************************************************************/
IFX_EXTERN App_VadcAutoScan g_VadcAutoScan;


/***********************************************************************/
/*Global Function Prototype*/ 
/***********************************************************************/
extern void Driver_Adc_Init(void);
extern void Driver_Adc0_ConvStart(void);
extern uint32_t Driver_Adc0_DataObtain(void);

#endif /* DRIVER_STM */
```

# 수동부저
## PWM이란?
✅ PWM (Pulse Width Modulation, 펄스 폭 변조)   
PWM은 디지털 신호를 사용하여 아날로그 동작을 제어하는 기술이다. 쉽게 말하면, 신호를 빠르게 켜고 끄는 것으로 출력 세기를 조절하는 방법이다. LED 밝기나 모터 속도를 조절할 때 많이 사용한다.   
## 수동부저란?
✅ 수동부저 (Passive buzzer)   
신호의 진동으로 소리가 나는 소자. 주파수에 따라 음의 높낮이가 결정된다.   

# 실습
## fadeLED
import 할 때 blinky_led_1_kit_tc275_5b를 다운 받아서 하면 된다. while문에 fadeLED();와 waitTime(ticksFor10ms);만 들어간다. 그럼 파란색 LED가 서서히 어두워지다 서서히 밝아진다.

## Adc에 따른 LED제어
✅ Cpu0_Main.c   
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

#include "Driver_Stm.h"
#include "Driver_Adc.h"
#include "GTM_TOM_PWM.h"
#include "Bsp.h"

//GPIO related
#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1
#define PCn_0_IDX 3

#define STOP 0
#define RUNNING 1

#define SCLK IfxPort_P00_0
#define RCLK IfxPort_P00_1
#define DIO IfxPort_P00_2

#define LEDR IfxPort_P02_7
#define LEDG IfxPort_P10_5
#define LEDB IfxPort_P10_3
// ERU related
#define EXIS0_IDX 4
#define FEN0_IDX 8
#define EIEN0_IDX 11
#define INP0_IDX 12
#define IGP0_IDX 14

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11

typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

// Task scheduling related
void AppScheduling(void);
void AppTask1ms(void);
void AppTask10ms(void);
void AppTask100ms(void);
void AppTask1000ms(void);


/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

IfxCpu_syncEvent g_cpuSyncEvent = 0;

uint8_t _LED_0F[10] = {0xC0,0xf9,0xa4,0xb0,0x99,0x92,0x82,0xf8,0x80,0x90};
int n = 0;
uint8_t state = STOP;
uint32_t adcResult = 0;

uint8_t n1, n2, n3, n4;

IFX_INTERRUPT(ISR0, 0, 0x10);
void ISR0 (void){
    state = (state==STOP)? RUNNING: STOP;
    P10_OMR.U = 0x20002;
}

IFX_INTERRUPT(ISR1, 0, 0x20);
void ISR1 (void){
    n=0;
}
void initGPIO(void);
void initERU(void);
void send(unsigned char X);
void send_port(unsigned char X, unsigned char port);

int core0_main(void)
{
    IfxCpu_enableInterrupts();
    
    /* !!WATCHDOG0 AND SAFETY WATCHDOG ARE DISABLED HERE!!
     * Enable the watchdogs and service them periodically if it is required
     */
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    Ifx_TickTime ticksFor10ms = IfxStm_getTicksFromMilliseconds(BSP_DEFAULT_TIMER, 10);

    /* Wait for CPU sync event */
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent, 1);

    initGPIO();
    initERU();
    Driver_Stm_Init();
    Driver_Adc_Init();
    initGtmTomPwm();

    while(1)
    {
        AppScheduling();

        n1 = (uint8_t) adcResult%10;
        n2 = (uint8_t) (adcResult%100)/10;
        n3 = (uint8_t) (adcResult%1000)/100;
        n4 = (uint8_t) (adcResult%10000)/1000;

        send_port(_LED_0F[n1],0x1);
        send_port(_LED_0F[n2],0x2);
        send_port(_LED_0F[n3]&0x7f,0x4);
        send_port(_LED_0F[n4],0x8);

        fadeLED(adcResult);
        waitTime(ticksFor10ms);
    }
    return (1);
}

void initGPIO(void){
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    IfxPort_setPinModeOutput(SCLK.port, SCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
//    P00_IOCR0.U &= ~(0x1F << PCn_0_IDX);
//    P00_IOCR0.U |= 0x10 << PCn_0_IDX;
    IfxPort_setPinModeOutput(RCLK.port, RCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(DIO.port, DIO.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);

    IfxPort_setPinModeOutput(LEDR.port, LEDR.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(LEDG.port, LEDG.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(LEDB.port, LEDB.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);

}

void initERU(void){
    // set EICR
       SCU_EICR1.U &= ~(0x7 << EXIS0_IDX);
       SCU_EICR1.U |= 0x1 << EXIS0_IDX;

       SCU_EICR1.U |= 1 << FEN0_IDX;
       SCU_EICR1.U |= 1 << EIEN0_IDX;

       SCU_EICR1.U &= ~(0x7 << INP0_IDX);

       // set IGCR
       SCU_IGCR0.U &= ~(0x3 << IGP0_IDX );
       SCU_IGCR0.U |= 0x1 << IGP0_IDX;

       // set SCUERU
       SRC_SCU_SCU_ERU0.U &= ~0xff;
       SRC_SCU_SCU_ERU0.U |= 0x10;

       SRC_SCU_SCU_ERU0.U |= 1<<SRE_IDX;
       SRC_SCU_SCU_ERU0.U &= ~(0x3 <<TOS_IDX);

       // set EICR
           SCU_EICR1.U &= ~(0x7 << EXIS0_IDX+16);
           SCU_EICR1.U |= 0x2 << EXIS0_IDX+16;

           SCU_EICR1.U |= 1 << FEN0_IDX+16;
           SCU_EICR1.U |= 1 << EIEN0_IDX+16;

           SCU_EICR1.U &= ~(0x7 << INP0_IDX+16);
           SCU_EICR1.U |= 0x1 << INP0_IDX+16;

           // set IGCR
           SCU_IGCR0.U &= ~(0x3 << IGP0_IDX+16 );
           SCU_IGCR0.U |= 0x1 << IGP0_IDX+16;

           // set SCUERU
           SRC_SCU_SCU_ERU1.U &= ~0xff;
           SRC_SCU_SCU_ERU1.U |= 0x20;

           SRC_SCU_SCU_ERU1.U |= 1<<SRE_IDX;
           SRC_SCU_SCU_ERU1.U &= ~(0x3 <<TOS_IDX);
}

void send(unsigned char X)
{

  for (int i = 8; i >= 1; i--)
  {
    if (X & 0x80)
    {
      //digitalWrite(_DIO, HIGH);
      IfxPort_setPinHigh(DIO.port, DIO.pinIndex);
    }
    else
    {
//      digitalWrite(_DIO, LOW);
      IfxPort_setPinLow(DIO.port, DIO.pinIndex);
    }
    X <<= 1;
//    digitalWrite(_SCLK, LOW);
//    digitalWrite(_SCLK, HIGH);
    IfxPort_setPinLow(SCLK.port, SCLK.pinIndex);
    IfxPort_setPinHigh(SCLK.port, SCLK.pinIndex);

  }
}

void send_port(unsigned char X, unsigned char port)
{
  send(X);
  send(port);
//  digitalWrite(_RCLK, LOW);
//  digitalWrite(_RCLK, HIGH);
  IfxPort_setPinLow(RCLK.port, RCLK.pinIndex);
  IfxPort_setPinHigh(RCLK.port, RCLK.pinIndex);
}

void AppTask1ms(void)
{
    stTestCnt.u32nuCnt1ms++;
}

void AppTask10ms(void)
{
    stTestCnt.u32nuCnt10ms++;
    adcResult = Driver_Adc0_DataObtain();


    Driver_Adc0_ConvStart();
}

void AppTask100ms(void)
{
    stTestCnt.u32nuCnt100ms++;
    IfxPort_togglePin(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);

    if(adcResult<2700){
        IfxPort_setPinHigh(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinLow(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinLow(LEDB.port, LEDB.pinIndex);
    }
    else{
        IfxPort_setPinLow(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinLow(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinHigh(LEDB.port, LEDB.pinIndex);
    }

}

void AppTask1000ms(void)
{
    static int flag = 0;
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
    stTestCnt.u32nuCnt1000ms++;

}

void AppScheduling(void)
{
    if(stSchedulingInfo.u8nuScheduling1msFlag == 1u)
    {
        stSchedulingInfo.u8nuScheduling1msFlag = 0u;
        AppTask1ms();

        if(stSchedulingInfo.u8nuScheduling10msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling10msFlag = 0u;
            AppTask10ms();
        }

        if(stSchedulingInfo.u8nuScheduling100msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling100msFlag = 0u;
            AppTask100ms();
        }
        if(stSchedulingInfo.u8nuScheduling1000msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling1000msFlag = 0u;
            AppTask1000ms();
        }
    }
}
```
   
✅ GTM_TOM_PWM.h   
```c
/**********************************************************************************************************************
 * \file GTM_TOM_PWM.h
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

#ifndef GTM_TOM_PWM_H_
#define GTM_TOM_PWM_H_

/*********************************************************************************************************************/
/*-----------------------------------------------Function Prototypes-------------------------------------------------*/
/*********************************************************************************************************************/
void initGtmTomPwm(void);
void fadeLED(unsigned int adcValue);

#endif /* GTM_TOM_PWM_H_ */
```
   
✅ GTM_TOM_PWM.c   
```c
/**********************************************************************************************************************
 * \file GTM_TOM_PWM.c
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

/*********************************************************************************************************************/
/*-----------------------------------------------------Includes------------------------------------------------------*/
/*********************************************************************************************************************/
#include "GTM_TOM_PWM.h"
#include "Ifx_Types.h"
#include "IfxGtm_Tom_Pwm.h"

/*********************************************************************************************************************/
/*------------------------------------------------------Macros-------------------------------------------------------*/
/*********************************************************************************************************************/
#define ISR_PRIORITY_TOM    20                                      /* Interrupt priority number                    */
#define LED                 IfxGtm_TOM2_9_TOUT103_P10_1_OUT         /* LED which will be driven by the PWM          */
#define PWM_PERIOD          50000                                   /* PWM period for the TOM                       */
#define FADE_STEP           PWM_PERIOD / 100                        /* PWM duty cycle for the TOM                   */

/*********************************************************************************************************************/
/*-------------------------------------------------Global variables--------------------------------------------------*/
/*********************************************************************************************************************/
IfxGtm_Tom_Pwm_Config g_tomConfig;                                  /* Timer configuration structure                */
IfxGtm_Tom_Pwm_Driver g_tomDriver;                                  /* Timer Driver structure                       */
uint32 g_fadeValue = 0;                                             /* Fade value, starting from 0                  */
sint8 g_fadeDir = 1;                                                /* Fade direction variable                      */

/*********************************************************************************************************************/
/*-----------------------------------------------Function Prototypes-------------------------------------------------*/
/*********************************************************************************************************************/
void setDutyCycle(uint32 dutyCycle);                                /* Function to set the duty cycle of the PWM    */

/*********************************************************************************************************************/
/*--------------------------------------------Function Implementations-----------------------------------------------*/
/*********************************************************************************************************************/
/* This function initializes the TOM */
void initGtmTomPwm(void)
{
    IfxGtm_enable(&MODULE_GTM);                                     /* Enable GTM                                   */

    IfxGtm_Cmu_enableClocks(&MODULE_GTM, IFXGTM_CMU_CLKEN_FXCLK);   /* Enable the FXU clock                         */

    /* Initialize the configuration structure with default parameters */
    IfxGtm_Tom_Pwm_initConfig(&g_tomConfig, &MODULE_GTM);

    g_tomConfig.tom = LED.tom;                                      /* Select the TOM depending on the LED          */
    g_tomConfig.tomChannel = LED.channel;                           /* Select the channel depending on the LED      */
    g_tomConfig.period = PWM_PERIOD;                                /* Set the timer period                         */
    g_tomConfig.pin.outputPin = &LED;                               /* Set the LED port pin as output               */
    g_tomConfig.synchronousUpdateEnabled = TRUE;                    /* Enable synchronous update                    */

    IfxGtm_Tom_Pwm_init(&g_tomDriver, &g_tomConfig);                /* Initialize the GTM TOM                       */
    IfxGtm_Tom_Pwm_start(&g_tomDriver, TRUE);                       /* Start the PWM                                */
}

/* This function creates the fade effect for the LED */
void fadeLED(unsigned int adcValue)
{
//    if((g_fadeValue + FADE_STEP) >= PWM_PERIOD)
//    {
//        g_fadeDir = -1;                                             /* Set the direction of the fade                */
//    }
//    else if((g_fadeValue - FADE_STEP) <= 0)
//    {
//        g_fadeDir = 1;                                              /* Set the direction of the fade                */
//    }
//    g_fadeValue += g_fadeDir * FADE_STEP;                           /* Calculation of the new duty cycle            */
    g_fadeValue = (uint32)((adcValue/4095.0)*50000);
    setDutyCycle(g_fadeValue);                                      /* Set the duty cycle of the PWM                */
}

/* This function sets the duty cycle of the PWM */
void setDutyCycle(uint32 dutyCycle)
{
    g_tomConfig.dutyCycle = dutyCycle;                              /* Change the value of the duty cycle           */
    IfxGtm_Tom_Pwm_init(&g_tomDriver, &g_tomConfig);                /* Re-initialize the PWM                        */
}
```

## 누르면 도 or 솔
✅ Cpu0_Main.c   
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

#include "Driver_Stm.h"
#include "Driver_Adc.h"
#include "GTM_TOM_PWM.h"
#include "Bsp.h"

//GPIO related
#define PCn_2_IDX 19
#define P2_IDX 2
#define PCn_1_IDX 11
#define P1_IDX 1
#define PCn_0_IDX 3

#define STOP 0
#define RUNNING 1

#define SCLK IfxPort_P00_0
#define RCLK IfxPort_P00_1
#define DIO IfxPort_P00_2

#define LEDR IfxPort_P02_7
#define LEDG IfxPort_P10_5
#define LEDB IfxPort_P10_3
// ERU related
#define EXIS0_IDX 4
#define FEN0_IDX 8
#define EIEN0_IDX 11
#define INP0_IDX 12
#define IGP0_IDX 14

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11

typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

// Task scheduling related
void AppScheduling(void);
void AppTask1ms(void);
void AppTask10ms(void);
void AppTask100ms(void);
void AppTask1000ms(void);


/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

IfxCpu_syncEvent g_cpuSyncEvent = 0;

uint8_t _LED_0F[10] = {0xC0,0xf9,0xa4,0xb0,0x99,0x92,0x82,0xf8,0x80,0x90};
int n = 0;
uint8_t state = STOP;
uint32_t adcResult = 0;

uint8_t n1, n2, n3, n4;

IFX_INTERRUPT(ISR0, 0, 0x10);
void ISR0 (void){
    state = (state==STOP)? RUNNING: STOP;
    P10_OMR.U = 0x20002;
}

IFX_INTERRUPT(ISR1, 0, 0x20);
void ISR1 (void){
    n=0;
}
void initGPIO(void);
void initERU(void);
void send(unsigned char X);
void send_port(unsigned char X, unsigned char port);

int core0_main(void)
{
    IfxCpu_enableInterrupts();
    
    /* !!WATCHDOG0 AND SAFETY WATCHDOG ARE DISABLED HERE!!
     * Enable the watchdogs and service them periodically if it is required
     */
    IfxScuWdt_disableCpuWatchdog(IfxScuWdt_getCpuWatchdogPassword());
    IfxScuWdt_disableSafetyWatchdog(IfxScuWdt_getSafetyWatchdogPassword());
    Ifx_TickTime ticksFor1000ms = IfxStm_getTicksFromMilliseconds(BSP_DEFAULT_TIMER, 1000);

    /* Wait for CPU sync event */
    IfxCpu_emitEvent(&g_cpuSyncEvent);
    IfxCpu_waitEvent(&g_cpuSyncEvent, 1);

    initGPIO();
//    initERU();
    Driver_Stm_Init();
    Driver_Adc_Init();
    initGtmTomPwm();

    while(1)
    {
        AppScheduling();

        n1 = (uint8_t) adcResult%10;
        n2 = (uint8_t) (adcResult%100)/10;
        n3 = (uint8_t) (adcResult%1000)/100;
        n4 = (uint8_t) (adcResult%10000)/1000;

        send_port(_LED_0F[n1],0x1);
        send_port(_LED_0F[n2],0x2);
        send_port(_LED_0F[n3]&0x7f,0x4);
        send_port(_LED_0F[n4],0x8);

//        fadeLED(adcResult);

    }
    return (1);
}

void initGPIO(void){
    P02_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P02_IOCR0.U |= 0x02 << PCn_1_IDX;

    P02_IOCR0.U &= ~(0x1F << PCn_0_IDX);
    P02_IOCR0.U |= 0x02 << PCn_0_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_2_IDX);
    P10_IOCR0.U |= 0x10 << PCn_2_IDX;

    P10_IOCR0.U &= ~(0x1F << PCn_1_IDX);
    P10_IOCR0.U |= 0x10 << PCn_1_IDX;

    IfxPort_setPinModeOutput(SCLK.port, SCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
//    P00_IOCR0.U &= ~(0x1F << PCn_0_IDX);
//    P00_IOCR0.U |= 0x10 << PCn_0_IDX;
    IfxPort_setPinModeOutput(RCLK.port, RCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(DIO.port, DIO.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);

    IfxPort_setPinModeOutput(LEDR.port, LEDR.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(LEDG.port, LEDG.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(LEDB.port, LEDB.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);

}

void initERU(void){
    // set EICR
       SCU_EICR1.U &= ~(0x7 << EXIS0_IDX);
       SCU_EICR1.U |= 0x1 << EXIS0_IDX;

       SCU_EICR1.U |= 1 << FEN0_IDX;
       SCU_EICR1.U |= 1 << EIEN0_IDX;

       SCU_EICR1.U &= ~(0x7 << INP0_IDX);

       // set IGCR
       SCU_IGCR0.U &= ~(0x3 << IGP0_IDX );
       SCU_IGCR0.U |= 0x1 << IGP0_IDX;

       // set SCUERU
       SRC_SCU_SCU_ERU0.U &= ~0xff;
       SRC_SCU_SCU_ERU0.U |= 0x10;

       SRC_SCU_SCU_ERU0.U |= 1<<SRE_IDX;
       SRC_SCU_SCU_ERU0.U &= ~(0x3 <<TOS_IDX);

       // set EICR
           SCU_EICR1.U &= ~(0x7 << EXIS0_IDX+16);
           SCU_EICR1.U |= 0x2 << EXIS0_IDX+16;

           SCU_EICR1.U |= 1 << FEN0_IDX+16;
           SCU_EICR1.U |= 1 << EIEN0_IDX+16;

           SCU_EICR1.U &= ~(0x7 << INP0_IDX+16);
           SCU_EICR1.U |= 0x1 << INP0_IDX+16;

           // set IGCR
           SCU_IGCR0.U &= ~(0x3 << IGP0_IDX+16 );
           SCU_IGCR0.U |= 0x1 << IGP0_IDX+16;

           // set SCUERU
           SRC_SCU_SCU_ERU1.U &= ~0xff;
           SRC_SCU_SCU_ERU1.U |= 0x20;

           SRC_SCU_SCU_ERU1.U |= 1<<SRE_IDX;
           SRC_SCU_SCU_ERU1.U &= ~(0x3 <<TOS_IDX);
}

void send(unsigned char X)
{

  for (int i = 8; i >= 1; i--)
  {
    if (X & 0x80)
    {
      //digitalWrite(_DIO, HIGH);
      IfxPort_setPinHigh(DIO.port, DIO.pinIndex);
    }
    else
    {
//      digitalWrite(_DIO, LOW);
      IfxPort_setPinLow(DIO.port, DIO.pinIndex);
    }
    X <<= 1;
//    digitalWrite(_SCLK, LOW);
//    digitalWrite(_SCLK, HIGH);
    IfxPort_setPinLow(SCLK.port, SCLK.pinIndex);
    IfxPort_setPinHigh(SCLK.port, SCLK.pinIndex);

  }
}

void send_port(unsigned char X, unsigned char port)
{
  send(X);
  send(port);
//  digitalWrite(_RCLK, LOW);
//  digitalWrite(_RCLK, HIGH);
  IfxPort_setPinLow(RCLK.port, RCLK.pinIndex);
  IfxPort_setPinHigh(RCLK.port, RCLK.pinIndex);
}

void AppTask1ms(void)
{
    stTestCnt.u32nuCnt1ms++;
}

void AppTask10ms(void)
{
    stTestCnt.u32nuCnt10ms++;
    adcResult = Driver_Adc0_DataObtain();

    Driver_Adc0_ConvStart();
    if(!(P02_IN.U & 0x1)){
        makeSound(0);
    }
    else if(!(P02_IN.U &(0x1<<1))) {
        makeSound(4);
    }
    else{
        stopPWM();
    }
}

void AppTask100ms(void)
{
    stTestCnt.u32nuCnt100ms++;
    IfxPort_togglePin(IfxPort_P10_1.port, IfxPort_P10_1.pinIndex);

    if(adcResult<2700){
        IfxPort_setPinHigh(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinLow(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinLow(LEDB.port, LEDB.pinIndex);
    }
    else{
        IfxPort_setPinLow(LEDR.port, LEDR.pinIndex);
        IfxPort_setPinLow(LEDG.port, LEDG.pinIndex);
        IfxPort_setPinHigh(LEDB.port, LEDB.pinIndex);
    }

}

void AppTask1000ms(void)
{
    static int flag = 0;
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
    stTestCnt.u32nuCnt1000ms++;

}

void AppScheduling(void)
{
    if(stSchedulingInfo.u8nuScheduling1msFlag == 1u)
    {
        stSchedulingInfo.u8nuScheduling1msFlag = 0u;
        AppTask1ms();

        if(stSchedulingInfo.u8nuScheduling10msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling10msFlag = 0u;
            AppTask10ms();
        }

        if(stSchedulingInfo.u8nuScheduling100msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling100msFlag = 0u;
            AppTask100ms();
        }
        if(stSchedulingInfo.u8nuScheduling1000msFlag == 1u)
        {
            stSchedulingInfo.u8nuScheduling1000msFlag = 0u;
            AppTask1000ms();
        }
    }
}
```
   
✅ GTM_TOM_PWM.h   
```c
/**********************************************************************************************************************
 * \file GTM_TOM_PWM.h
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

#ifndef GTM_TOM_PWM_H_
#define GTM_TOM_PWM_H_

/*********************************************************************************************************************/
/*-----------------------------------------------Function Prototypes-------------------------------------------------*/
/*********************************************************************************************************************/
void initGtmTomPwm(void);
void fadeLED(unsigned int adcValue);
void makeSound(unsigned int sound);
void stopPWM (void);

#endif /* GTM_TOM_PWM_H_ */
```
   
✅ GTM_TOM_PWM.c   
```c
/**********************************************************************************************************************
 * \file GTM_TOM_PWM.c
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

/*********************************************************************************************************************/
/*-----------------------------------------------------Includes------------------------------------------------------*/
/*********************************************************************************************************************/
#include "GTM_TOM_PWM.h"
#include "Ifx_Types.h"
#include "IfxGtm_Tom_Pwm.h"

/*********************************************************************************************************************/
/*------------------------------------------------------Macros-------------------------------------------------------*/
/*********************************************************************************************************************/
#define ISR_PRIORITY_TOM    20                                      /* Interrupt priority number                    */
#define LED                 IfxGtm_TOM2_9_TOUT103_P10_1_OUT         /* LED which will be driven by the PWM          */
#define BUZZER              IfxGtm_TOM0_11_TOUT3_P02_3_OUT
#define PWM_PERIOD          50000                                   /* PWM period for the TOM                       */
#define FADE_STEP           PWM_PERIOD / 100                        /* PWM duty cycle for the TOM                   */

/*********************************************************************************************************************/
/*-------------------------------------------------Global variables--------------------------------------------------*/
/*********************************************************************************************************************/
IfxGtm_Tom_Pwm_Config g_tomConfig;                                  /* Timer configuration structure                */
IfxGtm_Tom_Pwm_Driver g_tomDriver;                                  /* Timer Driver structure                       */
uint32 g_fadeValue = 0;                                             /* Fade value, starting from 0                  */
sint8 g_fadeDir = 1;                                                /* Fade direction variable                      */

/*********************************************************************************************************************/
/*-----------------------------------------------Function Prototypes-------------------------------------------------*/
/*********************************************************************************************************************/
void setDutyCycle(uint32 dutyCycle);                                /* Function to set the duty cycle of the PWM    */

/*********************************************************************************************************************/
/*--------------------------------------------Function Implementations-----------------------------------------------*/
/*********************************************************************************************************************/
/* This function initializes the TOM */
void initGtmTomPwm(void)
{
    IfxGtm_enable(&MODULE_GTM);                                     /* Enable GTM                                   */

    IfxGtm_Cmu_enableClocks(&MODULE_GTM, IFXGTM_CMU_CLKEN_FXCLK);   /* Enable the FXU clock                         */

    /* Initialize the configuration structure with default parameters */
    IfxGtm_Tom_Pwm_initConfig(&g_tomConfig, &MODULE_GTM);

    g_tomConfig.tom = BUZZER.tom;                                      /* Select the TOM depending on the LED          */
    g_tomConfig.tomChannel = BUZZER.channel;                           /* Select the channel depending on the LED      */
    g_tomConfig.period = PWM_PERIOD;                                /* Set the timer period                         */
    g_tomConfig.pin.outputPin = &BUZZER;                               /* Set the LED port pin as output               */
    g_tomConfig.synchronousUpdateEnabled = TRUE;                    /* Enable synchronous update                    */

    IfxGtm_Tom_Pwm_init(&g_tomDriver, &g_tomConfig);                /* Initialize the GTM TOM                       */
    IfxGtm_Tom_Pwm_start(&g_tomDriver, TRUE);                       /* Start the PWM                                */
}

/* This function creates the fade effect for the LED */
void fadeLED(unsigned int adcValue)
{
//    if((g_fadeValue + FADE_STEP) >= PWM_PERIOD)
//    {
//        g_fadeDir = -1;                                             /* Set the direction of the fade                */
//    }
//    else if((g_fadeValue - FADE_STEP) <= 0)
//    {
//        g_fadeDir = 1;                                              /* Set the direction of the fade                */
//    }
//    g_fadeValue += g_fadeDir * FADE_STEP;                           /* Calculation of the new duty cycle            */
    g_fadeValue = (uint32)((adcValue/4095.0)*50000);
    setDutyCycle(g_fadeValue);                                      /* Set the duty cycle of the PWM                */
}

void makeSound(unsigned int sound){
    float fBuzz[7] = {261.6,293.724,329.724,349.309,392.089,440,493.858};
    uint32 uPeriod = (uint32)(10000000/fBuzz[sound]);

    g_tomConfig.period = uPeriod;
    g_tomConfig.dutyCycle = (uPeriod*0.02f);
    IfxGtm_Tom_Pwm_init(&g_tomDriver, &g_tomConfig);
}

/* This function sets the duty cycle of the PWM */
void setDutyCycle(uint32 dutyCycle)
{
    g_tomConfig.dutyCycle = dutyCycle;                              /* Change the value of the duty cycle           */
    IfxGtm_Tom_Pwm_init(&g_tomDriver, &g_tomConfig);                /* Re-initialize the PWM                        */
}

void stopPWM (void){
    IfxGtm_Tom_Pwm_stop(&g_tomDriver, TRUE);
}
```

# DHT11
## 쉬는시간을 줘야해
DHT11은 쉬는시간을 줘야한다. 1초정도 줬다.
<img src="https://github.com/user-attachments/assets/b68607c0-139c-4b32-99de-2949857dee49" width="500" height="160">   
# 실습
## 습도를 4FND에 출력
✅ 세팅   
DHT11.c에서 TC375에서 사용된 pin번호를 Module_P10, 4로 바꿔야한다. 메인에서 DHT11_UPDATE_TIME이 너무 크기때문에 100000ㅇ
✅ Cpu_Main.c   
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
 /*\title Read the data from DHT11 and visualize it using OneEye GUI
 * \abstract This project is a simple example of how to read the data from the temperature/humidity sensor DHT11 and visualize it using OneEye
 * \description This project is a simple example of how to read the data from the temperature/humidity sensor DHT11 and visualize it using OneEye
 * \name iLLD_TC375_ADS_DHT11
 * \version V1.0.0
 * \board AURIX TC375 lite Kit, KIT_A2G_TC375_LITE, TC37xTP_A-Step
 * \keywords TC375, DHT11, Sensor, OneEye
 * \documents see README.md
 * \lastUpdated 2023-03-20
 *********************************************************************************************************************/

/*********************************************************************************************************************/
/*-----------------------------------------------------Includes------------------------------------------------------*/
/*********************************************************************************************************************/
#include "Ifx_Types.h"
#include "IfxCpu.h"
#include "IfxScuWdt.h"
#include "Bsp.h"
#include "DHT11.h"

#include "IfxPort.h"
#include "IfxPort_PinMap.h"
#include "Driver_Stm.h"
#define LED IfxPort_P10_2
#define SCLK IfxPort_P00_0
#define RCLK IfxPort_P00_1
#define DIO IfxPort_P00_2

// SRC related
#define SRE_IDX 10
#define TOS_IDX 11
void send(unsigned char X);
void send_port(unsigned char X, unsigned char port);
uint8_t _LED_OF[10] = {0xC0, 0xf9, 0xa4, 0xb0, 0x99, 0x92, 0x82, 0xf8, 0x80, 0x90};
/*********************************************************************************************************************/
/*------------------------------------------------------Macros-------------------------------------------------------*/
/*********************************************************************************************************************/
IfxCpu_syncEvent g_cpuSyncEvent = 0;
/* DHT11 sensor data update time: 5 s */
#define DHT11_UPDATE_TIME                   100000

/*********************************************************************************************************************/
/*---------------------------------------------Function Implementations----------------------------------------------*/
/*********************************************************************************************************************/
typedef struct
{
    uint32_t u32nuCnt1ms;
    uint32_t u32nuCnt10ms;
    uint32_t u32nuCnt100ms;
    uint32_t u32nuCnt1000ms;

}TestCnt;

/***********************************************************************/
/*Variable*/
/***********************************************************************/
TestCnt stTestCnt;

void initGPIO(void);
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

    /* Initialize DHT11 communication */
    DHT11_init_comm();
    initGPIO();
    while(1)
    {
        /* Read, decode and verify data from DHT11 */
        DHT11_read_sensor();
        uint8_t h = ((uint8_t)g_dht11_one_eye_data.humidity);
        uint8_t n1 = h %10;
        uint8_t n2 = (h % 100)/ 10;
        uint8_t n3 = (h %1000)/100;
        uint8_t n4 = (h %10000)/1000;
        IfxPort_setPinHigh(LED.port, LED.pinIndex);

        for(int i=0; i<DHT11_UPDATE_TIME; i++){
            send_port(_LED_OF[n1], 0x1);
            send_port(_LED_OF[n2], 0x2);
            send_port(_LED_OF[n3]&0x7f, 0x4);
            send_port(_LED_OF[n4], 0x8);
        }

//        waitTime(DHT11_UPDATE_TIME);
    }
}
void initGPIO(void){
    IfxPort_setPinModeOutput(LED.port, LED.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);

    IfxPort_setPinModeOutput(SCLK.port, SCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);

    IfxPort_setPinModeOutput(RCLK.port, RCLK.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinModeOutput(DIO.port, DIO.pinIndex, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
}
void send(unsigned char X)
{

  for (int i = 8; i >= 1; i--)
  {
    if (X & 0x80)
    {
        IfxPort_setPinHigh(DIO.port, DIO.pinIndex);
    }
    else
    {
        IfxPort_setPinLow(DIO.port, DIO.pinIndex);
    }
    X <<= 1;
    IfxPort_setPinLow(SCLK.port, SCLK.pinIndex);
    IfxPort_setPinHigh(SCLK.port, SCLK.pinIndex);
  }
}

void send_port(unsigned char X, unsigned char port) // 4개의 led중에서 선택해서 송신하는 함수
{
  send(X);
  send(port);
  IfxPort_setPinLow(RCLK.port, RCLK.pinIndex);
  IfxPort_setPinHigh(RCLK.port, RCLK.pinIndex);
}
```
   
✅ DHT11.h 
```c
/**********************************************************************************************************************
 * \file DHT11.h
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
#ifndef DHT11_H
#define DHT11_H

/*********************************************************************************************************************/
/*-----------------------------------------------------Includes------------------------------------------------------*/
/*********************************************************************************************************************/
#include "Ifx_Types.h"

/*********************************************************************************************************************/
/*--------------------------------------------------Data Structures--------------------------------------------------*/
/*********************************************************************************************************************/
typedef struct
{
    uint8 humidity;
    uint8 temperature;
    uint8 data_valid;
    uint16 measurement_number;
} DHT11_one_eye_data;

/*********************************************************************************************************************/
/*-------------------------------------------------Global variables--------------------------------------------------*/
/*********************************************************************************************************************/
extern DHT11_one_eye_data g_dht11_one_eye_data;

/*********************************************************************************************************************/
/*------------------------------------------------Function Prototypes------------------------------------------------*/
/*********************************************************************************************************************/
void DHT11_init_comm(void);
void DHT11_read_sensor(void);

#endif /* DHT11_H */
```
✅ DHT11.c   
```c
/**********************************************************************************************************************
 * \file DHT11.c
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

/*********************************************************************************************************************/
/*-----------------------------------------------------Includes------------------------------------------------------*/
/*********************************************************************************************************************/
#include <stdio.h>
#include "IfxPort.h"
#include "Bsp.h"
#include "DHT11.h"

/*********************************************************************************************************************/
/*------------------------------------------------Function Prototypes------------------------------------------------*/
/*********************************************************************************************************************/
uint16 duration_signal_level(boolean state);
void data_valid(uint8 data_0, uint8 data_2);
void data_not_valid(void);

/*********************************************************************************************************************/
/*------------------------------------------------------Macros-------------------------------------------------------*/
/*********************************************************************************************************************/
/* DHT11 sensor data pin */
#define DHT11_PIN                           &MODULE_P10, 4
/* DHT11 sensor stabilization time: 2 s */
#define DHT11_STABILIZATION_TIME            200000000
/* DHT11 sensor start time: 18 ms - notify the sensor to prepare the data */
#define DHT11_START_TIME                    1800000

/*********************************************************************************************************************/
/*-------------------------------------------------Global variables--------------------------------------------------*/
/*********************************************************************************************************************/
/* DHT11 data visualized using OneEye
    - humidity
    - temperature
    - measurement number
    - data valid */
DHT11_one_eye_data g_dht11_one_eye_data = {0, 0, 0, 0};

/*********************************************************************************************************************/
/*---------------------------------------------Function Implementations----------------------------------------------*/
/*********************************************************************************************************************/
/* Function to initialize DHT11 communication */
void DHT11_init_comm(void)
{
    /* Initialize the DTH11 sensor data pin as input */
    IfxPort_setPinModeInput(DHT11_PIN, IfxPort_InputMode_noPullDevice);

    /* Wait to stabilize the sensor */
    waitTime(DHT11_STABILIZATION_TIME);
}

/* Function to use DHT11
   - check if the connection is established
   - read the raw data from the sensor
   - decode and check the received data */
void DHT11_read_sensor(void)
{
    /* Start signal */
    IfxPort_setPinModeOutput(DHT11_PIN, IfxPort_OutputMode_pushPull, IfxPort_OutputIdx_general);
    IfxPort_setPinState(DHT11_PIN, IfxPort_State_low);

    /* Execute the start time */
    waitTime(DHT11_START_TIME);
    IfxPort_setPinState(DHT11_PIN, IfxPort_State_high);

    /* Wait for 5 us */
    waitTime(500);
    IfxPort_setPinModeInput(DHT11_PIN, IfxPort_InputMode_noPullDevice);

    /* Check the Acknowledgement signal from sensor */
    duration_signal_level(TRUE);

    if (duration_signal_level(FALSE) != 0)
    {
        if (duration_signal_level(TRUE) != 0)
        {
            /* Read 40 bits (5 Bytes) */
            uint16 signals[80] = {0};

            for (uint8 i = 0; i < 80; i += 2)
            {
                /* LOW state of bit */
                signals[i] = duration_signal_level(FALSE);
                /* HIGH state of bit */
                signals[i+1] = duration_signal_level(TRUE);
            }

            /* Decode 5 Bytes */
            uint8 data[5] = {0, 0, 0, 0, 0};

            for (uint8 j = 0; j < 5; j ++)
            {
                /* 8 bits (2 states per bit) */
                for (uint8 k = 0; k < 16; k += 2)
                {
                    data[j] <<= 1;

                    if (signals[(16 * j) + k] < signals[(16 * j) + k + 1])
                    {
                        data[j] |= 1;
                    }
                }
            }

            /* Calculate the checksum */
            uint8 checksum = 0;

            checksum = data[0] + data[1] + data[2] + data[3];

            if (checksum == data[4])
            {
                /* Data is valid */
                data_valid(data[0], data[2]);
            }
            else
            {
                /* Checksum is wrong */
                data_not_valid();
            }
        }
        else
        {
            /* DHT11 sensor is not responding */
            data_not_valid();
        }
    }
    else
    {
        /* DHT11 sensor is not connected */
        data_not_valid();
    }
    g_dht11_one_eye_data.measurement_number ++;
}

/* Function to measure the duration of the provided digital level */
uint16 duration_signal_level(boolean state)
{
    uint16 count = 0;

    while(IfxPort_getPinState(DHT11_PIN) == state)
    {
        count ++;

        /* Overflow */
        if (count >= 0xFFFF)
        {
          return 0;
        }
    }

    return count;
}

/* Function to validate the sensor data */
void data_valid(uint8 data_0, uint8 data_2)
{
    g_dht11_one_eye_data.humidity = data_0;
    g_dht11_one_eye_data.temperature = data_2;
    g_dht11_one_eye_data.data_valid = TRUE;
}

/* Function to invalidate the sensor data */
void data_not_valid(void)
{
    g_dht11_one_eye_data.humidity = 0;
    g_dht11_one_eye_data.temperature = 0;
    g_dht11_one_eye_data.data_valid = FALSE;
}
```
