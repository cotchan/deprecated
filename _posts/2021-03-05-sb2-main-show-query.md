---
title: sb2) 실제로 실행되는 쿼리 보는 방법 (JPA 사용 시)
author: cotchan 
date: 2021-03-05 01:31:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Main]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. spring.jpa.show_sql=true

+ 위 옵션값을 application.properties 또는 application.yml에 설정합니다.

```
//application.properties

spring.jpa.show_sql=true
```


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
