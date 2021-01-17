---
title: Spring) 왜 스프링을 쓰나요? - 2 (좋은 객체 지향 설계의 5가지 원칙(SOLID))
author: cotchan 
date: 2021-01-16 20:46:21 +0800 
categories: [Spring, Spring_Basic]
tags: [spring] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. SOLID란

+ 클린코드로 유명한 로버트 마틴이 좋은 객체 지향 설계의 5가지 원칙을 정리했습니다.

---

+ **`SRP`** : 단일 책임 원칙(Single Responsibility Principle)
+ **`OCP`** : 개방-폐쇄 원칙(Open/Closed Principle)
+ **`LSP`** : 리스코프 치환 원칙(Liskov Substitution Principle)
+ **`ISP`** : 인터페이스 분리 원칙(Interface segregation principle)
+ **`DIP`** : 의존관계 역전 원칙(Dependency inversion Principle)

---

## 1-1. 단일 책임 원칙

+ 한 클래스는 하나의 책임만 가져야 한다.
  + 하나의 책임이라는 것은 모호합니다.

+ **중요한 기준은 변경입니다.**
  + 변경이 있을 때 파급 효과가 적으면 단일 책임 원칙을 잘 따른 것입니다.
  + ex. UI 변경, 객체의 생성과 사용을 분리

---

## 1-2. 개방-폐쇄 원칙 - 가장 중요

+ 소프트웨어 요소는 **확장에는 열려**있으나 **변경에는 닫혀**있어야 한다.
+ 이런 거짓말 같은 일이? 확장을 하려면, 당연히 기존 코드를 변경해야 하는 것 아닌가요?

+ **다형성**을 활용해보면 됩니다.
  + 인터페이스를 구현한 새로운 클래스를 하나 만들어서 새로운 기능을 구현
  + 역할-구현의 분리

```java
public class MemberService {
    private MemberRepository memberRepository = new MemoryMemberRepository();
}
```

---

```java
public class MemberService {
//  private MemberRepository memberRepository = new MemoryMemberRepository();
    private MemberRepository memberRepository = new JdbcMemberRepository();
}
```

---

## 1-2. 개방-폐쇄 원칙 - 문제점

1. `MemberService` 클라이언트가 구현 클래스를 직접 선택합니다.
2. **구현 객체를 변경하려면 클라이언트 코드를 변경해야 합니다.**
3. 위 코드는 **분명히 다형성을 잘 사용했지만 OCP 원칙을 지킬 수 없습니다.**
  + 2번의 이유로 OCP가 깨진다고 할 수 있습니다.
4. 이 문제를 해결하려면?
  + 객체를 생성하고, 연관관계를 맺어주는 별도의 조립, 설정자가 필요합니다.
  + **이 별도의 설정자 역할을 스프링 컨테이너가 해주는 것입니다.**
  + **이 `OCP 원칙`을 지키기 위해 `DI`도 필요하고 `IoC` 원칙도 필요한 것입니다.**

---

## 1-3. 리스코프 치환 원칙

+ 단순히 컴파일에 성공하는 것을 넘어서는 이야기
  + **인터페이스의 규약이 있다면, 이 규약을 전부 보장해야 한다는 이야기입니다.**

+ 다형성에서 하위 클래스는 인터페이스의 규약을 다 지켜야 한다는 것, 다형성을 지원하기 위한 원칙, 인터페이스를 구현한 구현체를 믿고 사용하려면 이 원칙이 필요합니다.
  + ex. 자동차 인터페이스의 엑셀은 앞으로 가라는 기능, 뒤로 가게 구현하면 LSP 위반, 느리더라도 앞으로 가야합니다.
 
---

## 1-4. 인터페이스 분리 원칙


+ 자동차라는 인터페이스가 있을 때, 운전과 관련된 인터페이스가 있을 수 있고, 정비와 관련된 인터페이스가 있을 수 있습니다.
+ 근데 자동차 인터페이스가 너무 크니까  
  + 자동차 인터페이스를 `운전 인터페이스`와 `정비 인터페이스`로 분리합니다.
  + 그러면 클라이언트도 사용자 클라이언트를 -> `운전자 클라이언트`와 `정비사 클라이언트`로 분리할 수 있습니다.
+ **분리하면 정비 인터페이스 자체가 변해도 운전자 클라이언트에 영향을 주지 않습니다.**
+ 인터페이스가 명확해지고, 대체 가능성이 높아집니다.

---

## 1-5. 의존관계 역전 원칙 - 가장 중요

+ 프로그래머는 "`추상화에 의존`해야지, 구체화에 의존하면 안됩니다."
  + 의존성 주입은 이 원칙을 따르는 방법 중 하나입니다.

+ 쉽게 이야기해서 구현 클래스에 의존하지 말고, `인터페이스에 의존`하라는 뜻입니다.
  + **클라이언트 코드가 구현 클래스를 바라보는 게 아니라 인터페이스만 바라보라는 뜻입니다.**
  + ex. 서비스는 리포지토리 인터페이스만 바라보아야 합니다.

+ 앞에서 이야기한 **`역할에 의존`하게 해야 한다는 것과 같은 의미입니다.** 객체 세상도 클라이언트가 인터페이스에 의존해야 유연하게 구현체를 변경할 수 있습니다. 구현체에 의존하게 되면 변경이 아주 어려워집니다. 

---

## 1-5. 의존관계 역전 원칙 - 문제점 

+ OCP에서 설명한 MemberService는 인터페이스에 의존하지만, 구현 클래스도 동시에 의존합니다.
  + 무슨 말이냐면, 아래 코드에서 MemberService는 `MemberRepository`뿐만 아니라 `MemoryMemberRepository`도 안다는 것입니다. 
+ MemberService 클라이언트가 구현 클래스를 직접 선택합니다.
  
```java
public class MemberService {
    private MemberRepository memberRepository = new MemoryMemberRepository();
}
```

+ **그러므로 DIP 위반입니다.**


---

## 2. 정리

+ **객체 지향의 핵심은 `다형성`**
+ 다형성만으로는 쉽게 부품을 갈아 끼우듯이 개발할 수 없습니다.
+ 다형성만으로는 구현 객체를 변경할 때 클라이언트 코드도 함께 변경됩니다.
+ **다형성만으로는 `OCP`, `DIP`를 지킬 수 없습니다.**
+ 뭔가 더 필요합니다.

```java
public class MemberService {
//  private MemberRepository memberRepository = new MemoryMemberRepository();
    private MemberRepository memberRepository = new JdbcMemberRepository();
}
```

---

```java
public class MemberService {
    private MemberRepository memberRepository = new MemoryMemberRepository();
}
```


---

+ 출처
    + [스프링 핵심 원리 - 기본편](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%ED%95%B5%EC%8B%AC-%EC%9B%90%EB%A6%AC-%EA%B8%B0%EB%B3%B8%ED%8E%B8/dashboard)
