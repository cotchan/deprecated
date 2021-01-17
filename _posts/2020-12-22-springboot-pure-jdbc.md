---
title: Spring-Boot) Spring을 사용하는 이유 (구현체 바꿔끼기)   
author: cotchan 
date: 2020-12-22 04:40:21 +0800 
categories: [Spring-Boot, Spring-Boot_DI]
tags: [spring-boot] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 환경 설정

+ build.gradle 파일에 `jdbc`, `h2` 데이터베이스 관련 라이브러리 추가

```
implementation 'org.springframework.boot:spring-boot-starter-jdbc'
runtimeOnly 'com.h2database:h2'
```

+ Java는 기본적으로 DB와 붙으려면 `jdbc driver`가 필수적으로 필요합니다.
+ runtimeOnly는 DB랑 붙을 때 DB가 제공하는 클라이언트 프로그램이 필요한데, 그게 h2 데이터베이스라는 의미입니다. 


---

## 2. 스프링 부트에 DB 연결 설정 추가

+ **DB에 붙을려면 접속 정보를 넣어줘야 합니다.**

`resources/application.properties`

```
spring.datasource.url=jdbc:h2:tcp://localhost/~/test 
spring.datasource.driver-class-name=org.h2.Driver 
spring.datasource.username=sa

```

+ **여기까지 하면 DB에 접속하기 위한 준비가 끝났습니다.**

---

## 3. DataSource란

+ 먼저 DataBase에 붙으려면 DataSource가 필요합니다.
+ DataSource는 주입을 받아야 하는데, 스프링으로부터 주입받습니다.
+ application.xxx에 셋팅을 하면 Springboot가 `DataSource`를 접속정보로 만들어놓고 우리가 나중에 주입받을 때 여기서 `DataSource.getConnection`을 하면 실제 DataBase에 대한 커넥션을 얻을 수 있습니다.
+ 이제 여기다가 SQL문을 날려서 DB에 전달해주는 방법입니다.

```java
public class JdbcMemberRepository implements MemberRepository {
    private final DataSource dataSource;

    public JdbcMemberRepository(DataSource dataSource) {
        this.dataSource = dataSource;
    }
}
```

---

## 4-1. 구현체 새로 만들기 

+ `JdbcMemberRepository` 작성하기

```java
//JdbcMemberRepository.java
public class JdbcMemberRepository implements MemberRepository {
	//...
}
```

---

## 4-2. Configuration 설정을 통해 구현체 연결

+ **`DataSource`는 SpringBoot가 알아서 주입받습니다.**

```java
//SpringConfig.java

@Configuration
public class SpringConfig {

    private DataSource dataSource;

    @Autowired
    public SpringConfig(DataSource dataSource) {
        this.dataSource = dataSource;
    }

    @Bean
    public MemberRepository memberRepository() {
//        return new MemoryMemberRepository();
        return new JdbcMemberRepository(dataSource);
    }
}
```

+ 스프링부트가 `application.xxx` 설정 파일에 적은 걸 보고 `DataSource를 생성`해줍니다.
+ 스프링이 자체적으로 Bean을 생성해서 DB와 연결할 수 있는 연결 정보를(DataSource) 만들어줍니다.
+ 그리고 얘를 필요한 시점에 주입을 해줍니다.


---


## 5. 결론1

**현재 기능 확장을 위해 다른 코드는 일절 안 건드리고 딱 두가지만 했습니다.** 

1. 구현체 새로 만들기 (인터페이스 확장)
2. @Configuration 파일 변경


---

## 6. 결론2: 스프링을 왜 쓰는가? 스프링의 장점

+ 객체 지향적인 설계가 좋습니다.
+ **왜 좋은가 하면, `다형성을 활용`하기가 좋습니다.**
+ **즉, `인터페이스를 앞에 두고 구현체 바꿔끼기`가 굉장히 쉽습니다.
    + 스프링의 DI를 사용하면 `기존 코드는 전혀 손대지 않고, 설정만으로 구현클래스를 변경`할 수 있습니다.
+ 스프링은 이걸 굉장히 쉽게 할 수 있도록 스프링 컨테이너가 지원해줍니다. 
+ 기존의 코드는 하나도 건드리지 않고 오직 `Application을 설정하는 코드`(어셈블리 코드)만 손대면 나머지 실제 애플리케이션 동작과 관련된 코드는 일절 건드릴 필요가 없습니다. 이걸 굉장히 편리하게 해주는 것이 스프링의 장점입니다.


![Desktop View](/assets/img/post/spring-boot/2020-12-22-springboot-replace-implement.png)


---





---

+ 출처
    + [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
