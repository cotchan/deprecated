---
title: sb3) 로그백 설정 방법(Logback)
author: cotchan 
date: 2021-06-02 22:13:21 +0800 
categories: [Spring-Boot3]
tags: [spring-module] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 포스팅을 참고하여 작성한 글입니다.**
+ **계속 업데이트할 예정입니다.**

---

## 1. Logback 파일 위치

+ **`logback.xml` 파일을 `resources 디렉토리`에 만들어서 참조합니다.**
+ **`logback-spring.xml` 파일을 `resources 디렉토리`에 만들어서 참조합니다.**

---

## 2. 사용 방법

1. **`pom.xml`에 로거를 등록합니다.**
2. **`LoggerFactory`에서 로거 객체를 불러옵니다.**
3. **로거 객체를 이용해서 원하는 위치에 로그를 찍습니다.**

```xml
    <dependency>
      <groupId>org.lazyluke</groupId>
      <artifactId>log4jdbc-remix</artifactId>
      <version>0.2.7</version>
    </dependency>
```

---

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;


@Controller 
public class TestController { 
  
  private final Logger logger = LoggerFactory.getLogger(getClass()); 

  @RequestMapping(value = "/test") 
  public ModelAndView test() throws Exception { 

    logger.trace("Trace Level 테스트"); 
    logger.debug("DEBUG Level 테스트"); 
    logger.info("INFO Level 테스트"); 
    logger.warn("Warn Level 테스트"); 
    logger.error("ERROR Level 테스트"); 

    return mav; 
  } 
}
```

---

## 3. Logback Code

+ **`logback.xml`**

```xml
<?xml version="1.0" encoding="UTF-8"?>

<!-- 로그백 설정 부분-->
<configuration>

  <!-- property::로그백 설정파일에서 사용될 변수명을 선언합니다. -->
  <property name="CONSOLE_LOG_PATTERN" value="%clr(%d{yyyy-MM-dd HH:mm:ss.SSS,IST}){faint} %clr(${LOG_LEVEL_PATTERN:-%5p}) %clr(${PID:- }){magenta} %clr(---){faint} %clr([%15.15t]){faint} %clr(%-40.40logger{39}:%L){cyan} %clr(:){faint} %m%n${LOG_EXCEPTION_CONVERSION_WORD:-%wEx}"/>

  <!-- ch.qos.logback.core.ConsoleAppender => 콘솔에 로그를 찍습니다. 로그를 OutputStream에 작성하여 콘솔에 출력되도록 합니다. -->
  <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
    <!-- 출력패턴 설정 -->
    <encoder>
      <Pattern>${CONSOLE_LOG_PATTERN}</Pattern>
    </encoder>
  </appender>

  <!-- ch.qos.logback.core.rolling.RollingFileAppender => 여러개의 파일을 롤링, 순회하면서 로그를 찍습니다. -->
  <appender name="file" class="ch.qos.logback.core.rolling.RollingFileAppender">
    <!-- 기록할 파일명과 경로를 설정 -->
    <file>log/debug.log</file>
    <!-- Rolling 정책::ch.qos.logback.core.rolling.TimeBasedRollingPolicy => 일자별 적용 -->
    <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
      <!-- 파일 쓰기가 종료된 log 파일명의 패턴을 지정합니다. -->
      <!-- .gz,.zip 등을 넣으면 자동 일자별 로그파일 압축 -->
      <fileNamePattern>log/%d{yyyy-MM}/debug.%d{yyyy-MM-dd}.%i.log.gz</fileNamePattern>
      <!-- 한 파일당 최대 파일 용량을 지정합니다. -->
      <maxFileSize>1KB</maxFileSize>
      <!-- 일자별 로그파일 최대 보관주기(일), 해당 설정일 이상된 파일은 자동으로 제거-->
      <maxHistory>15</maxHistory>
    </rollingPolicy>

    <!-- 출력패턴 설정 -->
    <encoder>
      <Pattern>${CONSOLE_LOG_PATTERN}</Pattern>
    </encoder>
  </appender>

  <!-- logger => 특정패키지 로깅 레벨 설정, OFF => 출력하지 않음 -->
  <logger name="io.undertow" level="OFF"/>

  <!-- root 레벨 설정 -->
  <root level="INFO">
    <!-- appender-ref로 "appender name" 지정 -->
    <appender-ref ref="console"/>
  </root>
