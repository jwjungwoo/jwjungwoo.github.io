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

오토에버 전동화 부서에서 현직자 한 분이 와주셔서 질문을 엄청 많이 드렸다.   
   
✅ ASIL 적용   
개발하기에 앞서 각 기능별로 중요도를 평가하여 ASIL 등급을 매기고 시작하자. 모듈의 등급에 따른 차등 개발 프로세스를 적용한다면 프로젝트의 수준이 높아질 것이라고하셨다.   
보통 모듈별로 ASIL을 부여한다. 가장 중요한 기능(모듈) 순으로 분석한다. ISO 공식 사이트에서 ISO26262-11 사려했는데 33만원 or 3300만원이라서 사는건 다음 기회에 ㅠ.ㅠ   
   
## ASIL-QM & Watchdog

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

# TC275 기능안전

## lockstep vs non-lockstep

✅ TC275   
TC2XX 시리즈는 코어3개를 지원한다. 락스탭은 core0만 적용된다. 실제로는 물리적으로 1개 코어지만 Dual-core lockstep 처럼 보인다. 하나의 코어에서만 락스탭이 적용된다. 
core0 에 하드웨어 sm 을 활성화해야한다. checker 로직이 하나 더 돌아서 둘 중 하나가 삑나면 고장났다고 알려준다. smu 같은 곳에 알람을 띄우고 거기까지가 오토에버 전동화가 하는 일이다.   
```c
내부에 이중 연산 경로가 존재 → 명령을 2번 실행 → 결과를 하드웨어 레벨에서 비교
즉, “Lockstep-capable Core” = 내부적으로 2개 연산 유닛이 내장된 단일 코어

코어   특징
core0  Lockstep 기능(물리1, 내부2개 유닛)
core1  일반 코어
core2  일반 코어
```   
   
✅ TC3XX   
TC3XX 시리즈는 코어가 4개다. 락스탭은 그 중 2개만 지원된다!   

## watchdog

✅ watchdog   
Watchdog Timer (WDT)는 마이크로컨트롤러가 제대로 동작하지 않고 멈췄을 때, 시스템을 자동으로 재시작(Reset)시키기 위한 하드웨어 보호 장치이다. 
즉, 시스템이 정상이면 주기적으로 Watchdog을 “살아있다”는 신호로 갱신해야 하고, 그걸 못 하면 "죽었다"고 판단해서 Watchdog이 MCU를 리셋(재부팅) 시킨다. core 마다의 Watchdog 이 존재한다. 

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

# HARA

## HARA 단계

✅ HARA 사이트   
<https://www.hermessol.com/2024/12/19/hara/>   
   
✅ HARA 란?   
HARA가 HARA는대로 하자. 가 아니라 ㅎ   
HARA 는 ISO 26262 Part 3에 정의된 프로세스로 일반적으로 아래와 같은 흐름으로 구성된다.   
   
![hara](https://github.com/user-attachments/assets/fea36920-352a-44e9-917a-d34daf27bf35)   


## ASIL QM
