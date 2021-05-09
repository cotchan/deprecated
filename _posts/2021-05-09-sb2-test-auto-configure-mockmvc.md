---
title: sb2) @AutoConfigureMockMvc
author: cotchan 
date: 2021-05-09 21:12:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Test]
tags: [test_annotation] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 의미

```java
@SpringBootTest
@AutoConfigureMockMvc
@RunWith(SpringRunner.class)
public class GeneralExceptionHandlerTest {
```

+ **기본적으로 @SpringBootTest를 사용하면 `MockMvc을 빈으로 등록시키지 않습니다.`**
+ **그러므로 `MockMvc를 이용하기 위해서는` `@AutoConfigureMockMvc`을 선언해야 합니다.**

---

## 2. 샘플 코드

```java
@SpringBootTest
@AutoConfigureMockMvc
@RunWith(SpringRunner.class)
public class GeneralExceptionHandlerTest {

    @Autowired
    private MockMvc mockMvc;

    @MockBean
    UserRestController userRestController;

    @MockBean
    PostRestController postRestController;

    @MockBean
    AuthenticationRestController authenticationRestController;

    //...
    //@Test
}
```

---

+ 출처
  + [@SpringBootTest와 @WebMvcTest, @MockBean 그리고 WebClinet까지](https://gracelove91.tistory.com/88)
