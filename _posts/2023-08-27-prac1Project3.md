---
layout: single
title:  "애플리케이션 구현 준비"
categories: miniProject1
tag: [spring boot, jpa, java, H2 db]
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 애플리케이션 구현 준비

## 구현 요구사항

1. 예제를 단순화 하기 위해 다음 기능은 구현X   
2. 로그인과 권한 관리X   
3. 파라미터 검증과 예외 처리X   
4. 상품은 도서만 사용   
5. 카테고리는 사용X   
6. 배송 정보는 사용X   

# 애플리케이션 아키텍처

## 그림

![스크린샷 2023-08-27 appArch](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/2c0c0faf-2c33-47dc-896d-ec6ee666f9c0)   
   
## 계층형 구조 사용   

1. controller, web: 웹 계층   
2. service: 비즈니스 로직, 트랜잭션 처리   
3. repository: JPA를 직접 사용하는 계층, 엔티티 매니저 사용   
4. domain: 엔티티가 모여 있는 계층, 모든 계층에서 사용   

## 패키지 구조
- jpabook.jpashop
  - domain
  - exception
  - repository
  - service
  - web

## 개발 순서:
1. 서비스   
2. 리포지토리 계층을 개발하고   
3. 테스트 케이스를 작성해서 검증   
4. 마지막에 웹 계층 적용   
