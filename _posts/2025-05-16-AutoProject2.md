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
