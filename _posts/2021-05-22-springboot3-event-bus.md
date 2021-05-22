---
title: sb3) EventBus 사용법
author: cotchan 
date: 2021-05-22 15:00:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. EventBus

+ **`하나의 서비스 내에서 이벤트를 전파할 때 EventBus를 사용합니다.`**
  + **스프링 안에 있는 여러 컴포넌트 간에 이벤트를 전달할 때 `이벤트 버스`가 필요합니다.**
+ 이 포스팅에서는 `Google의 guava`를 통해 이벤트 버스를 구현하는 것을 다룹니다. (Maven 사용)

---

## 2. import guava

```xml
    <properties>
        <guava.version>30.1.1-jre</guava.version>
    </properties>

        <dependency>
            <groupId>com.google.guava</groupId>
            <artifactId>guava</artifactId>
            <version>${guava.version}</version>
        </dependency>
```

---

## 3. EventConfiguration 작성

+ guava를 추가하면 이벤트 처리를 위한 설정 클래스를 작성합니다.
  + `EventConfigure.java`

---

## 3-1. application.yml

+ `application.yml`

```yml
eventbus:
  asyncPoolCore: 1
  asyncPoolMax: 4
  asyncPoolQueue: 100
```

---

## 3-2. @Configuration

+ `EventConfigure.java` FullCode

```java
package com.github.prgrms.social.configure;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.github.prgrms.social.event.EventExceptionHandler;
import com.github.prgrms.social.event.listener.CommentCreateEventListener;
import com.github.prgrms.social.event.listener.JoinEventListener;
import com.google.common.eventbus.AsyncEventBus;
import com.google.common.eventbus.EventBus;
import org.springframework.boot.context.properties.ConfigurationProperties;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.core.task.TaskExecutor;
import org.springframework.kafka.core.KafkaTemplate;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;

@Configuration
@ConfigurationProperties(prefix = "eventbus")
public class EventConfigure {

  private int asyncPoolCore;
  private int asyncPoolMax;
  private int asyncPoolQueue;

  public int getAsyncPoolCore() {
    return asyncPoolCore;
  }

  public void setAsyncPoolCore(int asyncPoolCore) {
    this.asyncPoolCore = asyncPoolCore;
  }

  public int getAsyncPoolMax() {
    return asyncPoolMax;
  }

  public void setAsyncPoolMax(int asyncPoolMax) {
    this.asyncPoolMax = asyncPoolMax;
  }

  public int getAsyncPoolQueue() {
    return asyncPoolQueue;
  }

  public void setAsyncPoolQueue(int asyncPoolQueue) {
    this.asyncPoolQueue = asyncPoolQueue;
  }

  @Bean
  public TaskExecutor eventTaskExecutor() {
    ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
    executor.setThreadNamePrefix("EventBus-");
    executor.setCorePoolSize(asyncPoolCore);
    executor.setMaxPoolSize(asyncPoolMax);
    executor.setQueueCapacity(asyncPoolQueue);
    executor.afterPropertiesSet();
    return executor;
  }

  @Bean
  public EventExceptionHandler eventExceptionHandler() {
    return new EventExceptionHandler();
  }

  @Bean
  public EventBus eventBus(TaskExecutor eventTaskExecutor, EventExceptionHandler eventExceptionHandler) {
    return new AsyncEventBus(eventTaskExecutor, eventExceptionHandler);
  }

  @Bean(destroyMethod = "close")
  public JoinEventListener joinEventListener(EventBus eventBus, KafkaTemplate kafkaTemplate, ObjectMapper objectMapper) {
    return new JoinEventListener(eventBus, kafkaTemplate, objectMapper);
  }

  @Bean(destroyMethod = "close")
  public CommentCreateEventListener commentCreateEventListener(EventBus eventBus, KafkaTemplate kafkaTemplate, ObjectMapper objectMapper) {
    return new CommentCreateEventListener(eventBus, kafkaTemplate, objectMapper);
  }
}
``` 

---

## 3.3 EventBus Bean 등록

+ **여기서 중요한 건 `EventBus를 빈으로 등록한다는 점`입니다.**
+ 여기서 `AsyncEventBus`로 등록하면 비동기로 처리가 되기 때문에 Blocking이 되지 않습니다.
  + 하나의 이벤트를 여러 컴포넌트가 구독하는데, 이걸 순차적으로 처리하는 게 아니라 비동기로 처리할 수 있게 합니다.
+ 이 때 별도로 `eventTaskExecutor`를 전달할 수 있습니다.
+ 또한 `eventExceptionHandler`를 전달할 수 있습니다.

+ **이벤트 버스를 빈으로 등록시켜놓으면 `우리 코드(서비스)에서 이벤트를 전파할 수 있습니다.`**

```java
@Configuration
@ConfigurationProperties(prefix = "eventbus")
public class EventConfigure {
  
  //...

  @Bean
  public EventBus eventBus(TaskExecutor eventTaskExecutor, EventExceptionHandler eventExceptionHandler) {
    return new AsyncEventBus(eventTaskExecutor, eventExceptionHandler);
  }
}
```

---

## 3-4. EventListener Bean 등록

+ 이벤트를 전파하기 전에 필요한 EventListener를 만들고 빈으로 등록해줍니다.
  + **EventListener에서 제일 중요한 건 `특정 이벤트를 듣는 리스너`를 만들 수 있습니다.**
  + 이 내용은 EventBus 자체와는 무관하므로 아래 출처에서 다룹니다.
    + [EventListener 빈 등록 및 사용방법](https://cotchan.github.io/posts/springboot3-event-listener-push-manager/)

---

## 4. 이벤트 전파 방법

+ 3번에서의 설정을 모두 마치면 이제는 이벤트를 전파할 수 있습니다.
+ 전제: `JoinEvent` 사용
  + **JoinEvent란?** 
    + 사용자가 회원 가입 할 시 "유저가 가입했다"라는 걸 알리는 이벤트

---

## 4-1. 서비스 코드에서 EventBus에다가 JoinEvent 전파 

+ **`서비스 코드(UserService)에서` 서비스 로직을 마친 다음에 `이벤트 버스에다가 JoinEvent를 발생시킵니다.`**
  + 추가로 UserService를 만들 때 EventBus를 같이 주입받습니다.

```java
//UserService.java

@Service
public class UserService {

  //...

  private final EventBus eventBus;

  @Transactional
  public User join(String name, Email email, String password) {
    checkArgument(isNotEmpty(password), "password must be provided.");
    checkArgument(
      password.length() >= 4 && password.length() <= 15,
      "password length must be between 4 and 15 characters."
    );

    User user = new User(name, email, passwordEncoder.encode(password));
    User saved = insert(user);

    // raise event
    eventBus.post(new JoinEvent(saved));
    return saved;
  }
}
```

---

## 4-2. JoinEventListener에서 후처리

+ UserService에서 eventBus에다가 JoinEvent를 발생시키면
  + JoinEventListener에서 @Subscribe 메서드를 통해 후처리를 할 수 있습니다.

+ [JoinEventListener에서 후처리하는 방법](https://cotchan.github.io/posts/springboot3-event-listener-push-manager/)

```java
public class JoinEventListener implements AutoCloseable { 
 
  //...

  @Subscribe
  public void handleJoinEvent(JoinEvent event) {
    //...
  }
}
```

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 

