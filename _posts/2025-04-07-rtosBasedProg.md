---
layout: single
title:  "차량용 RTOS 기반 프로그래밍"
categories: autoever
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 느낀점

✅ 팀의 중요성: 우리팀은 마지막 프로그램 때 처음 보는 환경에서 Ifx_Port 기능을 추가해야했는데 진짜 헤더파일 하나하나 다 확인하며 결국 관련 헤더파일을 찾아냈다. 
팀원들과의 소통이 없었으면 시간(1시간)내에 할 수 없었다.   
✅ RTOS 의 중요성: 실시간 작동을 위해 너무 무거운 모델을 탑재한 프로젝트는 진행하면 안되겠다는 생각이 들었다.   
✅ 흥미진진: Resource, Alarm 등의 코드를 동작하면서 앞으로 있을 프로젝트를 얼른 하고싶었다.   

# RT System

## Real Time

✅ Logical and Temporal correctness   
embedded system 에 많다.   
   
✅ Infotainment 는 운전자를 도와주는 수준이고, Intelligence 는 운전자 대체 느낌이다. 여러 데이타(비전 정보)를 받아들이기 위해 Ethernet 등을 사용한다. Intelligence 는 아직 많이 안 쓰인다. 테슬라는 
이더넷 말고 다른 기술을 사용한다.   
<img src="https://github.com/user-attachments/assets/12952b2b-c426-4b20-8926-a5e89e95c64b" width="800" height="600">   
   
## Firmware vs RTOS

✅ Firmware vs RTOS   
<img src="https://github.com/user-attachments/assets/02dec599-137f-49d6-8e5b-9832119d811e" width="800" height="640">   

# 실습

## Task Activation

✅ 결과   
<<<<<<<<<<< 사진  task activation >>>>>>

   
✅ asw.c   
```c
#include "bsw.h"

ISR2(TimerISR)
{
    static long c = -4;
    osEE_tc_stm_set_sr0_next_match(1000000U); // 1 sec
    if(c == 0)
        ActivateTask(Task1);
    printfSerial("\n%4ld: ", c++);
}

TASK(Task1)
{
    printfSerial("Task1 begins..."); 
    mdelay(2000);
    ActivateTask(Task2);
    mdelay(4000);
    printfSerial("Task1 ends...");

    TerminateTask();
}

TASK(Task2)
{
    printfSerial("Task2 begins...");
    mdelay(4000);
    ActivateTask(Task3);
    mdelay(2000);
    printfSerial("Task2 ends...");

    TerminateTask();
}

TASK(Task3)
{
    printfSerial("Task3 begins...");
    mdelay(3000);
    printfSerial("Task3 ends...");

    TerminateTask();
}
```   
   
