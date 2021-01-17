---
title: Spring) IoC, DI, 컨테이너
author: cotchan 
date: 2021-01-18 05:15:21 +0800 
categories: [Spring, Spring_Basic]
tags: [spring] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. IoC(제어의 역전)

+ 기존 프로그램은 클라이언트 구현 객체가 스스로 필요한 서버 구현 객체를 생성하고 연결, 실행했습니다.
+ 한마디로 구현 객체가 프로그램의 제어 흐름을 스스로 조종했습니다.

+ 반면에 `AppConfig`가 등장한 이후에 구현 객체는 자신의 로직을 실행하는 역할만 담당합니다.
+ 프로그램의 제어 흐름은 이제 AppConfig가 가져갑니다. 
+ 예를 들어서 `OrderServiceImpl`은 필요한 인터페이스들을 호출하지만 어떤 구현 객체들이 실행될지는 모릅니다.
+ 프로그램에 대한 제어 흐름 권한은 모두 `AppConfig`가 가지고 있습니다. 심지어 `OrderServiceImpl`도 AppConfig가 생성합니다.

+ 그리고 AppConfig는 `OrderServiceImpl`이 아닌 `OrderService` 인터페이스의 다른 구현 객체를 생성하고 실행할 수도 있습니다.
+ 그런 사실도 모른채 `OrderServiceImpl`은 묵묵히 자신의 로직을 실행할 뿐입니다.
+ **이렇듯 프로그램의 제어 흐름을 직접 제어하는 것이 아니라 외부에서 관리하는 것을 제어의 역전(IoC)라고 합니다.



```java
public class AppConfig {

    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }

    private MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }

    public OrderService orderService() {
        return new OrderServiceImpl(memberRepository(), discountPolicy());
    }

    private DiscountPolicy discountPolicy() {
//      return new FixDiscountPolicy();
        return new RateDiscountPolicy();
    }
}
```

---

## 2. 프레임워크 vs 라이브러리

+ 프레임워크가 제가 작성한 코드를 제어하고, 대신 실행하면 그것은 프레임워크입니다. (`JUnit`)
+ 반면에 제가 작성한 코드가 직접 제어의 흐름을 담당한다면 그것은 라이브러리입니다.

---

## 3. DI(의존관계 주입)

+ `OrderServiceImpl`은 `DiscountPolicy` 인터페이스에 의존합니다. 실제 어떤 구현 객체가 사용될지는 모릅니다.
  + `의존`한다는 것은 **'내가 그 코드를 안다'는 것을 의미**합니다.
  + `의존`한다는 것은 **'내가 그 코드를 사용한다'는 것을 의미**합니다.

+ 의존관계는 **정적인 클래스 의존 관계와, 실행 시점에 결정되는 동적인 객체(인스턴스)의 의존 관계** 이 둘을 분리해서 생각해야 합니다.

---

## 3-1. 정적인 클래스 의존관계

+ 클래스가 사용하는 import 코드만 보고 의존관계를 쉽게 판단할 수 있습니다. 정적인 의존관계는 애플리케이션을 실행하지 않아도 분석할 수 있습니다.

![Desktop View](/assets/img/post/spring/2021-01-18-spring-ioc-di1.png)

+ `OrderServiceImpl`은 `MemberRepository`, `DiscountPolicy`에 의존한다는 것을 알 수 있습니다.
+ 그런데 이러한 클래스 의존관계만으로는 실제 어떤 객체가 `OrderServiceImpl`에 주입 될 지 알 수 없습니다.

```java
public class OrderServiceImpl implements OrderService {

    private final MemberRepository memberRepository;
    private final DiscountPolicy discountPolicy;

    public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
    }
}
```

---


## 3-2. 동적인 객체 인스턴스 의존관계

+ 애플리케이션 실행 시점에 실제 생성된 객체 인스턴스의 참조가 연결된 의존 관계입니다.

![Desktop View](/assets/img/post/spring/2021-01-18-spring-ioc-di2.png)

+ 애플리케이션 `실행 시점(런타임)`에 외부에서 실제 구현 객체를 생성하고 클라이언트에 전달해서 클라이언트와 서버의 실제 의존관계가 연결되는 것을 `의존관계 주입`이라고 합니다.

+ 객체 인스턴스를 생성하고, 그 참조값을 전달해서 연결됩니다.

+ 의존관계 주입을 사용하면 클라이언트 코드를 변경하지 않고, 클라이언트가 호출하는 대상의 타입 인스턴스를 변경할 수 있습니다.

+ **의존관계 주입을 사용하면 정적인 클래스 의존관계를 변경하지 않고, 동적인 객체 인스턴스 의존관계를 쉽게 변경할 수 있습니다.** 


---

## 4. IoC 컨테이너, DI 컨테이너

+ **`AppConfig`처럼 객체를 생성하고 관리하면서 의존관계를 연결해 주는 것을 IoC 컨테이너 또는 `DI 컨테이너`라고 합니다.
+ 의존관계 주입에 초점을 맞추어 최근에는 주로 DI 컨테이너라고 합니다.
+ 또는 어셈블러, 오브젝트 팩토리 등으로 불리기도 합니다. 


---

+ 출처
    + [스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)
