---
title: HelloShop) 도메인 분석 설계
author: cotchan
date: 2021-01-06 08:16:21 +0800
categories: [Project, HelloShop]
tags: [helloshop]     # TAG names should always be lowercase
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**   

---

## 1. 도메인 모델과 테이블 설계

![Desktop View](/assets/img/post/helloShop/2021-01-06-analysis-design1.png)

+ `회원`, `주문`, `상품`의 관계
    + **주문과 상품은 `다대다 관계`입니다.**
        + 회원은 여러 상품을 주문할 수 있습니다.
        + 그리고 한 번에 주문할 때 여러 상품을 선택할 수 있습니다.
        + **그러나 다대다 관계는 관계형 데이터베이스뿐만 아니라 엔티티에서도 거의 사용하지 않습니다.**
        + **그러므로 `주문상품`이라는 엔티티를 추가해서 `다대다 관계를 일대다, 다대일 관계로` 풀어냈습니다.**

+ 상품 분류
    + 상품은 도서, 음반, 영화로 구분되는데 상품이라는 공통 속성을 사용하므로 상속 구조로 표현

---

## 2. (회원)엔티티 분석

![Desktop View](/assets/img/post/helloShop/2021-01-06-analysis-design2.png)

1. **회원(Member)**
    + 이름과 임베디드 타입인 주소(Address), 그리고 주문(order)리스트를 가집니다.

2. **주문(Order)**
    + 한 번 주문시 여러 상품을 주문할 수 있으므로 `주문과 주문상품`(OrderItem)은 `일대다 관계`입니다.
    + 주문은 상품을 주문한 회원과 배송 정보, 주문 날짜, 주문 상태(status)를 가지고 있습니다.
    + 주문 상태는 열거형을 사용, 주문(ORDER), 취소(CANCEL)을 표현할 수 있습니다.

3. **주문상품(OrderItem)**
    + 주문한 상품 정보와 주문 금액(orderPrice), 주문 수량(count) 정보를 가지고 있습니다. 

4. **상품(Item)**
    + 이름, 가격, 재고수량(stockQuantity)을 가지고 있습니다. 
    + 상품을 주문하면 재고수량이 줄어듭니다. 

5. **배송(Delivery)**
    + 주문시 하나의 배송 정보를 생성합니다. `주문과 배송`은 `일대일 관계`입니다.

6. **카테고리(Category)**
    + `상품과 카테고리`는 `다대다 관계`를 맺습니다. `parent`, `child`로 부모, 자식 카테고리를 연결합니다.
 
7. **주소(Address)**
    + 값 타입(임베디드 타입)입니다. 회원과 배송(Delivery)에서 사용합니다.

+ 참고사항
    + 회원이 주문을 하기 때문에, 회원이 주문리스트를 가지는 것은 잘 설계한 것 같지만, 객체 세상은 실제 세계와는 다릅니다. **실무에서는 회원이 주문을 참조하지 않고, 주문이 회원을 참조하는 것으로 충분합니다.** 여기서는 일대다, 다대일의 양방향 연관관계를 설명하기 위해서 추가했습니다.

---

## 3. (회원)테이블 분석

![Desktop View](/assets/img/post/helloShop/2021-01-06-analysis-design3.png)

1. **MEMBER**
    + 회원 엔티티의 Address 임베디드 타입 정보가 회원 테이블에 그대로 들어갔습니다. (DELIVERY 테이블도 마찬가지) 

2. **ITEM**
    + SingleTable 전략 사용. 앨범, 도서, 영화 타입을 통합해서 하나의 테이블로 만들었습니다. `DTYPE`컬럼으로 타입을 구분합니다.

---

## 4. 연관관계 매핑 분석
 
1. **회원과 주문(Member & Order)**
    + **`일대다`, `다대일` `양방향 관계`입니다.**
    + 따라서 `연관관계의 주인`을 정해야 합니다. 이 때 `외래 키가 있는 곳`(주문)을 연관관계의 주인으로 정하는 것이 좋습니다.
    + 그러므로 `Order.member`를 `ORDER_MEMBER_ID` `외래키와 매핑`합니다.

2. **주문상품과 주문(OrderItem & Order)**
    + **`다대일` `양방향 관계`입니다.**
    + `외래 키`가 주문상품에 있으므로 주문상품이 `연관관계의 주인`입니다.
    + 그러므로 `OrderItem.order`를 `ORDER_ITEM.ORDER_ID` `외래키와 매핑`합니다.

3. **주문상품과 상품(OrderItem & Item)**
    + **`다대일` `단방향 관계`입니다.**
    + `OrderItem.item`을 `ORDER_ITEM.ITEM_ID` `외래키와 매핑`합니다.

4. **주문과 배송(Order & Delivery)**
    + **`일대일` `양방향 관계`입니다.**
    + `Order.delivery`를 `ORDERS.DELIVERY_ID` `외래키와 매핑`합니다.

5. **카테고리와 상품(Category & Item)**
    + `@ManyToMany`를 사용해서 매핑했습니다.
    + 실무에서는 @ManyToMany는 사용ㅎ금지. 여기서는 다대다 관계를 예제로 보여주기 위해 추가했습니다. 


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)


