---
title: Spring) 왜 스프링을 쓰나요? - 4 (AppConfig의 등장)
author: cotchan 
date: 2021-01-18 04:15:21 +0800 
categories: [Spring, Spring_Basic]
tags: [spring] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 관심사의 분리

+ 배우는 본인의 역할인 배역을 수행하는 것에만 집중해야 합니다.
+ 디카프리오는 어떤 여자 주인공이 선택되더라도 똑같이 공연을 할 수 있어야 합니다.
+ 공연을 구성하고, 담당 배우를 섭외하고, 역할에 맞는 배우를 지정하는 책임을 담당하는 별도의 공연 기획자가 필요합니다.
+ **공연 기획자를 만들고, 배우와 공연 기획자의 책임을 확실히 분리해야 합니다.** 

---

## 2. AppConfig의 등장

+ 애플리케이션의 전체 동작 방식을 구성(config)하기 위해, `구현 객체를 생성`하고, `연결`하는 책임을 가지는 별도의 설정 클래스를 만듭니다.

```java
//AppConfig.java
package hello.core;

import hello.core.discount.DiscountPolicy;
import hello.core.discount.FixDiscountPolicy;
import hello.core.discount.RateDiscountPolicy;
import hello.core.member.MemberRepository;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import hello.core.member.MemoryMemberRepository;
import hello.core.order.OrderService;
import hello.core.order.OrderServiceImpl;

/**
 * '역할' - '구현'에 대해
 * 설계에 대한 정보가 AppConfig 구성정보에 전부 드러나게 됩니다.
 */
public class AppConfig {

    /**
     * MemberService의 역할
     * @return
     */
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    /**
     * MemberRepository의 역할
     * @return
     */
    private MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    /**
     * OrderService의 역할
     * @return
     */
    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    /**
     * DiscountPolicy의 역할
     * @return
     */
    private DiscountPolicy discountPolicy() {
//      return new FixDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```

+ **AppConfig는 애픒리케이션의 실제 동작에 필요한 `구현 객체를 생성`합니다.**
  + `MemberServiceImpl` 
  + `MemoryMemberRepository`
  + `OrderServiceImpl`
  + `FixDiscountPolicy`

+ **AppConfig는 생성한 객체 인스턴스의 참조(레퍼런스)를 `생성자를 통해서 주입(연결)`해줍니다.**
  + `MemberServiceImpl` => `MemoryMemberRepository`
  + `OrderServiceImpl` => `MemoryMemberRepository`, `FixDiscountPolicy`


---

## 2-1. MemberServiceImpl - 생성자 주입

```java
//MemberServiceImpl.java
package hello.core.member;

public class MemberServiceImpl implements MemberService {

    private final MemberRepository memberRepository;

    /**
     * 생성자를 통해 어떤 MemberRepository가 주입될지를 결정해줍니다.
     * @param memberRepository
     */
    public MemberServiceImpl(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }

    @Override
    public void join(Member member) {
        memberRepository.save(member);
    }

    @Override
    public Member findMember(Long memberId) {
        return memberRepository.findById(memberId);
    }
}
```

+ 설계 변경으로 `MemberServiceImpl`은 `MemoryMemberRepository`를 의존하지 않습니다.
+ 단지 `MemberRepository` 인터페이스만 의존합니다.
+ `MemberServiceImpl` 입장에서 생성자를 통해 어떤 구현 객체가 들어올지(주입될지)는 알 수 없습니다.
+ `MemberServiceImpl`의 생성자를 통해서 어떤 구현 객체를 주입할지는 오직 외부(`AppConfig`)에서 결정됩니다.
+ **`MemberServiceImpl`은 이제부터 `의존관계에 대한 고민`은 `외부에` 맡기고 `실행에만 집중`하면 됩니다.**

---

## 2-2. 클래스 다이어그램

+ AppConfig 적용 후(변경 후) 

![Desktop View](/assets/img/post/spring/2021-01-18-spring-ioc-appconfig1.png)


+ 객체의 생성과 연결은 `AppConfig`가 담당합니다.
+ **`DIP 완성`**: `MemberServiceImpl`은 `MemberRepository`인 인터페이스에만 의존하면 됩니다. 이제 구현체 클래스를 몰라도 됩니다.
+ **`관심사의 분리`**: 객체를 생성하고 연결하는 역할과 실행하는 역할이 명확히 분리되었습니다.

---

## 2-3. 회원 객체 인스턴스 다이어그램

+ AppConfig 적용 후(변경 후)

![Desktop View](/assets/img/post/spring/2021-01-18-spring-ioc-appconfig2.png)

+ `appConfig` 객체는 `memoryMemberRepository` 객체를 생성하고 그 참조값을 `memberServiceImpl`을 생성하면서 생성자로 전달합니다.
+ 클라이언트인 `memberServiceImpl` 입장에서 보면 의존관계를 마치 외부에서 주입해주는 것 같다고 해서 DI(의존성 주입)이라고 합니다.

