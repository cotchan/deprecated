---
title: HelloShop) 주문 도메인 개발(Entity, Repo, Service, Test) 
author: cotchan
date: 2021-01-09 17:15:21 +0800
categories: [Project, HelloShop]
tags: # [TAG]     # TAG names should always be lowercase
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 0. 구현기능

+ 상품 주문
+ 주문 내역 조회
+ 주문 취소

---


## 1. 주문(Order) Entity 개발

+ 기능 설명
  + **생성 메서드(`createOrder()`)**
    + 주문 엔티티를 생성할 때 사용합니다. 주문 회원, 배송정보, 주문상품의 정보를 받아서 실제 주문 엔티티를 생성합니다.
  + **주문 취소(`cancel()`)**
    + 주문 취소 시 사용합니다. 주문 상태를 취소로 변경하고 주문상품에 주문 취소를 알립니다. 
    + 만약 이미 배송을 완료한 상품이면 주문을 취소하지 못하도록 예외를 발생시킵니다.
  + **전체 주문 가격 조회(`getTotalPrice()`)**
    + 주문 시 사용한 전체 주문 가격을 조회합니다.
    + 전체 주문 가격을 알려면 각각의 주문상품 가격을 알아야 합니다. 로직을 보면 연관된 주문상품들의 가격을 조회해서 더한 값을 반환합니다. (**실무에서는 주로 주문에 전체 주문 가격 필드를 두고 역정규화 합니다.)** 

```java
package jpabook.jpashop.domain;

import lombok.Getter;
import lombok.Setter;

import javax.persistence.*;
import java.time.LocalDateTime;
import java.util.ArrayList;
import java.util.List;

@Entity
@Table(name = "orders")
@Getter @Setter
public class Order {

    @Id @GeneratedValue
    @Column(name = "order_id")
    private Long id;

    //Order와 Member는 다대일 관계
    @ManyToOne(fetch = FetchType.LAZY)
    //매핑을 뭘로 할거냐를 적어주면 됩니다.
    //FK의 이름이 member_id가 됩니다.
    //연관관계의 주인이므로 얘 값을 셋팅하면 FK값이 다른 멤버로 변경이 됩니다.
    @JoinColumn(name = "member_id")
    private Member member;

    //order에 의해서 매핑이 된다는 뜻(나는 그냥 readonly 입니다)
    @OneToMany(mappedBy = "order", cascade = CascadeType.ALL)
    private List<OrderItem> orderItems = new ArrayList<>();

    /**
     * 원래는 Entity 당 각 각 Persist 호출해야 합니다.
     * persist(orderItemA)
     * persist(orderItemB)
     * persist(orderItemC)
     *
     * cascade를 셋팅하면
     * persist(order)만 해주면 위 3줄 코드는 지워도 됩니다.
     */

    //Order를 저장할 때 delivery의 값만 셋팅해놓으면 order를 저장할 때 Delivery도 persist 해줍니다.
    //원래는 각 각을 persist 해줘야함.
    //기본은 모든 Entity는 저장하고 싶으면 각 각 persist 해야합니다.
    //여기에 값만 해놓고 Order를 persist하면 얘도 persist 됩니다.
    @OneToOne(fetch = FetchType.LAZY, cascade = CascadeType.ALL)
    @JoinColumn(name = "delivery_id")
    private Delivery delivery;

    //Hibernate가 자동으로 지원해주는 type
    private LocalDateTime orderDate;

    @Enumerated(EnumType.STRING)
    private OrderStatus orderStatus; //주문 상태 [ORDER, CANCEL]

    //==연관관계 편의 메서드==//
    //Member를 set할 때 연관관계가 있는 두 개를 원자적으로 묶는 메서드
    public void setMember(Member member) {
        this.member = member;
        member.getOrders().add(this);
    }

    //==연관관계 편의 메서드==//
    public void addOrderItem(OrderItem orderItem) {
        orderItems.add(orderItem);
        orderItem.setOrder(this);
    }

    //==연관관계 편의 메서드==//
    public void setDelivery(Delivery delivery) {
        this.delivery = delivery;
        delivery.setOrder(this);
    }

    //==생성 메서드==//

    /**
     * 생성 메서드
     * 복잡한 생성에 대해서는 별도의 생성 메서드를 둡니다.
     * 이 스타일의 장점은 앞으로 '주문을 생성하는 지점'을 변경해야 하는 경우 이 메서드만 변경하면 됩니다.
     * @param member
     * @param delivery
     * @param orderItems
     * @return
     */
    //'...' 문법을통해 Order를 여러 개 넘길 수 있습니다.
    public static Order createOrder(Member member, Delivery delivery, OrderItem... orderItems) {
        Order order = new Order();
        order.setMember(member);
        order.setDelivery(delivery);
        for (OrderItem orderItem : orderItems)
        {
            order.addOrderItem(orderItem);
        }

        order.setOrderStatus(OrderStatus.ORDER);
        order.setOrderDate(LocalDateTime.now());
        return order;
    }

    //==비즈니스 로직==//
    /**
     * 주문 취소
     */
    public void cancel() {

        //배송이 이미 되버린 경우에 대한 처리(취소가 불가능)
        if (delivery.getStatus() == DeliveryStatus.COMP)
        {
            throw new IllegalStateException("이미 배송완료된 상품은 취소가 불가능합니다.");
        }

        this.setOrderStatus(OrderStatus.CANCEL);
        for (OrderItem orderItem : orderItems) {
            orderItem.cancel();
        }
    }

    //==조회 로직==//
    public int getTotalPrice() {
        int totalPrice = 0;
        for (OrderItem orderItem : orderItems)
        {
            totalPrice += orderItem.getTotalPrice();
        }

        return totalPrice;
    }
}
```


