---
title: TIL) @Transactional이 적용이 안 되는경우
author: cotchan
date: 2021-07-11 15:40:00 +0800
categories: #
tags: [til]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 문제

+ 아래와 같이 `userRepo.save`와 `userPwRepo.save`를 묶고 트랜잭션을 날림.
+ 의도적으로 `userPwRepo.save`에서 오류가 나게 하였지만 userRepo.save가 commit이 되는 문제

```java
//UserService

    public void joinService(UserVO user) throws Exception {

        //...

        //insert into table using JPA
        saveUser(user, vq, encodedPassword);
    }

     @Transactional 
     public void saveUser(UserVO userVO, VerifyQuestion vq, String encodedPassword) { 
         try { 
             User newUser = User.builder().seq(null).userId(userVO.getId()).nickname(userVO.getNickname()) 
                     .verifyQuestion(vq).verifyAnswer(userVO.getVerifyAnswer()).role(Role.USER).build(); 
             User resUser = userRepo.save(newUser); 
  
             UserPassword newUserPassword = UserPassword.builder().seq(null).user(resUser). 
                     password(encodedPassword).build(); 
             userPwRepo.saveAndFlush(newUserPassword); 
         } catch (Exception e) { 
             throw new RuntimeException(e); 
         } 
     } 
```

---

## 2. 문제 원인

+ **@Transactional이 붙지 않은 메서드에서 @Transactional이 붙은 메서드를 호출하였음**
  + Controller가 실제로 호출하는 메서드는 `joinService()` 였음.
  + 그래서 @Transactional이 붙은 `saveUser` 메서드에 @Transactional이 적용되지 않음

---

+ @Transactional 는 Proxy 기반이고 AOP로 구성되어 있다. 
+ **이는 Method 혹은 Class가 실행되기 전/후 등의 단계에서 자동으로 트랜잭션을 묶는다는 의미**

+ **스프링에서의 `@Transactional`은 `인스턴스에서 처음으로 호출하는 메서드나 클래스의 속성을 따라가게 된다.`**
+ **그래서 동일한 Bean안에 상위 메서드가 @Transactional 가 없으면 하위에는 선언이 되었다 해도 전이되지 않는다.**
+ 때문에 별도의 클래스(또는 Bean)으로 분리하거나 아니면 상위 메서드에 @Transactional 을 걸어주어야 한다. 
  + 이때 하위 메서드에 @Transactional 은 생략해도 된다.

---

## 3. 해결 방법

+ **Controller가 호출하는 `joinService` 메서드에 @Transactional 어노테이션을 추가하여 해결** 

```java
//UserService

    @Transactional
    public void joinService(UserVO user) throws Exception {

        //...

        //insert into table using JPA
        saveUser(user, vq, encodedPassword);
    }
```

---

+ 출처
  + [[spring] @Transactional 작동 안할때 확인해봐야 할 것](https://lemontia.tistory.com/878)
