---
title: sb3) Health Check API (feat. ApiResult)
author: cotchan 
date: 2021-05-23 02:31:21 +0800 
categories: [Spring-Boot3]
tags: [spring-module] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 헬스체크 API

+ `현재 Unix Timestamp를 반환`하는 헬스체크 API
  + **`System.currentTimeMillis()`를 사용해서 간단하게 구현할 수 있습니다.**

```java
package com.github.prgrms.social.controller;

import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import static com.github.prgrms.social.controller.ApiResult.OK;

@RestController
@RequestMapping("api")
public class HealthCheckRestController {

    @GetMapping(path = "_hcheck")
    public ApiResult<Long> healthCheck() {
        return OK(System.currentTimeMillis());
    }
}
```

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 

