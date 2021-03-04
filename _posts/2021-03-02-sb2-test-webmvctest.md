---
title: sb2) @WebMvcTest
author: cotchan 
date: 2021-03-02 21:45:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Test]
tags: [test_annotation] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 의미

+ Web(Spring MVC)에 집중할 수 있는 어노테이션입니다.
+ **선언할 경우 `@Controller`, `@ControllerAdvice` 등을 사용할 수 있습니다.**
+ **단, `@Service`, `@Component`, `@Repository` 등은 사용할 수 없습니다.**
+ **또한 @WebMvcTest의 경우 `JPA` 기능이 작동하지 않습니다.**
  + 그러므로 JPA 기능까지 테스트할 때는 `@SpringBootTest`와 `TestRestTemplate`을 사용하면 됩니다.
+ 아래 샘플 코드는 컨트롤러만 사용하기 때문에 선언합니다.

---


## 2. 샘플 코드

+ 코드의 의미는 아래의 출처에서 확인할 수 있습니다.
  + [sb2) MockMvc mvc](https://cotchan.github.io/posts/sb2-test-mockmvc/)

```java
package com.cotchan.review.springboot.web;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;


@RunWith(SpringRunner.class)
@WebMvcTest(controllers = HelloController.class)
public class HelloControllerTest {

    @Autowired
    private MockMvc mvc;

    @Test
    public void hello가_리턴된다() throws Exception {
        String hello = "hello";

        mvc.perform(get("/hello"))
                .andExpect(status().isOk())
                .andExpect(content().string(hello));
    }
}
```





---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
