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

✅ 

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

