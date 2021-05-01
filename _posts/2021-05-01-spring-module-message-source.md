---
title: sb3) MessageSource
author: cotchan 
date: 2021-05-01 22:30:21 +0800 
categories: [Spring-Boot3]
tags: [spring-module] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. MessageSource란?

+ ApplicationContext의 다양한 기능 중 한 가지인 MessageSource 입니다.
+ ApplicationContext는 다양한 기능을 상속하고 있습니다.
  +  **그중에 MessageSource는 `다국어 처리를 할 때 사용되는 객체`입니다. 
+ 지금부터 MessageSource의 사용 방법을 알아보겠습니다.

---

## 2. MessageSource 사용 방법

---

## 2-1. messages.properties 생성

+ **`resources 밑에 messages.properties` 파일을 만듭니다.**

+ messages.properties 파일의 내용은 `key값=value값이 기본입니다.`
  + **원한다면 {0}, {1} 등을 추가**하여 원하는 string을 parameter로 바인딩하여 사용할 수 있습니다.

```
//messages.properties

success_message=true
success_response =가입완료
fail_response=가입실패
not_existed_user=존재하지 않는 회원입니다.
```

---

## 2-2. MessageSource Bean 주입

+ Message 처리를 사용하고자 하는 Class에서 MessageSource Bean을 주입받습니다.

```java
@RestController
@RequestMapping("/api/users")
public class UsersApiController {

    private final UsersService usersService;
    private final MessageSource messageSource;

    //...
}
```

---

## 2-3. .getMessage 함수 호출

+ **`messages.properties`에 작성한 메시지를 사용할 때는 `getMessage 함수를 호출`하면 됩니다.** 

```java
messageSource.getMessage("success_message", null, Locale.getDefault())
messageSource.getMessage("success_response", null, Locale.getDefault())
messageSource.getMessage("fail_response", null, Locale.getDefault())
messageSource.getMessage("not_existed_user", null, Locale.getDefault())
```

---

## 3. 부트에서는 Bean 등록 필요 X

+ 원래는 Bean으로 등록해주어야 정상적으로 사용할 수 있습니다.
+ 그러나 스프링 부트에서는 `ResourceBundleMessageSource`라는 Bean이 이미 등록되어 있습니다.
+ 그래서  `messages`라는 이름을 가진 리소스를 자동으로 읽어 Bundle로 만들어줍니다.
+ **따라서 별다른 설정 없이도 사용할 수 있는 것입니다.**

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
    + [[Spring] MessageSource](https://velog.io/@max9106/Spring-MessageSource-4sk5oz1mjd)
    + [[Spring] 메세지소스(MessageSource)를 통한 메세지 국제화, 메세지 소스 리로딩(MessageSource Reloading)](https://engkimbs.tistory.com/717)
    + [[스프링 핵심기술] - MessageSource](https://jjingho.tistory.com/13)
