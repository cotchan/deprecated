---
title: Spring-Boot) 프로젝트 생성(init)
author: cotchan 
date: 2021-01-01 08:26:21 +0800 
categories: [Spring-Boot, Spring-Boot_Setting]
tags: [spring-boot] 
---

## 1. 스프링 부트 스타터

+ [start.spring.io](https://start.spring.io/)

---

## 2. 사용 기능

+ 아래의 기능들을 `Dependency`에 추가 

---

## 2-1. web
## 2-2. jpa
## 2-3. h2
## 2-4. lombok
## 2-5. thymeleaf
## 2-6. validation



---

## 3. build.gradle 설정

```yml
plugins {
    id 'org.springframework.boot' version '2.4.1'
    id 'io.spring.dependency-management' version '1.0.10.RELEASE' 
    id 'java'
}

group = 'jpabook'
version = '0.0.1-SNAPSHOT' 
sourceCompatibility = '11'
    
configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'   
    implementation 'org.springframework.boot:spring-boot-starter-validation'
    implementation 'org.springframework.boot:spring-boot-starter-thymeleaf'
    implementation 'org.springframework.boot:spring-boot-starter-web'

    compileOnly 'org.projectlombok:lombok'
    runtimeOnly 'com.h2database:h2'

    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    //JUnit4 추가
    testImplementation("org.junit.vintage:junit-vintage-engine") {
        exclude group: "org.hamcrest", module: "hamcrest-core"
    }
}

test {
    useJUnitPlatform()
}
```

---

## 4. JUnit4 사용 방법

+ **`build.gradle`에 아래 부분을 꼭 직접 추가해줘야 합니다.**
+ 해당 부분을 입력하지 않으면 JUnit5로 동작합니다.

```yml
//JUnit4 추가 
testImplementation("org.junit.vintage:junit-vintage-engine") {
exclude group: "org.hamcrest", module: "hamcrest-core" 
}
```


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
