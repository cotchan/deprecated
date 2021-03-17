---
title: Spring-Security) 승인된 리다이렉션 URI
author: cotchan 
date: 2021-03-18 00:18:21 +0800 
categories: [Spring, Spring_Security]
tags: [spring-security] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 승인된 리다이렉션 URI란

+ **`서비스에서 파라미터로 인증 정보를 주었을 때` 인증에 성공하면 구글(네이버, 카카오)에서 리다이렉트할 URL을 의미합니다.**

---

## 2. 스프링 부트2 기본 URL

+ 스프링 부트2 버전의 시큐리티에서는 기본적으로 아래 주소로 리다이렉트 URL을 지원하고 있습니다.

+ **`{도메인}/login/oauth2/code/{소셜서비스코드}`**

+ example
  + `http://localhost:8080/login/oauth2/code/google`

---

## 3. 리다이렉트 Controller

+ **사용자가 별도로 리다이렉트 URL을 지원하는 Controller를 만들 필요가 없습니다.**
+ 시큐리티에서 이미 구현해놓은 상태입니다.


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
