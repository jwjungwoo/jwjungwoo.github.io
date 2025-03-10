---
layout: single
title:  "소프트웨어 요구사항 분석 및 설계"
categories: autoever
tag: 
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 느낀점
✅ 요구사항: 앞으로있을 프로젝트에 기능적인 측면도 중요하지만 요구사항도 잘 분석하자.. 중요하다.   
✅ 회사의 수준: 회사의 수준이 업그레이드됨에 따라 sw 요구사항을 정확하게 파악하려한다.

# 소프트웨어 개발과 요구사항
## 요구사항 개요
✅ SW 개발 4단계   
(기획) -> 요구사항 -> 설계 -> 코딩 -> 테스팅 -> (유지보수)   
요구사항: "그래서 누가 산대"   
미국의 sw 성공률 33%, 우리나라 성공률 97%. 미국은 실패를 두려워하지 않는다.   
   
✅ 요구사항의 정의   
이해관계자들이 제공될 시스템에 요구하는 기능과 제약사항을 정의   

## 요구사항 관리
✅ 요구사항 관리   
시장 적기. 따라서 요구사항 분석은 끊임없이 이루어져야함.   
만들다 아니다 싶으면 버림.   
   
우리가 공부했던 V & V model은 시스템 품질 기반 모델이다. 150kg 남성같다 하셨다. 펀치 한방한방이 쎄지만 모션이 무겁다.   
   
✅ 정의된 기능의 절반 이상은 한번도 사용되지 않는다.   
그니까 분석 잘하자.   

## sw 개발의 실제
✅ sw 규모와 실패 확률은 비례한다.   
   
✅ sw 결함   
sw 결함 원인 분포표를 보면 56퍼센트가 요구사항 결함이다. 초기 요구관리를 할수록 비용적으로 많이 아낄 수 있다.   
   
# sw 요구공학
## sw 요구공학의 특징
✅ sw 공학 안에 sw 요구공학이 있다. (위에서 다룬 4단계)   
   
✅ 요구사항 검증방법   
사람이 보통 검증을 한다.   

✅ well-defined 요구사항 관리를 위한 6P's   
1. Purpose: 프로젝트 목적과 요구사항 범위에 대한 식별   
2. Participants: 이해관계자 (고객)과의 의사개선   
3. Precision: 고객의 기대 만족   
4. Priority: 고객의 가치 평가기준 정의   
5. Product: 완전하고 명확한 기능과 품질 정의   
6. Process: 체계적이고 내재화된 요구공학 프로세스의 적용   
   
# 비즈니스 중심 요구사항 추출
## 요구사항 추출의 개요
✅ 엘레베이터가 느려!   
누군가는 속도를 높이려고 기어 수를 높이려하는데, 단 하루만에 거울을 달고 해결했다.   
   
✅ 요구사항의 계층   
why(추상화) -> what(가시화) ->, behavior(정형화)   

## 비즈니스 요구사항 추출
✅ SGS Cycle   
Scope-> Goal -> Stakeholders -> Scope...   

# 이해관계자 중심 요구사항 추출
## 이해관계자 요구사항 추출
✅ 스님들에게 빗을 팔라고??   
-> 스님의 이해관계를 찾아낸 경우   
-> 스님의 이해관계자의 이해를 찾아낸 경우   

## 이해관계자 요구사항 추출 방법론
✅ 전통적 요구사항 추출 기법
1. interview   
2. brainstroming   
3. survey/questionnare   
개방형 및 폐쇄형 질문 목록   
4. observation

# 가치 기반 요구사항 분석
## 가치 기반 요구공학 (KANO) 실습
<img src="https://github.com/user-attachments/assets/d06cb593-a64a-4094-ba80-443f13b5aaa3" width="600" height="510">   

## 요구사항 모델 유형
✅ UML 같은 툴을 사용할 때 사용자는 복잡한 Diagrams를 이해 못한다. 이는 Communication을 방해한다. 따라서 SW-based model을 사용하는 것 보다, User-based model을 사용하는 것이 좋다. 

## 깨비책방 관리
<img src="https://github.com/user-attachments/assets/273ee74b-d836-4d3b-9d5d-fea2d8c510e9" width="400" height="1050">   

## 제대로 기록하자
대기업, 중소기업들의 인력은 계속 바뀌는데 뭔가 제대로 기록된건 없으니 다시 추출.. 추출은 잘 하는데 정작 기록을 제대로 안 함.

## 요구사항 우선순위화
✅ VOP 기법   
각각의 요구사항에 대해 5개 정도의 V(가치) 각각을 측정한 합을 수치화하여 요구사항을 우선순위 메길수 있다.   
<img src="https://github.com/user-attachments/assets/d5e57393-d6d6-4ff7-b123-50d15eaee86c" width="1000" height="400">   
판매, 소비자만족도, 마케팅, 전략, 무결성이 V 기준들이다.   

# 유스케이스
## 깨비책방 유스케이스
✅유스케이스 일부   
<img src="https://github.com/user-attachments/assets/2f600996-dba6-4f8c-a8b2-fa84c71e595f" width="600" height="600">   

   
✅ 우리팀이 준 피드백 일부   
<img src="https://github.com/user-attachments/assets/461062c6-29e9-4256-b8c7-1d7e3d12bcf4" width="600" height="700">   
   
✅ 우리팀이 받은 피드백 일부   
<img src="https://github.com/user-attachments/assets/bad23c85-de6b-42e6-808b-2e09b05ae186" width="600" height="700">   
   
✅ 서비스 처리 순서   
<img src="https://github.com/user-attachments/assets/c7a8b90d-cb60-40a3-b1a6-3fc5f4555cc8" width="600" height="1000">   
<img src="https://github.com/user-attachments/assets/a81b45a9-01f6-4022-ab23-f4e02d38fa38" width="600" height="750">   

# 요구사항 명세 유형
✅ informal specification   
자연어 기반의 서술, 작업흐름도 등의 그림 중심 작성   
불충분, 일관성 결여, 내용의 모호성   
   
✅ semi-formal specification   
우리가 한 UML이 여기에 속한다. 이외에도 여러 명세법들이 존재한다.   
   
✅ formal specification   
Z, VDM (뭔지는 잘 모르겠다!!)   
