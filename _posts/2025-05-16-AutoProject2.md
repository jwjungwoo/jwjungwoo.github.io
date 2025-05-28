---
layout: single
title:  "TC275 Multi Core 플젝"
categories: autoever
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 현직자분의 feedback

## ASIL 등급에 따른 차등 개발 프로세스

오토에버 전동화 부서에서 현직자 한 분께 와주셔서 질문을 엄청 많이 드렸다.   
   
✅ ASIL 적용   
개발하기에 앞서 각 기능별로 중요도를 평가하여 ASIL 등급을 매기고 시작하자. 모듈의 등급에 따른 차등 개발 프로세스를 적용한다면 프로젝트의 수준이 높아질 것이라고 하셨다.   
보통 모듈별로 ASIL을 부여한다. 가장 중요한 기능(모듈) 순으로 분석한다. ISO 공식 사이트에서 ISO26262-11 사려했는데 33만원 or 3300만원이라서 사는건 다음 기회에 ㅠ.ㅠ   
   
## ASIL-QM & Watchdog

✅ 용어 정리   
```c
SMU: 시스템 이상(Fault)을 감지하고 알람·인터럽트·NMI로 전달하는 중앙 감시 유닛
SCU: 시스템 전체의 초기화, 클럭, 리셋 등을 관리하는 하드웨어 제어 유닛
Watchdog: CPU나 시스템의 정지·지연을 감지해 리셋이나 Fault를 유도하는 타이머

Watchdog이 timeout되면 SMU가 Fault로 감지하고, 리셋은 SCU가 실행한다.
```   
   
✅ 우리는 ASIL-QM 등급으로 우리의 모터제어 모듈의 등급을 정했다. 그럼 뭘해야할까?   
최소한 이런 기능이 들어가야한다. 라는게 적혀있다고 한다(이걸 보려고 ISO 26262를 사려했는데!) 예를 들면 모니터링 같은 것이 있다.   
   
✅ watchdog   
우선 task에서 watchdog 이 trigger 만 시켜준다. SMU 는 watchdog fault 를 감지한다. 이후 작동방식은 예제코드에 나와있다하셨다. 우선 크게 두 가지 작동방식이 있는데 Reset, NMI Trap이다. 이는 
User(우리)가 마음대로 작성할 수 있다. 단 조심해야한다. 말 그대로 마스크할 수 없는 가장 우선순위 높은 인터럽트기 때문이다. 리셋은 말그대로 리셋이고 NMI Trap 은 내가 원하는 작동을 
하게 하는 것이다.(ex. LED toggle) 우선 Fault ISR은 SMU(System Monitoring Unit)가 Fault를 감지했을 때 설정된 Alarm을 통해 해당 코어에 인터럽트를 발생시켜서 실행된다.   
```c
1. core의 task에서 watchdog에 신호를 못 보냄(watchdog timeout 발생)
2. fault ISR 발생 -> SMU 에 전달
3. SMU Fault 상태 레지스터에 기록됨
4. 해당 Fault가 SMU Alarm에 매핑되어 있으면
5. SMU가 인터럽트를 해당 코어에 보냄
6. 코어에서 우선순위가 꽤 높은 ISR 작동
```

# 결과


## 발표자료 발췌

