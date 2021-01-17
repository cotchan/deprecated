---
title: Spring-Boot) 스프링 빈과 의존관계1. 컴포넌트 스캔과 자동 의존관계 설정방법 
author: cotchan 
date: 2020-12-13 20:07:21 +0800 
categories: [Spring-Boot, Spring-Boot_DI]
tags: [spring-boot] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 스프링 컨테이너란? 

**스프링이 실행되면 `스프링 컨테이너`라고 하는 스프링 통이 생깁니다.**           
여기에 `@Controller` 어노테이션이 있으면, 스프링 컨테이너가 컨트롤러 객체를 생성해서 스프링 통에 넣어둡니다.    
그리고 스프링이 이 Controller 객체를 관리합니다.       

그런데 스프링이 객체를 관리 하게 되면, 객체를 다 스프링 컨테이너에 등록을 하고 스프링 컨테이너로부터 객체를 받아서 쓰도록 바꿔야 합니다.    
또한 스프링 컨테이너에 객체를 등록하면 딱 하나만 등록이 됩니다. (Default)       

---


## 1. 스프링 컨테이너와 객체를 연결하는 방법

+ 스프링이 실행되면 `@Component`와 관련되어있는 어노테이션이 붙어있는 클래스는 전부 스프링이 객체로 하나씩 생성해놓고 스프링 컨테이너에 등록을 해놓습니다.
+ 그리고 @Autowired는 컴포넌트간의 연결관계를 설정해줍니다.**생성자를 만들고 생성자에 `@Autowired`를 사용하면 됩니다.**   


---


## 2. 스프링을 사용하여 객체 간 의존관계 설정방법

1. **생성자를 만들고** 
2. **`@Autowired`를 사용하면 됩니다.**  


```java
@Controller
public class MemberController {

    private final MemberService memberService;

    @Autowired
    public MemberController(MemberService memberService) {
        this.memberService = memberService;
    }
}
```

1. 스프링 컨테이너가 뜰 때 MemberController 객체를 생성합니다. (@Controller 어노테이션이 붙어있으므로)
2. 1번에 의해 MemberController 생성자가 호출됩니다.
3. 이 때 생성자에 @Autowired가 있으면 생성자의 parameter를 `스프링 컨테이너에 등록되어 있는` MemberService를 가져다가 `연결시켜줍니다.`
 
![Desktop View](/assets/img/post/spring-boot/2020-12-13-springboot-di.png)


---


## 3. 생성자 만들기 & 생성자에 @Autowired 추가하기

```java
//Service Layer
@Service
public class MemberService {

    private final MemberRepository memberRepository;

    @Autowired
    public MemberService(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```

```java
//Persistence Layer
@Repository
public class MemoryMemberRepository implements MemberRepository {

    private static Map<Long, Member> store = new HashMap<>();
    private static long sequence = 0L;

    @Override
    public Member save(Member member) {
        member.setId(++sequence);
        store.put(member.getId(), member);
        return member;
    }
}
```

---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
