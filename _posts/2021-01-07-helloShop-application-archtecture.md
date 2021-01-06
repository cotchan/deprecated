---
title: HelloShop) 애플리케이션 아키텍쳐 
author: cotchan
date: 2021-01-07 04:31:21 +0800
categories: [Project, HelloShop]
tags: # [TAG]     # TAG names should always be lowercase
---

## 1. 애플리케이션 아키텍쳐

![Desktop View](/assets/img/post/helloShop/2021-01-07-application-archtecture.png)

+ **계층형 구조 사용**
    + **`controller`, `web`**
        + 웹 계층
    + **`service`**
        + 비즈니스 로직, 트랜잭션 처리
    + **`repository`**
        + JPA를 직접 사용하는 계층, 엔티티 매니저 사용
    + **`domain`**
        + 엔티티가 모여있는 계층, 모든 계층에서 사용

---

+ 패키지 구조
    + jpabook.japshop
        + domain
        + exception
        + repository
        + service
        + web

+ 개발 순서
    1. 서비스, 리포지토리 계층 개발
    2. 테스트 케이스를 작성해서 검증
    3. 마지막에 웹 계층 적용


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)


