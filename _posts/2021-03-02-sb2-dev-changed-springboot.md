---
title: sb2) 1. Gradle 프로젝트 => SpringBoot 프로젝트로 변경
author: cotchan 
date: 2021-03-02 00:00:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 초기 build.gradle

+ `build.gradle`의 초기상태는 아래와 같습니다.

```java
//삭제 예정
plugins {
    id 'java'
}

group 'com.cotchan.review'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
}

dependencies {
    //삭제 예정
    testCompile group: 'junit', name: 'junit', version: '4.12'
}
```

---

## 2. 수정-1: buildscript

+ 먼저 build.gradle 맨 위에 위치할 코드입니다.
+ 아래 코드를 새로 추가해줍니다.

```java
buildscript {
    ext {
        springBootVersion = '2.1.7.RELEASE'
    }
    repositories {
        mavenCentral()
        jcenter()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }
}
```

+ 위 코드의 의미는 **프로젝트의 플러그인 의존성 관리를 위한 설정**입니다.
+ `ext`는 build.gradle에서 사용하는 전역변수를 설정하겠다는 의미입니다.
  + 여기서는 springBootVersion 전역변수를 생성하고 버전 값을 넣어줬습니다.
  + **해석: spring-boot-gradle-plugin 이라는 스프링 부트 그레이들 플러그인의 2.1.7 RELEASE를 의존성으로 받겠다**

---

## 3. 수정-2: apply plugin

+ 아래 코드를 새로 추가해줍니다.
+ 다음은 앞서 선언한 플러그인 의존성들을 어떻게 적용할 것인지 결정합니다.

```java
apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'
```

+ `io.spring.dependency-management` 플러그인은 스프링 부트의 의존성을 관리해주는 플러그인
  + 그러므로 꼭 설치해야 합니다.
+ **위의 4개의 플러그인은 자바와 스프링 부트를 사용하기 위해서는 필수 플러그인이니 항상 추가합니다.**


---

## 4. 수정-3: repositories

+ 기존의 repositories {} 부분을 수정해줍니다.

```java
repositories {
    mavenCentral()
    jcenter()
}
```

+ **`repositories`는 각종 의존성(라이브러리)들을 어떤 원격 저장소에서 받을지를 정합니다.**
  + 기본적으로 mavenCentral을 많이 사용하지만, 최근에는 jcenter도 많이 사용합니다.

---

## 5. 수정-4: dependencies

+ 기존의 dependencies {} 부분을 수정해줍니다.

```java
dependencies {
    compile('org.springframework.boot:spring-boot-starter-web')
    testCompile('org.springframework.boot:spring-boot-starter-test')
    testCompile group: 'junit', name: 'junit', version: '4.12'
}
```


+ **`dependencies`는 프로젝트 개발에 필요한 의존성들을 선언하는 곳입니다.**

+ 여기서는 아래 두 가지를 추가합니다.
  + `org.springframework.boot:spring-boot-starter-web`
  + `org.springframework.boot:spring-boot-starter-test`

+ **단, 특정 버전을 명시하면 안됩니다.**
  + 버전을 명시하지 않아야만 맨 위에 작성한 `${springBootVersion}` 버전을 따라가게 됩니다.
  + 이렇게 관리할 경우 각 라이브러리들의 버전 관리가 한 곳에 집중됩니다.

---

## 6. 수정-5: sourceCompatibility

+ 적용할 자바 버전을 적어줍니다.
  + 여기서는 JAVA8을 사용하겠습니다.

```java
sourceCompatibility = 1.8
```

---

## 7. 최종 build.gradle

+ 수정을 완료한 최종 `build.gradle` 형태입니다.

```java
buildscript {
    ext {
        springBootVersion = '2.1.7.RELEASE'
    }
    repositories {
        mavenCentral()
        jcenter()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
    }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'

group 'com.cotchan.review'
version '1.0-SNAPSHOT'
sourceCompatibility = 1.8

repositories {
    mavenCentral()
}

dependencies {
    compile('org.springframework.boot:spring-boot-starter-web')
    testCompile('org.springframework.boot:spring-boot-starter-test')
}
```

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
