---
layout: single
title:  "SW testing"
categories: autoever
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 느낀점

✅ Shift Left!!: 앞단에서부터 SW Testing 을 진행하면서 버그를 잡자!   
✅ 외우자   
<img src="https://github.com/user-attachments/assets/4de5ca6e-a06e-42d1-830d-9cfd35dff201" width="800" height="500">   
✅ 

# 개요

## What, How

✅ A-SPICE: What 만 적혀있음.   
✅ ISO 26262: How 도 적혀있음.   

## SW 품질 확보 방법 유형

✅ 프로세스 품질   
제품을 만드는 사람을 평가한다. 계획, 표준을 준수하여 개발하였는가. 심사(Assessment) 및 감사(Audit) 등을 통해 확인   
   
✅ 제품 품질   
SW 를 품질한다. 정적/동적 테스팅.   
V&V (Verification, Validation)   

## SW 제품 품질 특성(ISO 25010)

```c
1. 기능성: 원했던 기능이 다 들어갔어?(만족했어?)
2. 신뢰성: HW 에서 많이 쓰임. 결함 없어?
3. 사용성: 사용하기 쉽냐? 그리고 예쁘냐?
4. 효율성: 자원 많이 안 잡아먹냐?
5. 유지보수성
6. 이식성: 볼보에서도 쓸 수 있고, 현대차에서도 쓸 수 있고
7. 상호운영성: 다른 시스템과의 상호 연동 능력
8. 보안성
...
ADAS 는 제어정도, 자율성 등의 다른 특성이 있다.
```

## V&V

Verification(검증): 제품이 올바르게 생성하고 있는가?   
Validation(확인): 올바른 제품을 생성하고 있는가?(고객이 원했던 제품이 맞니?)   
   
모라이는 validation 에 사용된다고 하셨다.   

## V 모델

```c
우측
요구사항 정의: 고객 요구사항
요구사항 분석: SW 요구사항
상위설계: 청사진
상세설계: 알고리즘, 변수

구현: 소스코드 프로그래밍

좌측
인수: 이거만 validation
시스템: verification
통합: verification
단위: verification
```   

## A-SPICE 4.0

추적성, 일관성을 확보해야한다

## ISO 26262 mistake

```c
Mistake: Human Error
Fault
Error: 의도한 바와 실행된 결과간에 불일치가 발생
Failure: 고장
```

## SW 테스팅 종류

정적 테스팅: SW 를 실행하지 않고 결함 발생   
동적 테스팅: sW 를 실행하여 결함 발견   
   
<img src="https://github.com/user-attachments/assets/fc5dd1b7-9d44-4434-9a58-5923d41e138b" width="800" height="420">   
<img src="https://github.com/user-attachments/assets/30a402a6-f4f1-4c39-ac75-0184cd70bf18" width="800" height="450">   
   
##  SW 테스트 프로세스

```c
1. Test management process
Test Planning
Test Monitoring and Control
Test Completion: 테스트가 잘 완료됐는지 확인

2. Dynamic test process
Test Design: 테스트를 어떻게 할 건지 구상
Test Implemetation: 구조 만듦
Test Execution: 실제로 실행
```

# 중요

## 테스트 주요 용어

✅ Test Basis 란?   
테스트 케이스를 작성하기 위한 근거 혹은 근거 산출물. 로그인 수행을 평가할 때.. 어떻게 할지 등을 적는다.   
   
✅ Test Case 란?   
Test Basis 랑 짝꿍이다. 테스트 케이스는 각각의 Test Basis 에 맞춰 존재한다.   
필수항목: 입력값, 테스트 절차, 예상출력값. 반드시 반드시 반드시 3가지.   
   
<img src="https://github.com/user-attachments/assets/aae052ad-7597-4b18-a3da-7f7349210965" width="800" height="470">   
   
✅ Test Case 작성 시점   
<img src="https://github.com/user-attachments/assets/805fd4ac-cad0-4b26-ad42-544c3d300bdc" width="800" height="580">   
   
✅ 테스트 스위트(Suite)   
목표로 묶임.   
   
✅ 테스트 시나리오   
사용자의 실제 행동을 기반으로 묶음.   

