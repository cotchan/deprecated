---
title: sb2) 6. 프로젝트에 Spring Data JPA 적용하기 (+h2)
author: cotchan 
date: 2021-03-03 19:54:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. build gradle에 의존성 추가

```
dependencies {
    //...    
    compile('org.springframework.boot:spring-boot-starter-data-jpa')
    compile('com.h2database:h2')
}
```

---

## 2. spring-boot-starter-data-jpa

+ 스프링 부트용 Spring Data JPA 추상화 라이브러리입니다.
+ 스프링 부트 버전에 맞춰 자동으로 JPA관련 라이브러리들의 버전을 관리해줍니다.

---

## 3. h2

+ 인메모리 관계형 데이터베이스
+ 별도의 설치가 필요 없이 프로젝트 의존성만으로 관리할 수 있습니다.
+ **메모리에서 실행되기 때문에 애플리케이션을 재시작할 때 마다 초기화된다는 점을 이용하여 테스트 용도로 많이 사용됩니다.**

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