---

## 2. 주문상품(OrderItem) Entity 개발

+ 기능 설명
  + **생성 메서드(`createOrderItem()`)**
    + 주문 상품, 가격, 수량 정보를 사용해서 주문상품 엔티티를 생성합니다. 
    + 그리고 `item.removeStock(count)`를 호출해서 주문한 수량만큼 상품의 재고를 줄입니다.
  + **주문 취소(`cancel()`)**
    + `getItem().addStock(count)`를 호출해서 취소한 주문 수량만큼 상품의 재고를 증가시킵니다. 
 + **주문 가격 조회(`getTotalPrice()`)**
    + 주문 가격에 수량을 곱한 값을 반환합니다.

```java
package jpabook.jpashop.domain;

import jpabook.jpashop.domain.item.Item;
import lombok.Getter;
import lombok.Setter;

import javax.persistence.*;

@Entity
@Getter @Setter
public class OrderItem {

    @Id
    @GeneratedValue
    @Column(name = "order_item_id")
    private Long id;

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "item_id")
    private Item item;

    //하나의 Order가 여러 개의 OrderItem을 가질 수 있습니다.
    //OrderItem은 하나의 Order만 가질 수 있습니다.
    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "order_id")
    private Order order;

    private int orderPrice; //주문 가격
    private int count;  //주문 수량

    //==생성 메서드==//
    public static OrderItem createOrderItem(Item item, int orderPrice, int count) {
        OrderItem orderItem = new OrderItem();
        orderItem.setItem(item);
        orderItem.setOrderPrice(orderPrice);
        orderItem.setCount(count);

        //param으로 넘어온 만큼 아이템의 재고를 차감시켜줍니다.
        item.removeStock(count);
        return orderItem;
    }

    //==비즈니스 로직==//

    /**
     * OrderItem의 cancel의 뜻
     * 재고수량을 '원상복귀' 시켜줍니다.
     */
    public void cancel() {
        //Item의 재고를 늘리는 게 목적
        getItem().addStock(count);
    }

    //==조회 로직==//
    /**
     * totalPrice가 별도로 필요한 이유
     * '주문 가격' x '주문 수량'을 해야하므로
     * @return
     */
    public int getTotalPrice() {
        return getOrderPrice() * getCount();
    }
}
```


---

## 3. 주문 리포지토리 개발

+ 주문 리포지토리에는 주문 엔티티를 저장하고 검색하는 기능이 있습니다. 

```java
package jpabook.jpashop.repository;

import jpabook.jpashop.domain.Order;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Repository;

import javax.persistence.EntityManager;
import java.util.List;

@Repository
@RequiredArgsConstructor
public class OrderRepository {

    private final EntityManager em;

    public void save(Order order) {
        em.persist(order);
    }

    public Order findOne(Long id) {
        return em.find(Order.class, id);
    }

//    public List<Order> findAll(OrderSearch orderSearch) {}
}
```
---

## 4. 주문 서비스 개발