## 통합 테스트 수행

E(시작조건): Entry point   
Task   
V(검증)   
X(종료조건): Coverage(얼마나 많이 테스트했는가. 80% 정도 했는가?)   

## ISO 26262 의 테스트 결과 평가

테스트 결과와 "예상 결과"의 일치 : 테스트 성공률
SW 안전 요구사항 커버리지 달성: Req 커버리지(100%)   
   
```c
test case 들을 통해 모든 test basis 를 한번 이상은 봐줘야함.
```   
   
✅ 요구사항 커버리지   
30개 요구사항 중 15개 하면 50퍼센트 한거임.   
   
✅ 소스코드 커버리지   
소스코드 전체 문장의 40퍼센트 했다.. 이런식이다.   

## MBD

MBD: Model Base Development   
ISO 26262 에선 MBD 를 하라고 권고한다. 현대자동차에서도 2년전부터 하려한다. SW 상세 설계의 내용이 소스코드로 그대로 옮겨짐. 따라서 설계에서 모든 걸 결정하고 철저하게 분석하여 human error 를 배척한다. 

## A-SPICE 3.1 vs 4.0

<img src="https://github.com/user-attachments/assets/58b3f0d0-4bef-4f2e-8756-0911d1edb3f7" width="800" height="800">   

## Archi, Compo, Unit

Architecture = ECU 위에 올라가는 SW = (Integrated) SW = Embedded SW   
Unit = 하나의 파일 혹은 하나의 함수 = 모듈   
<img src="https://github.com/user-attachments/assets/04aa9d3d-02ac-4807-8c89-d314211d88d5" width="900" height="300">   

## Host, Target, in-the-loop tests

Host: 컴퓨터   
Target: 보드   
   
VIL: Vehicle. (나뿐만 아니라 나머지도 진짜)   
HIL: HW. 실제 올려라.(ECU, Sensor, Actuator 는 진짜. 다만 나빼고 나머지는 가짜)   
PIL: Processor. 실제 ECU 에 올릴 수 있겠냐.(ECU 는 진짜고 Sensor, Activator 는 가짜)   
SIL: SW. Target 까진 어렵고 Host 에서 돌린다. 가상으로   
MIL: Model   

# TEST

## Unit Test

개발자(본인)가 수행하는 단계이다.   
확인할 것: 기능(알고리즘), 경계조건(유닛과 유닛과의 관계_input 과 output)   
   
✅ 하기 위한 환경을 꾸며줘야함   
<img src="https://github.com/user-attachments/assets/0d8c71ea-b31f-4a6a-bdf6-ae4c3e3d1793" width="900" height="500">   
스텁: Unit 안에서 다른 함수를 호출하면 그 함수를 가짜로 만든다.   
테스팅 드라이버: 나를 호출시킨 다른 Unit. 
```c
int jw(int idx) {
  int num = sum(1, 2);
  return num; 
}
jw 입장에선 sum 이 스텁, sum 입장에선 jw 가 테스팅 드라이버
```   
   
## Integration Test

단위와 단위간의 통합, 컴포넌트와 컴포넌트간의 통합을 본다.   
쪼개진 것들을 통합하면서 연결된 부위들이 문제 없는지를 보겠다.   
모듈1,2,3 를 통합 SW 로 합치는 과정이다. 빅뱅, 하향식, 상향식, 그리고 CI   
   
1. 빅뱅 통합   
그냥 한번에 합쳐버림. 어디서 오류났는지 확인하기 힘들어서 요즘엔 잘 안 씀.   
<img src="https://github.com/user-attachments/assets/158b2d72-705f-48e4-98e8-d965eab35d0a" width="900" height="500">   
   
2. Top-Down 통합   
<img src="https://github.com/user-attachments/assets/2f8a96b5-4502-41d1-b55c-155980c77d96" width="900" height="500">   
   
3. Down-Top 통합   
<img src="https://github.com/user-attachments/assets/66dc3e79-5787-44f9-b7ac-369acc1e97f3" width="900" height="500">   
   