✅ conf.oil   
만약 task activation 이 아니라 가만히 있어도 실행하게 하고싶었으면 AUTOSTART = TRUE 로 하면된다.   
```c
CPU test_application {

    OS EE {
        EE_OPT = "OSEE_DEBUG";
        EE_OPT = "OSEE_ASSERT";
        EE_OPT = "OS_EE_APPL_BUILD_DEBUG";
        EE_OPT = "OS_EE_BUILD_DEBUG";
        EE_OPT = "EE_BUILD_SINGLE_ELF";    
        CFLAGS = "-Wno-implicit-function-declaration -Wno-cast-align -Wno-missing-prototypes  -Wno-missing-field-initializers -Wno-double-promotion -Wno-strict-prototypes -Wno-unused-parameter -Wno-missing-declarations";
	
        CPU_DATA = TRICORE {
            ID = 0x0;
        };

        MCU_DATA = TC27X {
            DERIVATIVE = "tc275tf";
            REVISION = "DC";
        };

        KERNEL_TYPE = OSEK {
            CLASS = ECC2;
        };
    };

    APPDATA tricore_mc {
        APP_SRC="illd/src/IfxAsclin_Asc.c";
        APP_SRC="illd/src/IfxStm.c";
        APP_SRC="illd/src/IfxStm_cfg.c";
        APP_SRC="illd/src/IfxStdIf_DPipe.c";
        APP_SRC="illd/src/Ifx_Fifo.c";
        APP_SRC="illd/src/Ifx_CircularBuffer.c";
        APP_SRC="illd/src/IfxAsclin_PinMap.c";
        APP_SRC="illd/src/IfxScuWdt.c";
        APP_SRC="illd/src/IfxAsclin_cfg.c";
        APP_SRC="illd/src/IfxScu_cfg.c";
        APP_SRC="illd/src/Bsp.c";
        APP_SRC="illd/src/IfxScuCcu.c";
        APP_SRC="illd/src/IfxScu_PinMap.c";
        APP_SRC="illd/src/IfxPort.c";
        APP_SRC="illd/src/IfxSrc.c";
        APP_SRC="illd/src/IfxSrc_cfg.c";
        APP_SRC="illd/src/IfxAsclin.c";
        APP_SRC="illd/src/IfxPort_cfg.c";
        APP_SRC="illd/src/CompilerGnuc.c";
        APP_SRC="illd/src/IfxCpu_Irq.c";
        APP_SRC="illd/src/IfxCpu.c";
        APP_SRC="illd/src/IfxAsclin.c";
        APP_SRC="illd/src/IfxCpu_cfg.c";
        APP_SRC="illd/src/IfxScuEru.c";
        APP_SRC="illd/src/IfxVadc_Adc.c";
        APP_SRC="illd/Libraries/iLLD/TC27D/Tricore/_Impl/IfxVadc_cfg.c";
        APP_SRC="illd/Libraries/iLLD/TC27D/Tricore/Vadc/std/IfxVadc.c";

        APP_SRC="bsw.c";
        APP_SRC="asw.c";
    };

    TASK Task1 {
        PRIORITY = 1;
        STACK = SHARED;
        SCHEDULE = FULL;
        AUTOSTART = FALSE;
        ACTIVATION = 1;
    };
    TASK Task2 {
        PRIORITY = 2;
        STACK = SHARED;
        SCHEDULE = FULL;
        AUTOSTART = FALSE;
        ACTIVATION = 1;
    };
    TASK Task3 {
        PRIORITY = 3;
        STACK = SHARED;
        SCHEDULE = FULL;
        AUTOSTART = FALSE;
        ACTIVATION = 1;
    };

    ISR TimerISR {
        CATEGORY = 2;
        SOURCE = "STM0SR0";
        PRIORITY = 2;
    };

    ISR ASCLIN0_TX {
        CATEGORY = 2;
        PRIORITY = 19;
        HANDLER = "asclin0TxISR";
        SOURCE = "ASCLIN0TX";
    };
};
```   
   
## GetTaskState

✅ 하고싶은거   
<img src="https://github.com/user-attachments/assets/a01d6dde-5fc0-47a5-ad4a-b67635c473e4" width="500" height="240">   
   
✅ 결과   
<img src="https://github.com/user-attachments/assets/c6537d62-8874-4b32-9b97-85301a7f9117" width="500" height="400">   
   
✅ asw.c   
```c
#include "bsw.h"

ISR2(TimerISR)
{
    static long c = -4;
    osEE_tc_stm_set_sr0_next_match(1000000U); // 1 sec
    if(c == 0)
        ActivateTask(Task1);
    printfSerial("\n%4ld: ", c++);
}

void printState(TaskType id) {
    TaskStateType state;

    if (GetTaskState(id, &state) == E_OK) {
        switch (state) {
            case SUSPENDED:
                printfSerial("%d: suspended...",id);
                break;
            case READY:
                printfSerial("%d: ready...",id);
                break;
            case WAITING:
                printfSerial("%d: waiting...",id);
                break;
            case RUNNING:
                printfSerial("%d: running...",id);
                break;
        }
    }
}

TASK(Task1)
{
    TaskType id;
    printfSerial("Task1 begins..."); 
    printState(Task1);
    printState(Task2);
    mdelay(3000);
    ActivateTask(Task2);    
    printState(Task1);
    printState(Task2);
    mdelay(3000);
    GetTaskID(&id);
    printfSerial("Task ID = %d...", id); 
    printfSerial("Task1 ends...");
    ChainTask(TaskM);
}

TASK(Task2)
{
    TaskType id;
    printfSerial("Task2 begins...");
    printState(Task1);
    printState(Task2);
    mdelay(3000);
    GetTaskID(&id);
    printfSerial("Task ID = %d...", id); 
    printfSerial("Task2 ends...");
    ChainTask(TaskM); // ActivateTask 는 지정 Task 를 대기상태로 해주는 반면
    // ChainTask 는 현재 Task 는 종료하고, 지정 Task 로 즉시 연결한다.
}

TASK(TaskM)
{
    printState(Task1);
    printState(Task2);

    TerminateTask();
}
```

