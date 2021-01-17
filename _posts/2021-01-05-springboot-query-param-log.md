---
title: Spring-Boot) 쿼리 파라미터 로그 남기기
author: cotchan 
date: 2021-01-05 20:38:21 +0800 
categories: [Spring-Boot, Spring-Boot_DB]
tags: [spring-boot] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. application.yml 수정

+ **`org.hibernate.type: trace` 추가**

```java
spring:
  datasource:
    url: jdbc:h2:tcp://localhost/~/jpashop
    username: sa
    password:
    driver-class-name: org.h2.Driver

  jpa:
    hibernate:
      ddl-auto: create
    properties:
      hibernate:
        show_sql: true
        format_sql: true

logging:
  level:
    org.hibernate.SQL: debug
    org.hibernate.type: trace
```

---

## 2. 외부 라이브러리 사용

+ https://github.com/gavlyukovskiy/spring-boot-data-source-decorator

```java
//build.gradle에 추가
implementation 'com.github.gavlyukovskiy:p6spy-spring-boot-starter:1.5.6'
```


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
