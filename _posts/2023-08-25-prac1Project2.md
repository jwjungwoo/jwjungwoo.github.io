---
layout: single
title:  "도메인 분석 설계"
categories: pracProject1
tag: [spring boot, jpa, java, H2 db]
author_profile: false #true면 글 안에서 내 프로필 보여줌
sidebar:
    nav: "counts"
#search: false
---

# 요구사항 분석

## 동작 화면

![스크린샷 2023-08-26 131024](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/a87a4377-1e8b-4185-8bfc-e5da25e31009)   

## 기능 목록

- 회원 기능 
  - 회원 등록   
  - 회원 조회  
- 상품 기능   
  - 상품 등록   
  - 상품 수정   
  - 상품 조회   
-주문 기능   
  -상품 주문   
  -주문 내역 조회   
  -주문 취소   
-기타 요구사항   
  -상품은 재고 관리가 필요하다.   
  -상품의 종류는 도서, 음반, 영화가 있다.   
  -상품을 카테고리로 구분할 수 있다.   
  -상품 주문시 배송 정보를 입력할 수 있다.   

# 도메인 모델과 테이블 설계

## 회원 엔티티 분석

![스크린샷 2023-08-26 memberEntity](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/8093327d-e4f6-444f-9114-73661da8a015)   
회원(Member): 이름과 임베디드 타입인 주소(Address), 그리고 주문(orders) 리스트를 가진다.   
   
주문(Order): 한 번 주문시 여러 상품을 주문할 수 있으므로 주문과 주문상품(OrderItem)은 일대다 관계이다. 주문은 상품을 
주문한 회원과 배송 정보, 주문 날짜, 주문 상태(status)를 가지고 있다. 주문 상태는 열거형을 사용했는데 주문(ORDER), 
취소(CANCEL)을 표현할 수 있다.   
   
주문상품(OrderItem): 주문한 상품 정보와 주문 금액(orderPrice), 주문 수량(count) 정보를 가지고 있다. 
(보통 OrderLine, LineItem으로 많이 표현한다.)   
   
상품(Item): 이름, 가격, 재고수량(stockQuantity)을 가지고 있다. 상품을 주문하면 재고수량이 줄어든다. 
상품의 종류로는 도서, 음반, 영화가 있는데 각각은 사용하는 속성이 조금씩 다르다.   
   
배송(Delivery): 주문시 하나의 배송 정보를 생성한다. 주문과 배송은 일대일 관계다.   
   
카테고리(Category): 상품과 다대다 관계를 맺는다. parent , child 로 부모, 자식 카테고리를 연결한다.   
   
주소(Address): 값 타입(임베디드 타입)이다. 회원과 배송(Delivery)에서 사용한다.   
   
참고: 회원 엔티티 분석 그림에서 Order와 Delivery가 단방향 관계로 잘못 그려져 있다. 양방향 관계가 맞다.   
   
참고: 회원이 주문을 하기 때문에, 회원이 주문리스트를 가지는 것은 얼핏 보면 잘 설계한 것 같지만, 객체 세
상은 실제 세계와는 다르다. 실무에서는 회원이 주문을 참조하지 않고, 주문이 회원을 참조하는 것으로 충분
하다. 여기서는 일대다, 다대일의 양방향 연관관계를 설명하기 위해서 추가했다.   

## 회원 테이블 분석

![스크린샷 2023-08-26 memberTable](https://github.com/jwjungwoo/jwjungwoo.github.io/assets/140131247/2cd3724b-8eac-4b17-ae6b-b8d4bd8a1ff3)


# 엔티티 클래스 개발

# 엔티티 설계시 주의점
