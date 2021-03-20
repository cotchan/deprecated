---
title: sb2) 19. [스프링 시큐리티] application-oauth 등록
author: cotchan 
date: 2021-03-19 00:30:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. application-oauth 등록

+ **`src/main/resources/`** 디렉토리에 `application-oauth.properties` 파일을 생성합니다.

+ 그리고 해당 파일에 Client ID와 Client Secret Code를 등록합니다.

```
//application-oauth.properties

spring.security.oauth2.client.registration.google.client-id=클라이언트 ID
spring.security.oauth2.client.registration.google.client-secret=클라이언트 Secret
spring.security.oauth2.client.registration.google.scope=profile,email
```

---

## 2. .gitignore에 등록

+ `.gitignore`에 아래 한 줄을 추가합니다.
+ **추가 한 뒤 커밋했을 때 커밋 목록에 파일이 나오지 않으면 성공입니다.**

```
//.gitignore

application-oauth.properties
```

---

## 3. application.properties 수정

+ `application.properties` 파일에 아래 한 줄을 추가해줍니다.

```
spring.profiles.include=oauth
```

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
