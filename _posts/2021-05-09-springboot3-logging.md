---
title: sb3) 로그 남기는 방법 (feat. 예외 로그) 
author: cotchan 
date: 2021-05-09 17:15:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 로그를 남기는 목적?

+ **`시스템에 문제가 생겼을 때 그 문제의 원인을 파악하기 위해 로그를 남깁니다.`**
+ 그러므로 로그를 남길 때는 장애의 원인을 최대한 쉽게 파악할 수 있게끔 `가능한 많은 정보를 남겨야 합니다.`
  + 즉, 가능한 디버깅이 쉽도록 로그를 남겨야 합니다.

---

## 2. printStackTrace 남기기

+ 어떻게 해야 디버깅이 쉽도록 로그를 남길 수 있을까요?
+ **가장 중요한 건 `예외를 캐치하고 반드시 printStackTrace를 남겨야합니다.`**
  + **즉, 디버깅에 도움이 되는 정보와 함께 stack trace를 함께 남기는 것이 좋습니다.**

+ 로깅 프레임워크를 쓸텐데, **마지막에 예외 클래스를 전달하면 자동으로 예외의 stackTrace가 찍힙니다.**
+ **`그러므로 아래와 로깅 포맷을 사용하는 것을 권장합니다.`**

```java
log.error("Unexpected exception occurred: {}", e.getMessage(), e);

log.error("Unexpected error from userId {}: {}", userId, e.getMessage(), e);
```

---

## 3. 적절한 로깅 레벨의 필요성

+ **예외 종류에 따라 로깅 레벨을 적절하게 분리해서 유연하게 운영해야 합니다.**
+ 그 이유는 실제 업무에서는 대부분 warn이나 error로 로그를 찍게되면 실시간으로 개발자에게 노티가 가도록 설정합니다.
  + 그러므로 적절한 로깅 레벨을 설정해주지 않으면 불필요하게 실시간 노티가 개발자에게 전달될 수 있습니다.

---

## 3-1. DEBUG

+ **개발단계에서부터 활성화. `프로세스의 처리 순서/흐름을 분석하는데 도움이 되는 정보`**
+ **클라이언트쪽에서 발생한 오류를 서버에서 기록할 때** 사용합니다.
+ 보통 운영단계에서는 끕니다.

---

## 3-2. INFO

+ **디버깅 정보 외에 `프로세스 설정/기동에 관련한 정보들을 담습니다.`**
+ 중요한 정보나, 설정 정보(지금 서버가 어떤 설정 정보를 통해서 기동되었다, 뭘 사용하고 있다 등)

---

## 3-3. WARN

+ 문제가 발생했는데, 지금 당장은 안 봐도 되는 경우
+ 오류 상황은 아니지만, 추후 확인이 필요한 정보 등

---

## 3-4. ERROR

+ 문제가 발생했는데, 지금 당장 봐야하는 경우
+ 오류가 발생한 상황. 즉시 대응이 필요한 경우

---

## 3-5. FATAL

+ 매우 심각한 상황. 즉시 대응이 필요함

---

## 4. 올바른 예외 로깅방법 예시

+ 클라이언트의 문제로 발생한 `handleBadRequestException` 메서드 부분은 `debug 레벨 로그를 통해서` 해결해도 충분합니다.
+ **`ServiceRuntimeException(사용자 정의 비즈니스 예외)`에 해당하는 부분**
  + 비즈니스 예외를 던지도록 처리 해놨는데, 위 if 문에 걸리지 않았다는 건 핸들링 되지 않은 에러를 의미합니다.
  + 그러므로 `warn 레벨 로그를 통해서` 현상을 확인한 후 해당 비즈니스 예외를 처리하는 클래스를 추가하는 식으로 대응할 수 있습니다. 
+ **`Exception`에 해당하는 부분**
  + 여기에 걸린 애는 예측할 수 없는 에러입니다. 그러므로 `error 레벨 로그를 통해서` 현상을 확인해야 합니다.


