---
title: Spring-Boot) 생성자 주입방식
author: cotchan
date: 2021-01-06 08:16:21 +0800
categories: [Spring-Boot, Spring-Boot_DI]
tags: [spring-boot]    
---

## 1. 생성자 주입방식

```java
public class MemberService {
    
    private final MemberRepository memberRepository;

    //생성자가 하나밖에 없다면 @Autowired 생략 가능
    @Autowired
    public MemberService(MemberRepository memberRepository) { 
        this.memberRepository = memberRepository;
    } 
    //...
}
```

+ `lombok` 적용 시

```java
@RequiredArgsConstructor    //final이 있는 필드만 가지고 생성자를 만들어줍니다.
public class MemberService {

    private final MemberRepository memberRepository;
    
```

---

## 2. 필드 주입방식

```java
public class MemberService {
    
    @Autowired
    MemberRepository memberRepository; 
    //...
}
```

---


## 3. 생성자 주입 방식을 사용해야 하는 이유

1. **변경 불가능한 안전한 객체 생성 가능**
2. **`final` 키워드를 추가하면 컴파일 시점에 `memberRepository`를 설정하지 않은 오류를 체크할 수 있습니다.**
    + 보통 기본 생성자를 추가할 때 발견할 수 있습니다.
3. 생성자가 하나인 경우, `@Autowired`를 생략할 수 있습니다.


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)


