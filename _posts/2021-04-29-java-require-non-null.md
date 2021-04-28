---
title: Java) requireNonNull
author: cotchan
date: 2021-04-29 08:20:00 +0800
categories: [Java]
tags: [java]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. java.util.Obejcts

+ `java.util.Obejcts`은 JDK 7부터 Java 언어자체에서 제공하는 기능이기 때문에 다른 라이브러리 참조도 필요 없습니다.

---

## 2. requireNonNull

+ **첫번째 requireNonNull은 input 값이 `not-null이면 input을 리턴`하고 `null 이라면 NullPointException을 발생`시켜줍니다.**

```java
public class User {

    private final String userName;

    public User(String userName) {
        // JDK 7 이상에서 사용 가능
        this.userName = Objects.requireNonNull(userName);
    }
}
```

---

+ **두번째 requireNonNull은 `NullPointException의 메시지를 정할 수 있습니다.`**

```java
public User(Long seq, Email email, String password) throws RuntimeException {

    Objects.requireNonNull(email, "email must be provided");
    Objects.requireNonNull(password, "password must be provided");

    this.seq = seq;
    this.email = email;
    this.password = password;
}
```

---

+ 출처
  + [Java Validation - null check, Optional
](http://dveamer.github.io/backend/JavaValidation.html)
