---
title: Spring-Boot) 정적 컨텐츠 내려주는 방법 
author: cotchan 
date: 2020-12-10 23:27:21 +0800 
categories: [Spring-Boot, Spring-Boot_Basic]
tags: [spring-boot] 
---

## 스프링 부트에서 정적 컨텐츠 내려주는 방법

+ [Spring-Boot Static Content](https://docs.spring.io/spring-boot/docs/2.3.1.RELEASE/reference/html/spring-boot-features.html#boot-features-spring-mvc-static-content)

`/resources/static` 이하 경로에 원하는 `파일`을 넣으면 됩니다.    
그러면 정적 파일이 그대로 반환이 됩니다. (단, 여기에는 어떠한 프로그래밍 불가능)

---

## Example

1. resources/static/hello-static.html 파일 생성

2. 브라우저에 localhost:8080/hello-static.html을 치면 브라우저가 정적 컨텐츠를 반환하는 것을 확인할 수 있습니다.

---

## 동작 원리

![Desktop View](/assets/img/post/spring-boot/2020-12-10-web-static-content.png)

1. 웹브라우저에서 hello-static.html 정적 컨텐츠 요청
2. 내장 톰캣 서버가 요청을 받고 Spring에게 요청을 넘깁니다.
3. 스프링은 `Controller` 쪽에서 먼저 hello-static이라는 Controller가 있는지 찾아봅니다. (컨트롤러가 더 높은 우선순위를 가집니다.)
4. Spring이 해당하는 컨트롤러를 찾지 못하는 경우, `resources 경로` 아래에 있는 static/hello-static.html을 찾습니다.
5. 4번 과정을 통해 정적 파일을 찾은 경우, 이 .html 파일을 그대로 반환해줍니다.    



---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
