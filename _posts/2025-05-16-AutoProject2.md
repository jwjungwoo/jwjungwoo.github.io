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

## 

