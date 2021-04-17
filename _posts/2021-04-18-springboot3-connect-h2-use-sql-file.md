---
title: sb3) h2 DB 연동 및 .sql 파일 실행 방법
author: cotchan 
date: 2021-04-18 00:00:21 +0800 
categories: []
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. Maven 의존성 라이브러리

```xml
<dependencies>
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
</dependencies>
```

---


## 2. H2 데이터베이스 설정

+ **H2 는 따로 설정하지 않으면 위와 같은 값으로 기본 설정을 합니다.**
  + 즉, application.properity를 비워놔도 아래와 같이 셋팅이 된다는 뜻입니다.

+ datasource.url의 jdbc:h2:mem:testdb는 testdb 스키마에 `mem` 인 메모리 데이터베이스로 동작하라는 설정

```
spring.datasource.url=jdbc:h2:mem:testdb  
spring.datasource.driverClassName=org.h2.Driver  
spring.datasource.username=sa  
spring.datasource.password=  
spring.h2.console.enabled=false  
```

+ application.properties을 수정하여 `H2 콘솔을 사용`하게 할 수 있습니다.
  + `http://localhost:8080/h2-console` 을 통해 로그인

```
spring.h2.console.enabled=true
```

---

## 3. 스프링부트 실행 될 때 SQL문 실행시키기

---

## 3-1. SQL 파일 생성

```sql
//schema.sql

CREATE TABLE users
(
    seq           bigint      NOT NULL AUTO_INCREMENT,
    email         varchar(50) NOT NULL,
    passwd        varchar(80) NOT NULL,
    login_count   int         NOT NULL DEFAULT 0,
    last_login_at datetime             DEFAULT NULL,
    create_at     datetime    NOT NULL DEFAULT CURRENT_TIMESTAMP(),
    PRIMARY KEY (seq),
    CONSTRAINT unq_user_email UNIQUE (email)
);
```

---

+ **Spring 기본값으로 classpath 루트에 `schema.sql`, `data.sql` 파일이 있다면 서버 시작 시 스크립트를 실행합니다. **
+ 보통 schema.sql은 DDL 스크립트를 명시해두고, 데이터를 위한 DML 문은 data.sql 파일로 작성합니다. 
+ 만약 특정 환경에 맞는 sql 문을 실행하고 싶다면 아래와 같이 platform이라는 property를 설정하면 됩니다.

```
spring.datasource.platform=test
```

---

## 3-2. 스프링 재실행 후 접속

+ 스프링을 재실행 후 접속하여 H2 콘솔에 들어가면 테이블이 생긴 것을 확인할 수 있습니다.


---

## 4. Error creating bean with name 'dataSource'

+ **스프링부트 프로젝트를 만들고 springboot-starter-jpa 를 추가하고 바로 실행하면 바로 예외가 발생합니다.**

+ datasouce bean 정의를 찾을 수 없다는 예외인데 이유는 당연하게도 jpa를 사용하기 위한 datasource 정보가 없어서 입니다.

+ **속성파일 `application.properties 에 아래 속성을 정의`하시면 됩니다.**

```
spring.datasource.url
spring.datasource.username
spring.datasource.password
spring.datasource.driver-class-name
```

---

+ 출처
  + [스프링부트 H2 데이터베이스](https://developerhive.tistory.com/34)
  + [Spring 데이터베이스 Schema 및 Data 초기설정하기](https://wan-blog.tistory.com/52)
  + [Error creating bean with name 'dataSource'](https://thecodinglog.github.io/spring/springboot/2020/04/17/bean-creation-exception.html)
