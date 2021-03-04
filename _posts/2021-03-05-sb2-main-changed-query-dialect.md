---
title: sb2) 출력되는 쿼리 로그를 변경하는 방법 (h2,MySql 등)
author: cotchan 
date: 2021-03-05 01:40:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Main]
tags: [] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. spring.jpa.properties.hibernate.dialect

+ **사용하는 용도는 h2 환경에서 실행하면서 다른 벤더(MySql 등)의 문법을 확인하여 쿼리를 생성하는데 참조합니다.**
+ `spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect`
+ 위 옵션값을 `application.properties` 또는 `application.yml`에 설정합니다.

```
//application.properties

spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
```


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