```java
//GeneralExceptionHandler.java

package com.github.prgrms.social.controller;

@ControllerAdvice
public class GeneralExceptionHandler {

  private final Logger log = LoggerFactory.getLogger(getClass());

  private ResponseEntity<ApiResult<?>> newResponse(Throwable throwable, HttpStatus status) {
    HttpHeaders headers = new HttpHeaders();
    headers.add("Content-Type", "application/json");
    return new ResponseEntity<>(ERROR(throwable, status), headers, status);
  }

  @ExceptionHandler(NoHandlerFoundException.class)
  public ResponseEntity<?> handleNotFoundException(Exception e) {
    return newResponse(e, HttpStatus.NOT_FOUND);
  }

  @ExceptionHandler({
    IllegalStateException.class, IllegalArgumentException.class,
    TypeMismatchException.class, HttpMessageNotReadableException.class,
    MissingServletRequestParameterException.class, MultipartException.class,
  })
  public ResponseEntity<?> handleBadRequestException(Exception e) {
    log.debug("Bad request exception occurred: {}", e.getMessage(), e);
    return newResponse(e, HttpStatus.BAD_REQUEST);
  }

  @ExceptionHandler(HttpMediaTypeException.class)
  public ResponseEntity<?> handleHttpMediaTypeException(Exception e) {
    return newResponse(e, HttpStatus.UNSUPPORTED_MEDIA_TYPE);
  }

  @ExceptionHandler(HttpRequestMethodNotSupportedException.class)
  public ResponseEntity<?> handleMethodNotAllowedException(Exception e) {
    return newResponse(e, HttpStatus.METHOD_NOT_ALLOWED);
  }

  @ExceptionHandler(ServiceRuntimeException.class)
  public ResponseEntity<?> handleServiceRuntimeException(ServiceRuntimeException e) {
    if (e instanceof NotFoundException)
      return newResponse(e, HttpStatus.NOT_FOUND);
    if (e instanceof UnauthorizedException)
      return newResponse(e, HttpStatus.UNAUTHORIZED);

    //비즈니스 예외는 앞으로 계속 추가될 수 있습니다.

    log.warn("Unexpected service exception occurred: {}", e.getMessage(), e);
    return newResponse(e, HttpStatus.INTERNAL_SERVER_ERROR);
  }

  @ExceptionHandler({Exception.class, RuntimeException.class})
  public ResponseEntity<?> handleException(Exception e) {
    log.error("Unexpected exception occurred: {}", e.getMessage(), e);
    return newResponse(e, HttpStatus.INTERNAL_SERVER_ERROR);
  }

}
```

---

## 5. 로그 파일 관리 방법

+ 생각없이 로그를 파일에 남기기만 하면 어느 순간 DISK FULL 장애를 마주하게 됩니다.
+ 단일 로그 파일이 너무 큰 경우 필요한 정보를 찾는데 시간이 오래 걸립니다.
+ **따라서 단일 `로그 파일이 너무 커지지 않게하고`, `날짜별로 로그를 쉽게 찾을 수 있도록 로그 파일을 정리`해야 합니다.**
+ 일반적으로 사용하는 로깅 프레임워크는 이런 기능을 모두 제공합니다.
  + `특정 사이즈 단위로 자르고`
  + `최대로 로그 파일을 몇 개까지 가지고 있을지`
  + 위 사항들을 정책으로 정할 수 있습니다.

```xml
//logback.xml

<appender name="file" class="ch.qos.logback.core.rolling.RollingFileAppender">
  <file>log/debug.log</file>
  <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
    <fileNamePattern>log/%d{yyyy-MM}/debug.%d{yyyy-MM-dd}.%i.log.gz</fileNamePattern>
    <maxFileSize>1KB</maxFileSize>
    <maxHistory>15</maxHistory>
  </rollingPolicy>
  <encoder>
    <Pattern>${CONSOLE_LOG_PATTERN}</Pattern>
  </encoder>
</appender>
```

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