## 버튼 누르기, activation 2

✅ activation 을 2로 하면 버튼 두 번 누르면 2번 동작한다. 총알이 2개라 보면 된다. 한번 작동완료 하면 또한다.   
   
✅ 결과   
<img src="https://github.com/user-attachments/assets/58bc1b4d-0a2b-4747-893b-25a9c06f68c0" width="800" height="400">   

✅ asw.c   
```c
CPU test_application {

    OS EE {
        EE_OPT = "OSEE_DEBUG";
        EE_OPT = "OSEE_ASSERT";
        EE_OPT = "OS_EE_APPL_BUILD_DEBUG";
        EE_OPT = "OS_EE_BUILD_DEBUG";
        EE_OPT = "EE_BUILD_SINGLE_ELF";    
        CFLAGS = "-Wno-implicit-function-declaration -Wno-cast-align -Wno-missing-prototypes  -Wno-missing-field-initializers -Wno-double-promotion -Wno-strict-prototypes -Wno-unused-parameter -Wno-missing-declarations";
	
        CPU_DATA = TRICORE {
            ID = 0x0;
            CPU_CLOCK = 200.0;
        };

        MCU_DATA = TC27X {
            DERIVATIVE = "tc275tf";
            REVISION = "DC";
        };

        KERNEL_TYPE = OSEK {
            CLASS = ECC2;
        };
    };

    APPDATA tricore_mc {
        APP_SRC="illd/src/IfxAsclin_Asc.c";
        APP_SRC="illd/src/IfxStm.c";
        APP_SRC="illd/src/IfxStm_cfg.c";
        APP_SRC="illd/src/IfxStdIf_DPipe.c";
        APP_SRC="illd/src/Ifx_Fifo.c";
        APP_SRC="illd/src/Ifx_CircularBuffer.c";
        APP_SRC="illd/src/IfxAsclin_PinMap.c";
        APP_SRC="illd/src/IfxScuWdt.c";
        APP_SRC="illd/src/IfxAsclin_cfg.c";
        APP_SRC="illd/src/IfxScu_cfg.c";
        APP_SRC="illd/src/Bsp.c";
        APP_SRC="illd/src/IfxScuCcu.c";
        APP_SRC="illd/src/IfxScu_PinMap.c";
        APP_SRC="illd/src/IfxPort.c";
        APP_SRC="illd/src/IfxSrc.c";
        APP_SRC="illd/src/IfxSrc_cfg.c";
        APP_SRC="illd/src/IfxAsclin.c";
        APP_SRC="illd/src/IfxPort_cfg.c";
        APP_SRC="illd/src/CompilerGnuc.c";
        APP_SRC="illd/src/IfxCpu_Irq.c";
        APP_SRC="illd/src/IfxCpu.c";
        APP_SRC="illd/src/IfxAsclin.c";
        APP_SRC="illd/src/IfxCpu_cfg.c";
        APP_SRC="illd/src/IfxScuEru.c";
        APP_SRC="illd/src/IfxVadc_Adc.c";
        APP_SRC="illd/Libraries/iLLD/TC27D/Tricore/_Impl/IfxVadc_cfg.c";
        APP_SRC="illd/Libraries/iLLD/TC27D/Tricore/Vadc/std/IfxVadc.c";

        APP_SRC="bsw.c";
        APP_SRC="asw.c";
    };

    TASK Task1 {
        PRIORITY = 1;
        STACK = SHARED;
        SCHEDULE = FULL;
        AUTOSTART = FALSE;
        ACTIVATION = 2;
    };

    TASK Task2 {
        PRIORITY = 2;
        STACK = SHARED;
        SCHEDULE = FULL;
        AUTOSTART = FALSE;
        ACTIVATION = 2;
    };

    TASK TaskM {
        PRIORITY = 3;
        STACK = SHARED;
        SCHEDULE = FULL;
        AUTOSTART = FALSE;
        ACTIVATION = 1;
    };

    ISR ASCLIN0_TX {
        CATEGORY = 2;
        PRIORITY = 19;
        HANDLER = "asclin0TxISR";
        SOURCE = "ASCLIN0TX";
    };

    ISR TimerISR {
        CATEGORY = 2;
        SOURCE = "STM0SR0";
        PRIORITY = 2;
    };

    ISR ButtonISR {
        CATEGORY = 2;
        SOURCE = "SCUERU0";
        PRIORITY = 10;
    };
};
```
   