4. CI (Continuous Integration)   
마지막에 몰아서 합치지말고 시작부터 지속적 통합을 적용하자.   
   
✅ 참고   
정적 아키텍처, 동적 아키텍처. 여기서 정적 아키텍처는 코드 구성표를 의미하고, 동적 아키텍처는 코드간 흐름, 어떤 함수가 오가는지 등을 표현한다.   
   
## System Test

System Test: 통합된 SW 가 요구사항과 동일하냐?   
   
수행주체: 테스터(제 3자)   
테스트 대상: 요구사항 명세서를 기초로 하여 기능 요구사항. 보안, 성능, 신뢰성, UX 등의 비기능 요구사항. 기능 안전 요구사항   
   
기능 테스트: 기능이 잘 작동하는지, 수행함에 있어서 defact 는 없는지.   
비기능 테스트: 성능 테스트, 사용성 테스트가 여기에 포함된다. ex) 12시간 돌렸을 때 정상동작 하는가. 메모리, ecu, resource 등에 문제가 없는지.   
   
```c
(비기능 테스트 예시)
사용자가 해당 기능을 요청하면, 네이게이션 시스템은 3초 이내에 처리해야한다. 단, 대량의 BATCH 성 처리 작업은 배제한다.
```
   
또한 테스트의 형상을 고정하고 테스트를 진행해야한다. (소스코드의 버전을 고정해놓고 하라)   
   
✅ ISO 26262의 SW 테스팅 수행 환경   
최소 HIL 이상의 테스트 환경을 요구. 즉, Host 말고 Target 에서 해야한다. Lab car, Mule, Rest of bus 등. Lab car 는 굴러가진 않는 
자동차, Mule 은 굴러다니는 차, Rest of bus 는 CANoe 같이 ECU 말고 나머지 장치들을 가상으로 만들어주는 것이다.

## Acceptance Testing

실제 사용자가 운영하는 환경에서 실시.   
실제 도로, 날씨 등을 구축해야한다. 예를들면 모라이 같은 것이 있다.   
   
```c
(대표적인 인수 테스트)
알파 테스트: 회사 내부 사람들이 "우리 제품 잘 돌아가나?" 하고 테스트해보는 단계
베타 테스트: 외부 사용자(일반 유저나 특정 고객)한테 써보게 하면서 피드백 받는 단계
```   
     
Test Bench, Test Track, validation on public roads, field use by end users.   

End-to-End 시나리오: 처음부터 끝까지의 모든 과정의 시나리오를 짜서 테스트해본다.

# 동적 테스트

## 대표적인 방법

(스펙기반)   
명세기반 테스트: black box test   
구조기반 테스트: white box test   
(경험기반)   
경험기반 테스트: 테스터의 경험 혹은 과거의 경험을 기반으로 테스팅. 하지만 ISO26262 는 입증할 수 있어야한다. 증명을 할 수 있어야하며 문서가 있어야한다. 따라서 경험기반 테스트는 잘 안 쓴다.   
   
## 테스트의 목적

가성비 있는 테스트 케이스를 찾아야한다.

## 테스팅 vs 디버깅

테스팅: 결함 발견하기위한 행위   
디버깅: 결함 수정하기위한 행위   
둘은 세트다.   
<img src="https://github.com/user-attachments/assets/122c0b3e-196c-455b-a96e-b3fea5859ede" width="900" height="550">   

## 재테스팅(Re-Testing)

목적: 발견된 결함이 정상적으로 조치되었는지 확인하는 행위   
수행: 결함 발견자(테스팅 담당자)   
수행 시기: 개발자가 결함이 조치되었다고 전달할 때   
수행 방법: Regression Test(얌생이마냥 그 부분만 하지말고, 전체 다 해라 ㅋㅋ_ISO26262. 근데 너무 힘들면 중요도 상중하 나눠서 상만 하던가 그렇게해.. 근데 전체 다 하는게 좋지.)   

## Risk Based Testing

테스트 대상에 비해서 테스트 자윈이 부족할 경우, 우선순위를 나눠서 시간을 할당한다.   
```c
우선순위   40일중할당일수
상         20일
중          5일
하          5일
그리고 버퍼 5일
```
