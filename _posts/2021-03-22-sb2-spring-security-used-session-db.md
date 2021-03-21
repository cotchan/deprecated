---
title: sb2) 24. [스프링 시큐리티] 구글 로그인 연동 - 5(세션 저장소로 데이터베이스 사용하기) 
author: cotchan 
date: 2021-03-22 04:40:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 내장 톰캣 메모리에 저장되는 세션

+ 현재 서비스는 **애플리케이션을 재실행하면 로그인이 풀립니다.**
+ 그 이유는 세션이 내장 톰캣의 메모리에 저장되기 때문입니다.
+ **기본적으로 세션은 실행되는 WAS의 메모리에 저장되고 호출됩니다.**
+ 메모리에 저장되다 보니 내장 톰캣처럼 애플리케이션 실행 시 실행되는 구조에선 항상 초기화가 됩니다.
  + 즉, 배포할 때마다 톰캣이 재시작됩니다. 

---

## 2. spring-session-jdbc 등록

---

## 2-1. build.gradle

+ `spring-session-jdbc`는 현재 상태에서는 바로 사용할 수 없습니다.
+ spring web, spring jpa와 마찬가지로 의존성이 추가되어야 사용할 수 있습니다.

```
//build.gradle

dependencies {
    compile('org.springframework.session:spring-session-jdbc')
}
```

---

## 2-2. application.properties

+ **그리고 `application.properties`에 세션 저장소를 jdbc로 선택하도록 코드를 추가합니다.**

```
//application.properties

spring.session.store-type=jdbc
```

---

## 3. 결과 확인

+ 다시 애플리케이션을 실행해서 로그인을 테스트한 뒤 `h2-console`에 접속합니다.

+ h2-console을 보면 세션을 위한 테이블이 2개 생성된 것을 볼 수 있습니다.
  + `SPRING_SESSION`
  + `SPRING_SESSION_ATTRIBUTES`
+ **`JPA`로 인해 세션 테이블이 자동 생성되었기 때문에 별도로 해야 할 일은 없습니다.**

![Desktop View](/assets/img/post/spring-boot2/2021-03-22-session-db-1.png)

---

+ 세션 저장소를 데이터베이스로 교체했습니다.
+ 물론 지금도 스프링을 재시작하면 세션이 풆립니다. 
  + **그 이유는 H2 기반으로 스프링이 재실행될 때 H2도 재시작되기 때문입니다.**
+ 이후에 AWS로 배포하게 되면 AWS의 DB서비스인 RDS를 사용하게 되니 이 때부터는 세션이 풀리지 않습니다.

![Desktop View](/assets/img/post/spring-boot2/2021-03-22-session-db-2.png)

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