+ 주문 서비스는 `주문 엔티티`와 `주문 상품 엔티티`의 `비즈니스 로직을 활용`해서 아래의 기능을 제공합니다.
  + 주문
  + 주문 취소
  + 주문 내역 검색

+ **주문(`order()`)**
  + 주문하는 회원 식별자, 상품 식별자, 주문 수량 정보를 받아서 실제 주문 엔티티를 생성한 후 저장합니다.
+ **주문 취소(`cancelOrder()`)**
  + 주문 식별자를 받아서 주문 엔티티를 조회한 후 주문 엔티티에 주문 취소를 요청합니다.
+ **주문 검색(`findOrders()`)**
  + `OrderSearch`라는 검색 조건을 가진 객체로 주문 엔티티를 검색합니다.

```java
package jpabook.jpashop.service;

import jpabook.jpashop.domain.Delivery;
import jpabook.jpashop.domain.Member;
import jpabook.jpashop.domain.Order;
import jpabook.jpashop.domain.OrderItem;
import jpabook.jpashop.domain.item.Item;
import jpabook.jpashop.repository.ItemRepository;
import jpabook.jpashop.repository.MemberRepository;
import jpabook.jpashop.repository.OrderRepository;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class OrderSerivce {

    private final OrderRepository orderRepository;
    private final MemberRepository memberRepository;
    private final ItemRepository itemRepository;

    /**
     * 주문
     */
    @Transactional
    public Long order(Long memberId, Long itemId, int count) {

        //엔티티 조회
        Member member = memberRepository.findOne(memberId);
        Item item = itemRepository.findOne(itemId);

        //배송정보 생성
        Delivery delivery = new Delivery();
        delivery.setAddress(member.getAddress());

        //주문상품 생성
        OrderItem orderItem = OrderItem.createOrderItem(item, item.getPrice(), count);

        //주문 생성
        Order order = Order.createOrder(member, delivery, orderItem);

        //주문 저장
        orderRepository.save(order);

        return order.getId();
    }

    /**
     * 주문 취소
     */
    @Transactional
    public void cancelOrder(Long orderId) {

        //주문 엔티티 조회
        Order order = orderRepository.findOne(orderId);

        //주문 취소
        order.cancel();
    }

    /**
     * 검색
     */
//    public List<Order> findOrders(OrderSearch orderSearch) {
//        return orderRepository.findAll(orderSearch);
//    }
}
```

---

## 5. 주문 기능 테스트

+ 테스트 요구사항
  + 상품 주문이 성공해야 합니다.
  + 상품을 주문할 때 재고 수량을 초과하면 안 됩니다.
  + 주문 취소가 성공해야 합니다.

```java
package jpabook.jpashop.service;

import jpabook.jpashop.domain.Address;
import jpabook.jpashop.domain.Member;
import jpabook.jpashop.domain.Order;
import jpabook.jpashop.domain.OrderStatus;
import jpabook.jpashop.domain.item.Book;
import jpabook.jpashop.domain.item.Item;
import jpabook.jpashop.exception.NotEnoughStockException;
import jpabook.jpashop.repository.OrderRepository;
import org.junit.Assert;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.transaction.annotation.Transactional;

import javax.persistence.EntityManager;

@RunWith(SpringRunner.class)
@SpringBootTest
@Transactional
public class OrderSerivceTest {

    @Autowired
    EntityManager em;

    @Autowired
    OrderSerivce orderSerivce;

    @Autowired
    OrderRepository orderRepository;

    @Test
    public void 상품주문() throws Exception {
        //given
        Member member = createMember();

        Item item = createItem("시골 JPA", 10000, 10);

        int orderCount = 2;

        //when
        Long orderId = orderSerivce.order(member.getId(), item.getId(), orderCount);

        //then
        Order getOrder = orderRepository.findOne(orderId);

        Assert.assertEquals("상품 주문시 상태는 ORDER", OrderStatus.ORDER, getOrder.getOrderStatus());
        Assert.assertEquals("주문한 상품 종류 수가 정확해야 한다.", 1, getOrder.getOrderItems().size());
        Assert.assertEquals("주문 가격은 가격 * 수량이다.", item.getPrice() * orderCount, getOrder.getTotalPrice());
        Assert.assertEquals("주문 수량만큼 재고가 줄어야 합니다.", 8, item.getStockQuantity());
    }

    @Test(expected = NotEnoughStockException.class)
    public void 상품주문_재고수량초과() throws Exception {
        //given
        Member member = createMember();
        Item item = createItem("시골 JPA", 10000, 10);

        int orderCount = 11;

        //when
        orderSerivce.order(member.getId(), item.getId(), orderCount);

        //then
        Assert.fail("재고 수량 부족 예외가 발생해야 합니다.");
    }

    @Test
    public void 주문취소() throws Exception {
        //given
        Member member = createMember();
        Item item = createItem("시골 JPA", 10000, 10);

        int orderCount = 2;
        Long orderId = orderSerivce.order(member.getId(), item.getId(), orderCount);

        //when
        orderSerivce.cancelOrder(orderId);

        //then
        Order getOrder = orderRepository.findOne(orderId);

        Assert.assertEquals("주문 취소 시 상태는 CANCEL 입니다.", OrderStatus.CANCEL, getOrder.getOrderStatus());
        Assert.assertEquals("주문이 취소된 상품은 그만큼 재고가 증가해야 합니다.", 10, item.getStockQuantity());
    }

    private Item createItem(String name, int price, int stockQuantity) {
        Item item = new Book();
        item.setName(name);
        item.setPrice(price);
        item.setStockQuantity(stockQuantity);
        em.persist(item);
        return item;
    }

    private Member createMember() {
        Member member = new Member();
        member.setName("회원1");
        member.setAddress(new Address("서울", "강가", "123-123"));
        em.persist(member);
        return member;
    }
}
```

