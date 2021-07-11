---
title: TIL) hamcrest로 가독성있는 jUnit Test Case 만들기
author: cotchan
date: 2021-07-11 23:37:00 +0800
categories: #
tags: [til]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. hamcrest?

+ 다양한 조건의 Match rule 을 손쉽게 작성하고 테스트 할 수 있는 library로 jUnit 이나 Mokoto 와 연계하여 사용할 수 있습니다.

---

## 2. Example

---

## 2-1. hamcrest.Matchers.is

+ **`Sample 1`**

```java
import static org.hamcrest.Matchers.anything;
import static org.hamcrest.Matchers.notNullValue;
import static org.hamcrest.Matchers.nullValue;
import static org.junit.Assert.assertThat;
import static org.hamcrest.Matchers.is;

 
// null 을 기대하는데 null 이므로 성공
@Test
public void nullTest() {
	String str = null;
	assertThat(str,  is(nullValue()));
}
 
// notnull 을 기대하는데 null 이므로 실패
@Test
public void notNullTest() {
	String str = null;
	assertThat(str,  is(notNullValue()));
}
```

---

+ **`Sample 2`**

```java
import static org.hamcrest.Matchers.is;
import static org.junit.jupiter.api.Assertions.assertArrayEquals;

@TestInstance(TestInstance.Lifecycle.PER_CLASS)
public class JwtTest {

    private Jwt jwt;

    @BeforeAll
    void setUp() {
        String issuer = "algorithm_quiz_curator";
        String clientSecret = "yhcycyyh";
        int expirySeconds = 10;

        jwt = new Jwt(issuer, clientSecret, expirySeconds);
    }

    @Test
    void JWT_토큰을_생성하고_복호화_할수있다() {
        Jwt.Claims claims = Jwt.Claims.of(1,"tester", "cotchanKevein", new String[]{"ROLE_USER"});
        String encodedJWT = jwt.newToken(claims);
        log.info("encodedJWT: {}", encodedJWT);

        Jwt.Claims decodedJWT = jwt.verify(encodedJWT);
        log.info("decodedJWT: {}", decodedJWT);

        assertThat(claims.userKey, is(decodedJWT.userKey));
        assertArrayEquals(claims.roles, decodedJWT.roles);
    }
}
```

---

## 2-2. hamcrest.Matchers.not

+ **`Sample 1`**

```java
import static org.hamcrest.Matchers.anything;
import static org.hamcrest.Matchers.notNullValue;
import static org.hamcrest.Matchers.nullValue;
import static org.junit.Assert.assertThat;
import static org.hamcrest.Matchers.is;
import static org.hamcrest.Matchers.not;


String str = null;
assertThat(str,  not(notNullValue()));  //성공: not null이 아니므로 => null 
assertThat(str,  not(nullValue())); //실패: null이 아님을 기대했는데 null이므로
```

---

+ **`Sample 2`**

```java
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
public class JwtTest {

    private final Logger log = LoggerFactory.getLogger(getClass());

    private Jwt jwt;

    @BeforeAll
    void setUp() {
        String issuer = "algorithm_quiz_curator";
        String clientSecret = "yhcycyyh";
        int expirySeconds = 10;

        jwt = new Jwt(issuer, clientSecret, expirySeconds);
    }

    @Test
    void JWT_토큰을_리프레시_할수있다() throws Exception {
        if (jwt.getExpirySeconds() > 0) {
            Jwt.Claims claims = Jwt.Claims.of(1,"tester", "cotchanKevein", new String[]{"ROLE_USER"});
            String encodedJWT = jwt.newToken(claims);
            log.info("encodedJWT: {}", encodedJWT);

            // 1초 대기 후 토큰 갱신
            sleep(1000L);

            String encodedRefreshedJWT = jwt.refreshToken(encodedJWT);
            log.info("encodedRefreshedJWT: {}", encodedRefreshedJWT);

            assertThat(encodedJWT, not(encodedRefreshedJWT));
            //...
        }
    }
}
```

---

+ 출처
  + [hamcrest 로 가독성있는 jUnit Test Case 만들기](https://www.lesstif.com/java/hamcrest-junit-test-case-18219426.html)
