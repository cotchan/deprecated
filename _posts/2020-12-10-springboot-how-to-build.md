---
title: Spring-Boot) 빌드 & 실행방법 
author: cotchan 
date: 2020-12-10 00:00:21 +0800 
categories: [Spring-Boot, Spring-Boot_Basic]
tags: [spring-boot] 
---

**빌드를 해서 실제 실행할 수 있는 파일을 만드는 방법**  
(macOS를 기준으로 작성하였습니다)   

## 빌드 방법

## 1. 프로젝트 폴더로 이동

![Desktop View](/assets/img/post/spring-boot/2020-12-10-spring-boot-how-to-build.png){: width="350" class="normal"}


---

## 2. gradlew를 통해 build 

+ windows는 `gradlew.bat`를 통해 빌드합니다.
+ masOS는 `gradlew`를 통해 빌드합니다.

아래 명령어를 통해 빌드하면 됩니다.   

```terminal
$ ./gradlew build
```

**빌드가 안된다면 build clean을 해줍니다.**
(build 폴더 자체를 없애는 명령어)    
이전 빌드 내용을 완전히 지우고 다시 빌드해줍니다.   

```terminal
$ ./gradlew clean build
```

---

## 3. build/libs 폴더로 이동

2번 과정을 통해 빌드하면 `build` 폴더가 생깁니다.    

```terminal
$ cd build/libs
```

위 경로로 이동하면 아래와 같은 .jar 파일이 만들어져 있습니다.    

`hello-spring-0.0.1-SNAPSHOT.jar`

---

## 4. .jar 파일 실행

```terminal
$ java -jar hello-spring-0.0.1-SNAPSHOT.jar
```

위 명령어를 실행하면 서버가 뜨는 것을 확인할 수 있습니다.

---

## Summary: 서버 배포 방법

서버에 배포할 때는 .jar 파일만 복사해서 서버에 넣어주고 `java -jar ***.jar` 명령어를 통해 실행하면 됩니다. 그러면 서버에서도 Spring이 동작하게 됩니다.


---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
