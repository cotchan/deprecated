---
title: sb2) 2. 패키지와 main 클래스 생성
author: cotchan 
date: 2021-03-02 00:20:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 프로젝트 패키지 생성하기

+ **일반적으로 패키지명은 `웹 사이트 주소의 역순`으로 합니다.**
  + 예시
    + URL: admin.jojoldu.com 
    + Package: com.jojoldu.admin

+ 패키지명
  + com.cotchan.review.springboot
  + 저자를 따라서 `Group Id` + `프로젝트명`으로 지었습니다. 

---

## 2. main class 생성

+ **`@SpringBootApplication`**

+ 위에서 만든 패키지 아래에 Java 클래스를 생성합니다.

```java
//Application.java
package com.cotchan.review.springboot;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

+ Application 클래스가 앞으로 만들 프로젝트의 `메인 클래스`입니다.

---

## 3. @SpringBootApplication 

+ `@SpringBootApplication`으로 인해 
  + 스프링 부트의 자동 설정, 스프링 Bean 읽기와 생성을 모두 자동으로 설정됩니다.

---

## 4. SpringApplication.run

+ **main 메소드에서 실행하는 SpringApplication.run으로 인해 `내장 WAS를 실행`합니다.**

+ 내장 WAS란 별도로 외부에 WAS를 두지 않고 애플리케이션을 실행할 때 내부에서 WAS를 실행하는 것을 의미합니다.
  + 이렇게 되면 항상 서버에 톰캣을 설치할 필요가 없게 되고, 스프링 부트로 만들어진 Jar 파일로 실행하면 됩니다.

+ **스프링 부트에서는 내장 WAS 사용을 권장하고 있습니다.**
  + **그 이유는 언제 어디서나 같은 환경에서 스프링 부트를 배포할 수 있기 때문입니다.**


---






---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