![스크린샷 2025-05-28 135745](https://github.com/user-attachments/assets/8ffb1da4-62b3-435a-b1d6-ecc2595f4f92)   
   
![스크린샷 2025-05-28 135957](https://github.com/user-attachments/assets/2515af78-c7e5-4097-bde8-d52a22379573)   
   
![스크린샷 2025-05-28 140023](https://github.com/user-attachments/assets/47e31ea2-3c83-4faf-a1e8-cdf57f802baa)   
   
![스크린샷 2025-05-28 120352](https://github.com/user-attachments/assets/2f6cf064-5f2f-45d2-8ea4-00448285a314)   

## 하드웨어

![스크린샷 2025-05-28 141446](https://github.com/user-attachments/assets/734fdfeb-bcab-4f5b-a4ab-82070602c51e)   
   
![KakaoTalk_20250528_141643505](https://github.com/user-attachments/assets/0d51bc75-9c1e-4724-b01f-d5dfb423fee7)   
   




# TC275 기능안전

## safety architecture

![스크린샷 2025-05-21 103403](https://github.com/user-attachments/assets/17f02503-c2c7-4b67-811a-ee4f01fee3c3)

## 공유자원 mutex

하드웨어적으로 지원해주는 것 같다.

# lockstep

## lockstep vs non-lockstep

✅ TC275   
TC2XX 시리즈는 코어3개를 지원한다. 락스탭은 core0, core1 에서 적용된다. 실제로는 물리적으로 1개 코어지만 Dual-core lockstep 처럼 보인다. 
core0, core1 에 하드웨어 sm 을 활성화해야한다. checker 로직이 하나 더 돌아서 둘 중 하나가 삑나면 고장났다고 알려준다. smu 같은 곳에 알람을 띄우고 거기까지가 오토에버 전동화가 하는 일이다.   
```c
내부에 이중 연산 경로가 존재 → 명령을 2번 실행 → 결과를 하드웨어 레벨에서 비교
즉, “Lockstep-capable Core” = 내부적으로 2개 연산 유닛이 내장된 단일 코어

코어   특징
core0  Lockstep 기능(물리1, 내부2개 유닛)
core1  Lockstep 기능(물리1, 내부2개 유닛)
core2  일반 코어
```   
   
✅ TC3XX   
TC3XX 시리즈는 코어가 4개다. 락스탭은 그 중 2개만 지원된다!   

## lockstep control

The lockstep control function is enabled by the LSEN(lockstep enable) bitfield in a control register in the SCU. Each core capable of lockstep has its own instance of the control register. In this product, both the CPU0 and CPU1 instances of the Tricore can be lockstepped so there are two registers, LCLCON0 for CPU0 and LCLCON1 for CPU1. 
These registers are only initialised by a cold power-on reset. In this initialisation state, all lockstepped processors in the system will have lockstep enabled. The lockstep function 
can only be disabled by the system initialisation software writing a 0B to the LSEN bitfield. Application software cannot enable or disable the lockstep function. The current mode of the lockstep logic can be monitored by reading the lockstep status bit, LS(lockstep status), in the associated LCLCON register. Writes to the control registers will be subject to the protection mechanisms of the SCU. 즉 app 에선 lockstep을 해제 또는 실행할 수 없다. 락스탭이 작동하는지는 LS bit 를 보고 판단한다.   
<img src="https://github.com/user-attachments/assets/8188c1c3-a6bb-4bc8-aa1a-3118113dd516" width="800" height="270">   

## lockstep monitoring

The lockstep monitoring function will compare the outputs from the master and checker cores and report that a failure has occurred to the Safety Management Unit (SMU) for appropriate action. 
두 코어는 완전히 동기화된 클럭으로 실행되지만, 하드웨어는 비교를 위해 일시적인 지연(synchronization delay) 을 삽입한다. 체커 코어의 입력과 마스터 코어의 출력은 비교기(comparator)로 전달되기 전에 각각 2 클럭 사이클 동안 지연된다. 이 지연을 통해, 두 코어의 신호를 정확히 정렬하여 비교할 수 있도록 보장한다.

# watchdog

✅ watchdog   
Watchdog Timer (WDT)는 마이크로컨트롤러가 제대로 동작하지 않고 멈췄을 때, 시스템을 자동으로 재시작(Reset)시키기 위한 하드웨어 보호 장치이다. 
즉, 시스템이 정상이면 주기적으로 Watchdog을 “살아있다”는 신호로 갱신해야 하고, 그걸 못 하면 "죽었다"고 판단해서 MCU를 리셋(재부팅) 시킨다. core0 watchdog, core1 watchdog, core2 watchdog 이 존재한다. 반면 safety watchdog 은 tc275에 1개만 존재한다.   
```c
WDT 타임아웃 → SMU 알람 발생 → NMI 요청 → Recovery Timer 0 시작
                             ↓
                        제한시간 초과 시 → 리셋 요청 + FSP
```   
   
✅ 특징   
기본적인 "감시(Watchdog)" 기능 외에도, 각 WDT에는 End-of-Initialization(ENDINIT) 기능이 포함되어 있어서 critical registers 를 실수로 덮어쓰는 것을 방지한다. So, use a password and guard bits during accesses to the WDT control registers. 주기적으로 유효한 접근(Password Access + Modify Access)을 통해 ENDINIT 비트를 클리어하여 중요 레지스터에 접근해야한다. 주기내에 못 오면 SMU 에게 알린다. SMU는 알람 발생 시 NMI 를 발생한다. 곧바로 리셋시키는 대신, 먼저 NMI(Non-Maskable Interrupt)를 통해 회복 또는 상태 저장 시간을 줄 수 있도록 설정할 수 있다.   
마스킹(masking) 할 수 없는 고정 우선순위의 인터럽트임으로 일반적인 인터럽트처럼 비활성화(disable)하거나 무시할 수 없다.   
   
core0 를 제외한 다른 CPU들의 Watchdog Timer는 기본 설정상, 타임아웃 발생 시 리셋을 유발하도록 설정되어 있지 않다. (근데 우리는 multi-core 쓰기로 해놔서 되려나?) 하지만 이는 SMU 설정을 통해 활성화할 수 있다 (자세한 내용은 SMU 섹션 참조). 그리고 각 CPU의 Watchdog은 오직 해당 CPU만이 설정, 활성화 또는 비활성화할 수 있다.

## CPU watchdog vs Safety watchdog

```c
구분	      CPU Watchdog	                       Safety Watchdog
동작 대상  	각 코어(Core0, Core1, ...)별로 존재	 시스템 전체(모든 코어 포함)
설정 위치	  각 CPU 전용 WDT 레지스터     	       SCU(시스템 제어 유닛)에 존재
감시 대상  	그 CPU 내의 코드가 멈췄는지 	         전체 시스템, safety 기능 자체가 살아 있는지
일반 용도	  task watchdog, heartbeat	           ISO 26262 등 ASIL 감시에 사용
클리어 방법	해당 코어에서만 가능	                 시스템 단에서만 클리어 가능
오용시 문제	해당 코어만 리셋          	           전체 시스템 리셋 유발 가능 (위험!)
```

# SMU

## SMU Event Triggered Emergency Stop

The Safety Alarm(s) which can trigger an Emergency Stop are configured and enabled within the Safety Management Unit (SMU). All SMU triggered Emergency Stop cases are in Synchronous Mode, regardless of the state of EMSR.MODE. The safety emergency stop flag (EMSR.SEMSF)(이때 EMSR는 SCU의 register 이다.) is set(1로 설정) when a configured and enabled SMU Safety Alarm occurs. The setting of (EMSR.SEMSF) activates the emergency stop. An SMU triggered emergency state can only be terminated by clearing the EMSR.SEMSF via software (Write EMSR.SEMSFM with 10B).   
SCU는 SMU에서 온 PES 신호를 감지하여 EMSR.SEMSF 비트를 자동으로 Set하여 긴급정지한다. 난 긴급정지 안 하고 싶음.   
그림은 SCU의 EMSR   
<img src="https://github.com/user-attachments/assets/5a52f707-8637-4244-a25b-f891a869de87" width="800" height="300">   
   
<img src="https://github.com/user-attachments/assets/7c4d7e0b-5232-4f8b-8b73-550b13060b1f" width="800" height="690">   

## SMU enum

![watchdog_enum](https://github.com/user-attachments/assets/aa8bb315-5ab4-4a34-8997-d65e241dd01b)   

# SCU

## NMI Trap

NMI 트랩은 소프트웨어나 하드웨어 트리거를 통해 발생할 수 있으며, TRAPSET 레지스터에 해당 비트를 set 하면 트랩 발생 가능하다.   
시스템 내 모든 코어에 동일한 NMI 트랩이 동시에 발생하다.   
트랩 플래그는 TRAPCLR 레지스터를 통해 소프트웨어로 클리어 가능하다.   
트랩 소스가 disable되어 있으면 NMI는 발생하지 않는다.	단순히 플래그만 설정되고, 실제로 NMI 인터럽트는 생성되지 않는다.   
   
✅ 잘 안된다.   
SMU 로 watchdog 알람이 오면 nmi trap 으로 watchdog 을 늦추고 싶었다. 그래서 함수를 타고타고 들어가기도하고 데이터시트도 읽어봤는데 어려워서 프로젝트 기간 내에 할 수 없다 판단했다. 자율주행 플젝 2주는 좀 빠듯했다.ㅋㅋㅋ   
![nmi](https://github.com/user-attachments/assets/c7881425-aada-4b1e-aacb-f519ac0e75af)

# HARA

## HARA 단계

✅ HARA 사이트   
<https://www.hermessol.com/2024/12/19/hara/>   
   
✅ HARA 란?   
HARA가 HARA는대로 하자. 가 아니라 ㅎ   
HARA 는 ISO 26262 Part 3에 정의된 프로세스로 일반적으로 아래와 같은 흐름으로 구성된다.   
   
![hara](https://github.com/user-attachments/assets/fea36920-352a-44e9-917a-d34daf27bf35)   

## ASIL QM

✅ (S=3, E=1, C=2)​   
우리는 우리가 개발하는 모듈을 심각성, 가능성, 제어성을 3,1,2로 정하였다. 이는 ASIL-QM 등급의 모듈임으로 가장 낮은 ASIL 등급이라 크게 까다로운 프로세스를 적용할 필요가 없었다. 하지만 re-test, regression test 에 되도록 신경쓰며 프로젝트 개발을 진했했다. 또한 Watchdog, SMU 기능을 도입하려 시도했다.   
![embitel (a Volkswagen group company)](https://github.com/user-attachments/assets/3f4e203f-c031-435d-906e-d79e34ae9bcb)   

# 정적테스트

## 의존성분석

✅ 의존성 경고(빨간색)   
![스크린샷 2025-05-27 110937](https://github.com/user-attachments/assets/d82fd533-08a2-4d7e-b70b-ca32d3663463)   
   
✅ 사이클 확인   
우리 코드에서 서로 의존하는 파일들이 발견됐다. 따라서 이 파일들은 관리 대상이다.   
![스크린샷 2025-05-27 111131](https://github.com/user-attachments/assets/ff9c2f75-37f4-493a-9233-1ab0a2ca5a75)   

