---
title: sb2) MockMvc mvc
author: cotchan 
date: 2021-03-02 21:48:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Test]
tags: [test_annotation] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 의미

```java
@Autowired
private MockMvc mvc;
```

+ 웹 API를 테스트할 때 사용합니다.
+ 스프링 MVC 테스트의 시작점입니다.
+ **이 클래스를 통해 `GET`, `POST` 등에 대한 API 테스트를 할 수 있습니다.**

---

## 2. 샘플 코드

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

## 3. 코드 의미 해석

---

## mvc.perform(get("/hello"))

+ **`mvc.perform(get("/hello"))`**
+ **MockMvc를 통해 /hello 주소로 HTTP GET 요청을 합니다.**
+ 체이닝이 지원되어 여러 검증 기능을 이어서 선언할 수 있습니다.


---

## .andExpect(status().isOk())

+ **`.andExpect(status().isOk())`**
+ mvc.perform의 결과를 검증합니다.
+ **HTTP Header의 Status를 검증합니다.**
+ 200, 404, 500 등의 서버 상태를 검증합니다.
+ **여기서는 OK 즉, 200인지 아닌지를 검증합니다.**

---

## .andExpect(content().string()

+ **`.andExpect(content().string(hello))`**
+ mvc.perform의 결과를 검증합니다.
+ **응답 본문의 내용을 검증합니다.**
+ Controller에서 "hello"를 리턴하기 때문에 이 값이 맞는지 검증합니다.

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
