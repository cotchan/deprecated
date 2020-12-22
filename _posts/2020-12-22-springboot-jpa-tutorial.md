---
title: Spring-Boot) 
author: cotchan 
date: 2020-12-10 00:00:21 +0800 
categories: [Spring-Boot, Spring-Boot_Basic]
tags: [spring-boot] 
---

## JPA (Java Persistence API)

+ JPA는 기존의 반복 코드는 물론이고, 기본적인 SQL도 JPA가 직접 만들어서 실행해줍니다.
+ JPA를 사용하면, SQL과 데이터 중심의 설계에서 객체 중심의 설계로 패러다임을 전환할 수 있습니다.
+ JPA를 사용하면 개발 생산성을 크게 높일 수 있습니다. 


jpa 추가하는 방법

```terminal
//build.gradle
dependencies {
	implementation 'org.springframework.boot:spring-boot-starter-data-jpa'

}


![Desktop View](/assets/img/post/spring-boot/2020-12-10-spring-boot-how-to-build.png){: width="350" class="normal"}







---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