✅ conf.oil   
```c
    TASK Task1 {
        PRIORITY = 1;
        STACK = SHARED;
        SCHEDULE = FULL;
        AUTOSTART = FALSE;
        ACTIVATION = 2;
    };

    TASK Task2 {
        PRIORITY = 2;
        STACK = SHARED;
        SCHEDULE = FULL;
        AUTOSTART = FALSE;
        ACTIVATION = 2;
    };
    ISR ButtonISR {
        CATEGORY = 2;
        SOURCE = "SCUERU0";
        PRIORITY = 10;
    };
```

## Event

✅ 설명   
```c
SetEvent(Task2, Event1); : task2 에게 event1 이 들어왔음을 알려준다.
MULTI_STACK = TRUE : 각각의 스택마다 개별 공간을 사용할 수 있게 해준다.
EVENT Event1 {Mask = AUTO;} : Mask는 이벤트를 구분해주는데, 그것을 AUTO 로 할당받는다.
STACK = PRIVATE { 
  SIZE = 1024;
} : 이벤트를 기다리는 event 는 반드시 private 이어야한다.
```   
<img src="https://github.com/user-attachments/assets/91dac5af-8dba-458e-b613-573172dddf4f" width="800" height="550">   
<img src="https://github.com/user-attachments/assets/5c4c0fb9-2f4a-4acf-8a33-31dc00bebf91" width="800" height="550">   
<img src="https://github.com/user-attachments/assets/bab0530f-1ffa-4a4f-8e53-f7d768935565" width="800" height="550">   
<img src="https://github.com/user-attachments/assets/5ec7f336-b4f2-4c39-947e-ef23f34993d5" width="800" height="550">   

## Alarm SetEvent

<img src="https://github.com/user-attachments/assets/5175621f-c33b-4453-be20-9fb29979a01c" width="800" height="550">   
<img src="https://github.com/user-attachments/assets/c5780cc4-62cd-403c-b471-fa834c960832" width="800" height="550">   

## Hook

✅ OS 가 시작했을 때도 hook 들어오고, task 가 실행되기 전, 끝나기 전에도 hook 이 들어온다.   
   
<img src="https://github.com/user-attachments/assets/aa9a3b64-438b-4bce-a328-447408ae2138" width="800" height="550">   
<img src="https://github.com/user-attachments/assets/7867ef12-5dd3-4ad4-a369-c6fdee6370e4" width="800" height="550">   
<img src="https://github.com/user-attachments/assets/5684786e-c58c-4a85-83b9-75027d068a03" width="800" height="550">   
<img src="https://github.com/user-attachments/assets/f142add8-b3a0-47de-8f3a-074d79b2ce74" width="800" height="550">   

## Error Handling

<img src="https://github.com/user-attachments/assets/2c6423e3-4496-4b86-bf75-11a26cdc594d" width="800" height="550">   
<img src="https://github.com/user-attachments/assets/de1e8425-d73f-4dd4-995a-00d3a1dd8da7" width="800" height="550">   
<img src="https://github.com/user-attachments/assets/c4c906dc-7cdb-41a3-937f-817958c1e729" width="800" height="550">   
<img src="https://github.com/user-attachments/assets/44bf950d-c64b-4d78-88df-f8dce7b4f157" width="800" height="550">   
<img src="https://github.com/user-attachments/assets/00f79cc8-b2fc-418d-83de-374942a06f31" width="800" height="550">   

## Deadline Miss

<img src="https://github.com/user-attachments/assets/b751c68f-aa98-4715-af0d-571c3f689ead" width="800" height="550">   
<img src="" width="800" height="550">   
