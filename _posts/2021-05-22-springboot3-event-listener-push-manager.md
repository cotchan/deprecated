---
title: sb3) EventListsner 사용법(feat.JoinEvent, JoinEventListener)
author: cotchan 
date: 2021-05-22 15:30:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 0.intro

+ **`여기서 말하는 EventListener란, 이벤트가 발생하면 처리하는 애를 말합니다.`**
+ Google guava를 사용하여 EventListener를 등록하고 사용하는 방법에 대해 작성합니다.
+ 여기서 사용하는 Event와 EventListener는 
  + **사용자가 회원 가입 시 발생하는 이벤트인 JoinEvent**
  + **JoinEvent 발생을 기다렸다가(`구독`) 그에 대한 처리를 하는 JoinEventListener입니다.**

---

## 1. 선행작업

+ JoinEventListener를 빈으로 등록하고 사용하기에 앞서 필요한 작업들입니다.
  + import guava
  + EventConfiguration.java 작성
  + EventConfiguration.java에 eventBus 빈으로 등록

---

## 2. JoinEventListener 생성

+ **빈으로 만들어지는 시점에 eventBus에다가 대고 자기 자신을 `regist` 합니다.**
+ `모든 EventListener 빈들이 다 eventBus에 의해서 호출`된다고 보면 됩니다.
+ eventBus에 등록된 bean들이 호출됩니다.

```java
package com.github.prgrms.social.event.listener;

public class JoinEventListener implements AutoCloseable {

  private final EventBus eventBus;
  private final KafkaTemplate<String, String> kafkaTemplate;
  private final ObjectMapper objectMapper;

  public JoinEventListener(EventBus eventBus, KafkaTemplate<String, String> kafkaTemplate, ObjectMapper objectMapper) {
    this.eventBus = eventBus;
    this.kafkaTemplate = kafkaTemplate;
    this.objectMapper = objectMapper;

    //eventBus에다가 대고 자기 자신을 regist 
    eventBus.register(this);
  }

  @Override
  public void close() throws Exception {
    eventBus.unregister(this);
  }

}
```

---

## 3. JoinEventListener 빈으로 등록

```java
//EventConfigure.java
@Configuration
@ConfigurationProperties(prefix = "eventbus")
public class EventConfigure {

  //...

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

## 4. JoinEventListener 사용 방법

+ 3번까지의 과정은 정말 '등록'만 하는 과정입니다.
+ 지금부터는 `EventListener로 뭘 할 수 있는지`에 대해 알아보겠습니다.

---

## 4-1. @Subscribe로 메소드 자동 호출

+ **`@Subscribe`이라는 키워드를 써서 메서드를 선언하면, `param 타입으로 있는 이벤트가 발생`하면 `eventBus에 의해 이 메서드가 호출됩니다.`**
  + **즉, JoinEventListener의 `handleJoinEvent` 메서드는, 나중에 JoinEvent가 발생하면 이벤트 버스에 의해 호출됩니다.**
  + 언제? param type으로 있는 JoinEvent가 발생했을 때

```java
public class JoinEventListener implements AutoCloseable {


  //...

  //param type으로 있는 JoinEvent가 발생했을 때 eventBus에 의해 이 메서드가 호출됩니다!
  @Subscribe
  public void handleJoinEvent(JoinEvent event) {
    //...
  }
}
```

---

## 4-2. JoinEventListener 확장 방법

+ 만약에 JoinEventListener에서 JoinEvent 발생 시 이메일을 보내주는 `sendEmail 로직을 추가한다면?`

```java
//JoinEventListener.java

@Subscribe
public void handleSendMail(JoinEvent event) {
  //...
}
```

+ **위 메서드를 추가해주면 `userService는 일절 건드리지 않고` JoinEvent에 대한 추가 이벤트를 선언할 수 있습니다.**

---

## 4-3. 사용 방법 요약

+ JoinEvent를 구독하는 newEventListener 사용 방법 요약 

---

1. newEventListener 생성 시 이벤트 버스에 regist
2. newEventListener 빈으로 등록
2. @Subscribe 메서드를 선언하고 JoinEvent를 Parameter로 받기


---

## 5. JoinEventListener FullCode

```java
//JoinEventListener.java

public class JoinEventListener implements AutoCloseable {

  private final EventBus eventBus;

  private final KafkaTemplate<String, String> kafkaTemplate;

  private final ObjectMapper objectMapper;

  public JoinEventListener(EventBus eventBus, KafkaTemplate<String, String> kafkaTemplate, ObjectMapper objectMapper) {
    this.eventBus = eventBus;
    this.kafkaTemplate = kafkaTemplate;
    this.objectMapper = objectMapper;

    eventBus.register(this);
  }

  @Subscribe
  public void handleJoinEvent(JoinEvent event) {
  }

  @Override
  public void close() throws Exception {
    eventBus.unregister(this);
  }
}
```

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 

