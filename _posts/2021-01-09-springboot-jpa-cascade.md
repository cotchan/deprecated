---
title: Spring-Boot) JPA-Cascade 옵션 
author: cotchan 
date: 2021-01-09 21:04:21 +0800 
categories: [Spring-Boot, Spring-Boot_JPA]
tags: [spring-boot] 
---

## 1. CasCade 옵션 적용 방법

+ **아래 예시의 경우 Order가 Delivery를 관리하고, OrderItem을 관리합니다.**
+ 그러므로 `delivery` 필드와 `orderItems` 필드에 `Cascade 옵션`을 적용해줍니다.

```java
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

    //...
}
```

---

## 2. Cascade 옵션이 적용되면

+ **cascade 옵션으로 인해 `orderRepository.save(order)` 한 줄로 persist 작업이 끝납니다.**
+ cascade 옵션 때문에 아래와 같이 `order만 저장`해도 됩니다.
  + order에 orderItem과 delivery에 cascade 옵션이 있으므로
  + order에서 persist를 하면 orderItem과 delivery도 persist를 합니다.


```java
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
}
```

---

## 3. Cascade 옵션을 적용하는 경우

+ `두 가지 조건`이 만족되면 Cascade 옵션을 적용할 수 있습니다.
  + **1. 참조하는 주인이 `private owner`인 경우에만 써야합니다.**
    + private owner라는 건 `Order만 Delivery를` 갖다쓰고, `Order만 `OrderItem을` 갖다쓰는 것을 의미합니다.
    + 외부에서 따로 참조하는 게 없는 경우를 의미합니다.
  + **2. Life Cycle을 자체가 동일하게 관리될 수 있을 때 cascade 옵션을 적용합니다.**
    + Life Cycle이 `persist`할 때 같이 persist 해야 하는 경우


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
