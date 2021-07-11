---
title: sb3) @TestInstance
author: cotchan 
date: 2021-07-11 22:35:21 +0800 
categories: [Spring-Boot3]
tags: [test_annotation] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ 아래 출처를 참고하여 작성하였습니다.

---

## 1. @TestInstance란?

+ TestInstance는 테스트 인스턴스의 라이프 사이클을 설정할 때 사용합니다.
  + **`PER_METHOD` : test 함수 당 인스턴스가 생성됩니다.**
  + **`PER_CLASS` : test 클래스 당 인스턴스가 생성됩니다.**

---

## 2. 사용 시 장점

+ **`라이프 사이클을 클래스 단위로 지정`해 놓으면 `@BeforeAll`, `@AfterAll` 어노테이션을 static method가 아닌 곳에서도 사용할 수 있습니다.**

```java
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
public class JwtTest {

    private Jwt jwt;

    @BeforeAll
    void setUp() {
        String issuer = "cotchan";
        String clientSecret = "cotchan";
        int expirySeconds = 10;

        jwt = new Jwt(issuer, clientSecret, expirySeconds);
    }

    //@Test Method
    //...
}
```



---

+ 출처
  + [[JUnit5] @TestInstance](https://withhamit.tistory.com/300)

