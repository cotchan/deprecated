---
title: sb3) SpringBoot 로그백 설정 방법(Logback)
author: cotchan 
date: 2021-06-02 22:13:21 +0800 
categories: [Spring-Boot3]
tags: [spring-module] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 사용 방법

+ **`logback.xml` 파일을 `resources 디렉토리`에 만들어서 참조합니다.**
+ **`logback-spring.xml` 파일을 `resources 디렉토리`에 만들어서 참조합니다.**


---

## 2.

---

---

##

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
    <appender-ref ref="console"/>
  </root>
</configuration>
```

---

## 3. Appender

+ **log의 형태를 설정합니다. 로그 메시지가 출력될 대상을 결정하는 요소입니다.**
  + 콘솔에 출력할지, 파일로 출력할지 등의 설정을 합니다.

---

## 4. Encoder

+ **Appender에 포함되어 `사용자가 지정한 형식으로 표현될 로그메시지를 변환하는 역할`을 담당합니다.**
+ Encoder는 바이트를 소유하고 있는 Appender가 관리하는 OutputStream에 쓸 시간과 내용을 제어할 수 있습니다.

---

## 4-1. Pattern





---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 

