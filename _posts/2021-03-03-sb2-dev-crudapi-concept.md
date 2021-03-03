---
title: sb2) 9. 등록/수정/조회 API 만들기 (개념)
author: cotchan 
date: 2021-03-03 23:31:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. CRUD API를 만들려면

+ **API를 만들기 위해서는 `총 3개의 클래스가 필요`합니다.**

1. **`Request` 데이터를 받을 `DTO`**
2. **API 요청을 받을 `Controller`**
3. **트랜잭션, 도메인 기능 간의 순서를 보장하는 `Service`**
  + Service에서 비즈니스 로직을 처리해야 한다고 하지만 그렇지 않습니다.
  + **Service는 트랜잭션, 도메인 간 `순서 보장`의 역할만 합니다.**

---

## 2. DTO

---

## 3. Controller

---

## 4. Service

---

## 5. Service는 순서 보장 역할만

+ order, billing, delivery가 각자 본인의 취소 이벤트 처리를 하며 서비스 메소드는 **트랜잭션과 도메인 간의 순서만 보장해줍니다.**

```java
@Transactional
public Order cancelOrder(int orderId) {

    Orders order = ordersRepository.findById(orderId);
    Billing billing = billingRepository.findByOrderId(orderId);
    Delivery delivery = deliveryRepository.findByOrderId(orderId);

    delivery.cancel();

    order.cancel();
    billing.cancel();

    return order;
}
```


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
