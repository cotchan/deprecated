---
title: TIL) Parse Snake case JSON in Spring Boot
author: cotchan
date: 2021-07-18 20:31:00 +0800
categories: #
tags: [til]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 문제

+ **Request-body의 `snake case` KEY값을 `camel case로 파싱`해줘야하는 문제**

---

## 2. 해결 방법

---

## 2-1. @JsonNaming

+ **DTO class에 `@JsonNaming` 어노테이션을 붙여줍니다.**

```java
@JsonNaming(PropertyNamingStrategy.SnakeCaseStrategy.class)
public class UserDTO {
    private String firstName ;
    private String lastName ;
    private String permanentAddress;
}
```

---

## 2-2. application.property

+ **`application.property`에 아래 코드를 추가해줍니다.**

```
spring.jackson.property-naming-strategy=SNAKE_CASE
```

---

+ 출처
  + [Parse Snake case JSON in Spring Boot](https://medium.com/@bhanuchaddha/parse-snake-case-json-in-spring-boot-66b42627a791)
