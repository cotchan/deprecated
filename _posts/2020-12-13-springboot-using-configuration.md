---
title: Spring-Boot) 스프링 빈과 의존관계2. 자바 코드로 직접 스프링 빈 등록하기 
author: cotchan 
date: 2020-12-13 23:15:21 +0800 
categories: [Spring-Boot, Spring-Boot_DI]
tags: [spring-boot] 
---

## 1. @Configuration 클래스

![Desktop View](/assets/img/post/spring-boot/2020-12-13-springboot-configuration-class.png)

Configuration 클래스를 만들면, 스프링이 뜰 때 이 Configuration을 읽고 `"이건 스프링 빈에 등록하라는 뜻이네."`라고 인식을 하고 MemberService와 MemberRepository를 각 각 `@Cofiguration 클래스에 등록한 메서드를 호출`해서 스프링 빈에 등록해줍니다. 

```java
@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }

    @Bean
    public MemberRepository memberRepository() {
        return new MemoryMemberRepository();
    }
}
```

단, 컨트롤러는 어쩔 수가 없습니다. 어차피 스프링이 관리해주는 것이기 때문에 @Controller나 @RestController를 사용하여 ComponentScan의 적용을 받도록 만듭니다.    

---


## 2. 생성자 주입방식

`생성자를 통해서` MemberService가 MemberController에게 주입되는 것을 생성자 주입이라고 하며, 이 방식을 가장 많이 사용합니다.         
DI에는 필드 주입, setter 주입, 생성자 주입 이렇게 3가지 방법이 있습니다. 의존관계가 실행중에 동적으로 변하는 경우는 거의 없으므로 생성자 주입을 권장합니다.    

```java
//생성자 주입 예시
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```

--- 

## 3. 직접 빈 등록을 사용하는 경우  

1. ComponentScan 방식
	+ 주로 정형화된 컨트롤러, 서비스, 리포지토리 같은 코드는 ComponentScan 사용
2. 직접 스프링빈으로 등록
	+ **정형화되지 않거나, `상황에 따라 구현 클래스를 변경해야하는 경우` 설정을 통해 스프링 빈으로 등록합니다.**


+ 사용 예시
현재는 구현클래스로 MemoryMemberRepository를 사용하지만 나중에는 DB로 교체할 예정. 이 때 기존에 사용되는 코드를 하나도 건드리지 않고 교체할 수 있습니다.

```java
//MemberRepository를 Memory => DB로 교체하는 경우
@Configuration
public class SpringConfig {

    @Bean
    public MemberService memberService() {
        return new MemberService(memberRepository());
    }

    //아래와 같이 return문 한 줄만 교체하면 됩니다.
    @Bean
    public MemberRepository memberRepository() {
        
	//return new MemoryMemberRepository();
	return new DBMemberRepository();
    }
}
```

 








---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
