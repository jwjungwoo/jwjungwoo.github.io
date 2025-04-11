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
<img src="https://github.com/user-attachments/assets/651799ec-8967-4c53-803b-5621380f5ba1" width="800" height="500">   


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