---

## 2-4. OrderServiceImpl - 생성자 주입

```java
//OrderServiceImpl
package hello.core.order;

import hello.core.discount.DiscountPolicy;
import hello.core.member.Member;
import hello.core.member.MemberRepository;

public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }

    @Override
    public Order createOrder(Long memberId, String itemName, int itemPrice) {
        Member member = memberRepository.findById(memberId);
        int discountPrice = discountPolicy.discount(member, itemPrice);
        
        return new Order(memberId, itemName, itemPrice, discountPrice);
    }
}
```

+ 설계 변경으로 `OrderServiceImpl`은 `FixDiscountPolicy`를 의존하지 않습니다.
+ 단지 `DiscountPolicy` 인터페이스만 의존합니다.
+ `OrderServiceImpl` 입장에서 생성자를 통해 어떤 구현 객체가 들어올지(주입될지)는 알 수 없습니다.
+ `OrderServiceImpl`의 생성자를 통해서 어떤 구현 객체를 주입할지는 오직 외부(AppConfig)에서 결정합니다.
+ `OrderServiceImpl`은 이제부터 실행에만 집중하면 됩니다.
+ `OrderServiceImpl`에는 `MemoryMemberRepository`, `FixDiscountPolicy` 객체의 의존관계가 주입됩니다.


---


## 2-5. AppConfig 실행

+ 사용 클래스 - `MemberApp`

```java
package hello.core;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import hello.core.order.OrderService;

public class MemberApp {

    public static void main(String[] args) {

//      MemberService memberService = new MemberServiceImpl();
        AppConfig appConfig = new AppConfig();
        MemberService memberService = appConfig.memberService();

        Member member = new Member(1L, "memberA", Grade.VIP);
        memberService.join(member);

        Member findMember = memberService.findMember(1L);

        System.out.println("new member = " + member.getName());
        System.out.println("findMember = " + findMember.getName());
    }
}
```

---

+ `MemberServiceTest`

```java
package hello.core.member;

import hello.core.AppConfig;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class MemberServiceTest {

    MemberService memberService;

    @BeforeEach
    public void beforeEach() {
        AppConfig appConfig = new AppConfig();
        memberService = appConfig.memberService();
    }

    @Test
    void join() {
        //given
        Member member = new Member(1L, "memberA", Grade.VIP);

        //when
        memberService.join(member);
        Member findMember = memberService.findMember(1L);

        //then
        Assertions.assertThat(member).isEqualTo(findMember);
    }
}
```

---

+ 사용 클래스 - `OrderApp`

```java
package hello.core;

import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberService;
import hello.core.order.Order;
import hello.core.order.OrderService;

public class OrderApp {

    public static void main(String[] args) {

//      MemberService memberService = new MemberServiceImpl(null);
//      OrderService orderService = new OrderServiceImpl(null, null);

        AppConfig appConfig = new AppConfig();
        MemberService memberService = appConfig.memberService();
        OrderService orderService = appConfig.orderService();

        Long memberId = 1L;
        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 20000);

        System.out.println("order = " + order);
    }
}
```

---

+ `OrderServiceTest`

```java
package hello.core.order;

import hello.core.AppConfig;
import hello.core.member.Grade;
import hello.core.member.Member;
import hello.core.member.MemberService;
import hello.core.member.MemberServiceImpl;
import org.assertj.core.api.Assertions;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;

public class OrderServiceTest {

    MemberService memberService;
    OrderService orderService;

    @BeforeEach
    public void beforeEach() {
        AppConfig appConfig = new AppConfig();
        memberService = appConfig.memberService();
        orderService = appConfig.orderService();
    }

    @Test
    public void createOrder() {
        Long memberId = 1L;

        Member member = new Member(memberId, "memberA", Grade.VIP);
        memberService.join(member);

        Order order = orderService.createOrder(memberId, "itemA", 10000);
        Assertions.assertThat(order.getDiscountPrice()).isEqualTo(1000);
    }
}
```

---

## 3. 정리

+ **`AppConfig`를 통해서 관심사를 확실하게 분리합니다.**
+ 배역, 배우를 생각해보면 됩니다.
  + AppConfig는 공연 기획자입니다.
  + AppConfig는 구현체 클래스를 선택합니다. 배역에 맞는 담당 배우를 선택합니다. **애플리케이션이 어떻게 동작해야 할지 전체 구성을 책임집니다.**

+ 이제 각 배우들은 담당 기능을 실행하는 책임만 지면 됩니다.
+ `OrderServiceImpl`은 기능을 실행하는 책임만 지면 됩니다.


---

+ 출처
    + [스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)
