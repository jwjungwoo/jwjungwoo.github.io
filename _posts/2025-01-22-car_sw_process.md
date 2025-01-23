---
layout: single
title:  "자동차 SW 개발 프로세스(A-SPICE, ISO 26262 포함)"
categories: autoever
tag:
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "autoever"
#search: false
---

# 자동차 SW 개발 프로세스
## 자동차의 전장화
자동차 산업은 SDV(Software Defined Vehicle)로 전환하면서 sw가 차량의 전 방위적인 혁신을 일으키는 가장 핵심적 요소가 되고 있다. 현재 자동차에 들어가는 코드 라인은 2~3억 lines라고 하셨다.   
<img src="https://github.com/user-attachments/assets/3c9267f3-33cf-49d9-a792-d3b26716a7d2" width="500" height="400">   

## 품질
현대자동차와 같이 글로벌 자동차 OEM 업체들은 품질을 매우 중요시한다.   
품질의 정의는 뭘까? ISO 9001에서는 다음과 같이 말한다.   
✅ 품질: 명시적, 묵시적 요구를 만족시키는 제품 또는 서비스의 능력에 관한 특성   

✅ 품질의 두 가지 관점   
프로세스 품질 vs 제품 품질   
1. 프로세스 품질   
PDCA   
Plan, Do, Check, Act   
프로세스 정의, 수행, 통제, 개선을 잘하고있는가 이다.   
   
2. 제품 품질   
추적성, 일관성   
추적성 관점: 개발 단계별 산출물들이 이전 단계 산출물에 기반하여 만들어 졌는가?   
일관성 관점: 개발 단계별 산출물들이 이전 단계 산출물과 일관성을 유지하고 있는가?

## SW
✅ SW = 소스 코드와 관련된 모든 산출 문서들   
✅ 특징   
1. 구조가 눈에 보이지 않음   
2. 비 선형의 복잡한 구조(call graph 그리면 새까맣게 나옴)   
3. 닳아 없어지는 것이 아니라 끊임없이 변화한다   
4. 사람 중심 작업 (따라서 Human Error가 존재함)   
   
✅ 성공요인   
QCD   
Quality, Cost, Delivery 이다. QCD는 서로 줄다리기 관계이다.(trade off 관계)   

## SWE 표준
✅ SWE를 다루고 있는 표준   
SWEBOK (BOK: 지식에 대한 체계)   
Automotive SPICE (유럽쪽)   
ISO 26262 (안전에 특화)   
ISO/IEC/IEEE 15504 (평가 표준)   
ISO/SAE 21434 (보안에 특화)   
CMMI (미국쪽)   

## 프로세스
✅ process란?   
고객의 요구사항을 만족하는 제품을 만들기 위한 '절차/방법, 도구/장비, 인력'의 통합   
process = glue 다.   
   
✅ issue: 이미 발생한 문제   
✅ risk: 발생할 수도 있는 문제 (potential risk)   
   
✅ process 정의 방법: ETVX   
process 단계 별 수행 활동을 체계적으로 정의하는 방법   
ETVX   
Entry Criteria: 착수 기준   
Task: 세부 업무   
Verification: 완료된 작업의 검증 기준(작업 수행 여부)   
eXit Criteria: 수행된 작업의 완료 기준(완료하기 위해 요구되는 업무)   

## V-Model
✅ V-Model   
개발 생명주기의 각 단계와 그에 상응하는 sw test 단계를 매핑한 모델   
<img src="https://github.com/user-attachments/assets/a5ce23e3-9a49-4d41-9e59-a9805c684908" width="500" height="400">   
실제 현차에서 하는 방식은 IVM(Iterative V-Model)이다.   
<img src="https://github.com/user-attachments/assets/b262a5f2-f139-4695-b718-73871c33179e" width="500" height="400">   

# A-SPICE
## A-SPICE 소개
✅ OEM의 고민   
"전장 소프트웨어 시스템의 프로세스와 제품 품질이 모두 우수한 업체를 어떻게 선정하지?"   
평가하기 위한 동일 기준이 필요하고, 자동차 분야의 특성을 반영한 프로세스 품질 평가 기준이 필요하다.   
   
✅ Automotive Spice   
<img src="https://github.com/user-attachments/assets/b262a5f2-f139-4695-b718-73871c33179e" width="300" height="500">   
VDA QMC Working Group 13과 Automotive SIG(Special Interest Group)에서 주관한다. QMC는 Quality, Management, Center이다. 
A-SPICE는 국제표준은 아니다. 하지만 사실상의 표준(산업계 표준)이다. 이를 디빽토 표준(Defacto Std)이라고한다. 현재 4.0까지 나왔지만 
여전히 3.1로 하며 배운 내용도 3.1이다.   

## A-SPICE 프로세스 모델 구조
✅ A-SPICE 프로세스 모델의 구성   
<img src="https://github.com/user-attachments/assets/25d0eaa6-86b4-4662-8c61-726c910717af" width="600" height="450">   
측정 프레임워크(ISO/IEC 33020을 채택함) -> 평가 방법   
PAM(프로세스 평가 모델) -> 평가 지표   
PRM(프로세스 참조 모델) -> 평가 대상   
PAM, PRM은 A-SPICE의 고유한 것이다.   
   
✅ PRM: 평가 대상 프로세스의 범위와 프로세스 별 정의에 필요한 프로세스 식별자 ID, 이름, 목적, 성과를 설명한 모델   
A-SPICE 3.1 PRM   
<img src="https://github.com/user-attachments/assets/5aa2662e-54ce-4ce1-a788-298482f7b84f" width="600" height="450">   
ACQ.4 (Supplier Monitoring): 하부 업체와 제대로 소통하고 있는지   
SUP.8 (Configuration Management): 형상관리   
Supporting Life Cycle Processes: 나머지 활동 잘 할 수 있게 도와줌   
32개 프로세스 모두를 평가할 때도 있지만 OEM과의 협의에 따라 VDA Scope(중요한 16개)만 적용할 수 있다.   
   
✅ PAM: 프로세스를 레벨에 따른 평가하기 위한 지표를 정의한 모델   
1. 프로세스 수행 지표(= 프로세스 성과의 수행 정도)   
1-1. 기본 프랙티스   
1-2. 작업 산출물   
2. 프로세스 능력 지표(= 프로세스 속성 성과의 달성 정도)   
2-1. 일반 프랙티스   
2-2. 일반 자원   




#
