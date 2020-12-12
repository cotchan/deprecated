---
title: Spring-Boot) 일반적인 웹 애플리케이션의 계층 구조 
author: cotchan 
date: 2020-12-10 20:20:21 +0800 
categories: [Spring-Boot, Spring-Boot_Basic]
tags: [spring-boot] 
---

![Desktop View](/assets/img/post/spring-boot/2020-12-12-springboot-web-application.png)

---

## 도메인 객체 (DTO)

회원, 주문, 쿠폰 등 주로`데이터베이스에 저장되고 관리되는 데이터`를 의미합니다.

---

## 리포지토리 객체 (DAO)

비즈니스로직 중 데이터베이스에 접근하는 작업을 전담합니다.    
`도메인 객체를 DB에 저장하고 관리하는 역할`을 합니다.    

---

## 서비스 객체

`핵심 비즈니스 로직을 구현`하는 부분을 서비스라고 합니다.    

---

## 컨트롤러 객체

Web MVC에서 컨트롤러를 의미합니다.    



---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