---

## 6. 주문 검색 기능 개발

+ **JPA에서 `동적 쿼리`를 어떻게 해결해야 하는가?**에 대한 문제

![Desktop View](/assets/img/post/helloShop/2021-01-09-orderSearch-service.png)

---

## 6-1. 검색 조건 파라미터 OrderSearch

+ repository 패키지 내에 추가 

```java
package jpabook.jpashop.repository;

import jpabook.jpashop.domain.OrderStatus;
import lombok.Getter;
import lombok.Setter;

@Getter @Setter
public class OrderSearch {

    private String memberName;  //회원 이름
    private OrderStatus orderStatus;    //주문 상태[ORDER, CANCEL]
}
```

## 6-2. 검색 로직 추가

+ `findAll(OrderSearch orderSearch)` 메서드는 검색 조건에 동적으로 쿼리를 생성해서 주문 엔티티를 조회합니다. 

+ 검색을 추가한 주문 리포지토리 코드

```java
package jpabook.jpashop.repository;

import jpabook.jpashop.domain.Order;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Repository;

import javax.persistence.EntityManager;
import java.util.List;

@Repository
@RequiredArgsConstructor
public class OrderRepository {

    private final EntityManager em;

    public void save(Order order) {
        em.persist(order);
    }

    public Order findOne(Long id) {
        return em.find(Order.class, id);
    }

    //검색 로직 추가
    public List<Order> findAll(OrderSearch orderSearch) {
        //... 검색 로직
    }
}
```

---

+ `QueryDSL`을 사용하는 것이 가장 바람직하지만, 현재는 JPQL에 단순히 String을 추가하는 방식으로 작성했습니다.

```java
public List<Order> findAll(OrderSearch orderSearch) { 
    //language=JPAQL
    String jpql = "select o From Order o join o.member m"; 
    boolean isFirstCondition = true;

    //주문 상태 검색
    if (orderSearch.getOrderStatus() != null) {
        if (isFirstCondition) {
            jpql += " where";
            isFirstCondition = false;
        } else {
            jpql += " and";
        }   
        jpql += " o.status = :status"; 
    }

    //회원 이름 검색
    if (StringUtils.hasText(orderSearch.getMemberName())) {
        if (isFirstCondition) {
            jpql += " where";
            isFirstCondition = false;
        } else {
            jpql += " and";
        }
        jpql += " m.name like :name"; 
    }
  
    TypedQuery<Order> query = em.createQuery(jpql, Order.class) 
            .setMaxResults(1000); //최대 1000건

    if (orderSearch.getOrderStatus() != null) {
        query = query.setParameter("status", orderSearch.getOrderStatus());
    }

    if (StringUtils.hasText(orderSearch.getMemberName())) {
        query = query.setParameter("name", orderSearch.getMemberName()); 
    }
    
    return query.getResultList(); 
}
```


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)


