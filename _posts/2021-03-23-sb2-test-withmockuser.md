---
title: sb2) @WithMockUser(roles = "USER")
author: cotchan 
date: 2021-03-23 04:20:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Test]
tags: [test_annotation] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 의미

+ 인증된 모의(가짜) 사용자를 만들어서 사용합니다.
+ roles에 권한을 추가할 수 있습니다.
+ **즉, 이 어노테이션으로 인해 ROLE_USER 권한을 가진 사용자가 API를 요청하는 것과 동일한 효과를 가집니다.**
+ **`@WithMockUser`는 `MockMvc`에서만 작동합니다.**

---

## 2. 샘플 코드

```java
@Test
@WithMockUser(roles = "USER")
public void Posts_등록된다() throws Exception {
//...
```

---

```java
@Test
@WithMockUser(roles = "USER")
public void Posts_수정된다() throws Exception {
//...
```

---

## 3. MockMvc를 사용하도록 수정

+ [sb2) @SpringBootTest에서 MockMvc를 사용하는 방법](https://cotchan.github.io/posts/sb2-test-springboottest-using-mockmvc/)

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
