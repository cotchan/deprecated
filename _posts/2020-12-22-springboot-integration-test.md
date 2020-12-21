---
title: Spring-Boot) 스프링 통합 테스트 
author: cotchan 
date: 2020-12-10 00:00:21 +0800 
categories: [Spring-Boot, Spring-Boot_Test]
tags: [spring-boot] 
---

## 1. @SpringBootTest, @Transactional 추가

+ `@SpringBootTest`: 스프링 컨테이너와 테스트를 함께 실행한다.
+ `@Transactional`: 아래 내용 참고

```java
@SpringBootTest
@Transactional
class MemberServiceIntegrationTest {
	//...
}
```

---

## 2. 주입은 인터페이스로 받기

+ 이렇게 하면 실제 구현체는 `Spring Cofiguration`을 통해 올라오게 됩니다.

```java
//before
class MemberServiceIntegrationTest {
    @Autowired
    MemberService memberService;

    @Autowired
    MemoryMemberRepository memberRepository;
}
```

```java
//after
class MemberServiceIntegrationTest {
    @Autowired
    MemberService memberService;

    @Autowired
    MemberRepository memberRepository;
}
```

---

## 3. @Transactional 의미

+ **테스트에 @Transactional을 달면 테스트를 실행할 때 `결과를 롤백`해줍니다.** 
    + DB에 데이터가 남아있지 않으므로 테스트를 반복적으로 실행할 수 있게 해주는 기능입니다. 
+ 트랜잭션을 실행하고 디비에 데이터를 다 넣은뒤
+ 테스트가 끝나면 롤백을 해줍니다. 
+ 그래서 DB에 넣었던 데이터가 실제로는 반영이 안됩니다.


---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