</configuration>
```

---

## 4. appender

+ **`log의 형태를 설정합니다.` 로그 메시지가 출력될 대상을 결정하는 요소입니다.**
  + `콘솔에 출력할지`, `파일로 출력할지` 등의 설정을 합니다.

---

## 4-1. appender-class

1. **`ch.qos.logback.core.ConsoleAppender` : 콘솔에 로그를 찍음, 로그를 OutputStream에 작성하여 콘솔에 출력되도록 합니다.**
2. `ch.qos.logback.core.FileAppender` : 파일에 로그를 찍음, 최대 보관 일 수 등를 지정할 수 있습니다.
3. **`ch.qos.logback.core.rolling.RollingFileAppender` : 여러개의 파일을 롤링, 순회하면서 로그를 찍습니다.**
  + (FileAppender를 상속 받는다. 지정 용량이 넘어간 Log File을 넘버링 하여 나누어 저장할 수 있다.)
4. `ch.qos.logback.classic.net.SMTPAppender` : 로그를 메일에 찍어 보냅니다.
5. `ch.qos.logback.classic.db.DBAppender` : DB(데이터베이스)에 로그를 찍습니다. 

---

## 5. encoder

+ **appender에 포함되어 `사용자가 지정한 형식으로 표현될 로그메시지를 변환하는 역할`을 담당합니다.**
+ Encoder는 바이트를 소유하고 있는 Appender가 관리하는 OutputStream에 쓸 시간과 내용을 제어할 수 있습니다.

---

## 5-1. Pattern

+ **패턴에 사용되는 요소**
  + **`%Logger{length}`** : Logger name을 축약할 수 있다. {length}는 최대 자리 수, ex)logger{35}
  + **`%-5level`** : 로그 레벨, -5는 출력의 고정폭 값(5글자)
  + **`%msg`** : - 로그 메시지 (=%message)
  + **`${PID:-}`** : 프로세스 아이디
  + **`%d`** : 로그 기록시간
  + **`%p`** : 로깅 레벨
  + **`%F`** : 로깅이 발생한 프로그램 파일명
  + **`%M`** : 로깅일 발생한 메소드의 명
  + **`%l`** : 로깅이 발생한 호출지의 정보
  + **`%L`** : 로깅이 발생한 호출지의 라인 수
  + **`%thread`** : 현재 Thread 명
  + **`%t`** : 로깅이 발생한 Thread 명
  + **`%c`** : 로깅이 발생한 카테고리
  + **`%C`** : 로깅이 발생한 클래스 명
  + **`%m`** : 로그 메시지
  + **`%n`** : 줄바꿈(new line)
  + **`%%`** : %를 출력
  + **`%r`** : 애플리케이션 시작 이후부터 로깅이 발생한 시점까지의 시간(ms)

---

## 6. root, logger

+ **`설정한 appender를 참조`하여 package와 level을 설정합니다.** 

---

## 6-1. root

+ **`전역 설정`이라고 볼 수 있습니다.**
+ 지역적으로 선언된 logger 설정이 있다면 해당 logger 설정이 기본으로 적용됩니다.

---

## 6-2. logger

+ **실제 로그 기능을 수행하는 객체로 각 Logger마다 이름을 부여하여 사용합니다.**
  + `지역 설정`이라고 볼 수 있습니다.
+ 각 logger마다 원하는 출력 레벨값을 설정할 수 있으며, 0개 이상의 Appender를 지정할 수 있습니다.
+ **각 소스로부터 입력받은 로깅 메시지는 `로그 레벨에 따라 Appender로 전달됩니다.`**
+ 기본적으로 최상위 로거인 `Root Logger`를 설정해 주어야하며, 추가로 필요한 로거에 대해 String 또는 클래스명 형식으로 `Logger name`을 추가하여 사용할 수 있습니다. 

---

+ 출처
  + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
  + [[스프링부트 (5)] Spring Boot 로그 설정(1) - Logback](https://goddaehee.tistory.com/206)
  + [logback.xml Example](https://mkyong.com/logging/logback-xml-example/)
  + [log4j2.xml example](https://mkyong.com/logging/log4j2-xml-example/)
  + [Spring 에서 Logback 사용하기](https://gs.saro.me/dev?tn=479)
  + [Logback - 3. Logback의 설정(2).configuration 파일 구성](https://ckddn9496.tistory.com/79)
  + [[JAVA] Logback 사용법](https://hochoon-dev.tistory.com/entry/JAVA-Logback-%EC%82%AC%EC%9A%A9%EB%B2%95)

