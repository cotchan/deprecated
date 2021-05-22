---
title: sb3) EventEntity
author: cotchan 
date: 2021-05-22 12:50:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 이벤트란?

+ **EventBus와 EventListener의 `구독 대상`, `관심 대상`입니다.**
+ **이벤트는 이벤트 자체만 보고도 모든 내용을 다 알 수 있도록 작성해야 합니다.**

---

## 2. Event 샘플 코드

```java
//JoinEvent.java

package com.github.prgrms.social.event;

import com.github.prgrms.social.model.commons.Id;
import com.github.prgrms.social.model.user.User;
import org.apache.commons.lang3.builder.ToStringBuilder;
import org.apache.commons.lang3.builder.ToStringStyle;

public class JoinEvent {

  private final Id<User, Long> userId;

  private final String name;

  public JoinEvent(User user) {
    this.userId = Id.of(User.class, user.getSeq());
    this.name = user.getName();
  }

  public Id<User, Long> getUserId() {
    return userId;
  }

  public String getName() {
    return name;
  }

  @Override
  public String toString() {
    return new ToStringBuilder(this, ToStringStyle.SHORT_PREFIX_STYLE)
      .append("userId", userId)
      .append("name", name)
      .toString();
  }

}
```

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 

