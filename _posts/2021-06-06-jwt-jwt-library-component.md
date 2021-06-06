---
title: JWT Module) Java JWT Library 사용 시 알아야 할 단어(Component)
author: cotchan 
date: 2021-06-06 14:56:21 +0800 
categories: #
tags: [jwt] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처를 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

## 1. 등록된 클레임

+ 등록된 클레임들은 서비스에서 필요한 정보들이 아닌, `토큰에 대한 정보를 담기위하여 이름이 이미 정해진 클레임들`입니다.
+ 등록된 클레임의 사용은 모두 선택적(optional)입니다.

---

## 1-1. iss (issuer)

+ **`토큰 발급자`를 의미합니다.**

---

## 1-2. iat (issued at)

+ **`토큰이 발급된 시간`입니다.**
+ 이 값을 사용하여 토큰의 age가 얼마나 되었는지 판단할 수 있습니다.
+ 시간은 NumericDate 형식으로 되어있어야 합니다.
  + RFC 7519 states that the `exp` and `iat` claim values must be NumericDate values.

---

## 1-3. exp (expiration)

+ **`토큰의 만료시간`입니다.**
+ 시간은 NumericDate 형식으로 되어있어야 하며 **언제나 현재 시간보다 이후로 설정되어 있어야합니다.**
  + RFC 7519 states that the `exp` and `iat` claim values must be NumericDate values.

---

## 1-4. nbf (Not Before)

+ **`Not Before`를 의미하며, 토큰의 활성 날짜와 비슷한 개념입니다.**
+ 여기에도 NumericDate 형식으로 날짜를 지정하며, 이 날짜가 지나기 전까지는 토큰이 처리되지 않습니다.

---

## 2. JWTVerifier (do verify)

+ **jwt를 검증할 수 있는 메소드입니다.** 
  + 내부적으로 `decode`도 같이합니다.
  + 날짜, 알고리즘, Claims 검증을 합니다.
+ **검증에 실패하면(decode가 실패하거나, 토큰이 만료되었거나 등) `JWTVerificationException`이 발생합니다.**

---

```java
//Jwt.java

public final class Jwt {

  //...

  private final JWTVerifier jwtVerifier;

  public Jwt(String issuer, String clientSecret, int expirySeconds) {
    //...

    this.jwtVerifier = com.auth0.jwt.JWT.require(algorithm)
      .withIssuer(issuer)
      .build();
  }

  public Claims verify(String token) throws JWTVerificationException {
    return new Claims(jwtVerifier.verify(token));
  }

  //...
}
```

---

```java
//외부에서 사용할 때
//JwtAuthenticationTokenFilter.java
public class JwtAuthenticationTokenFilter extends GenericFilterBean {

  //...

  private final Jwt jwt;

  public JwtAuthenticationTokenFilter(String headerKey, Jwt jwt) {
    //...

    this.jwt = jwt;
  }

  //...

  private Jwt.Claims verify(String token) {
    return jwt.verify(token);
  }
 
  //...
}
```

---

+ 출처
  + [auth0 JWT 활용하기](https://heowc.tistory.com/45)
  + [[JWT] JSON Web Token 소개 및 구조](https://velopert.com/2389)
  + [What format is the exp (Expiration Time) claim in a JWT](https://stackoverflow.com/questions/39926104/what-format-is-the-exp-expiration-time-claim-in-a-jwt)
