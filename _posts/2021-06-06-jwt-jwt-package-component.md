---
title: JWT Module) Java JWT Library 사용 시 필요한 Component  
author: cotchan 
date: 2021-06-06 20:04:21 +0800 
categories: #
tags: [jwt] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처를 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---


## 1. configure Part

---

## 1-1. WebSecurityConfigure

+ **여기서는 사용할 `authenticationProvider`를 설정합니다.**
  + **authenticationProvider는 `JwtAuthenticationProvider`를 의미합니다.**

+ **여기서는 사용할 `JwtAuthenticationTokenFilter`를 설정합니다.**

---

## 2. controller Part

---

## 2-1. AuthRestController

+ **역할**
  + **로그인 요청(post)을 처리하는 컨트롤러입니다.** 

+ **`authenticationManager`를 사용합니다.**
+ **`JwtAuthenticationToken(인증주체)`를 사용합니다.** 

```java
@PostMapping
public ApiResult<AuthenticationResultDto> authentication(@RequestBody AuthenticationRequest authRequest) throws UnauthorizedException {
  try {

    JwtAuthenticationToken authToken = new JwtAuthenticationToken(authRequest.getPrincipal(), authRequest.getCredentials());

    Authentication authentication = authenticationManager.authenticate(authToken);

    SecurityContextHolder.getContext().setAuthentication(authentication);
    return OK(
      new AuthenticationResultDto((AuthenticationResult) authentication.getDetails())
    );
  }
}
```

---

## 3. security Part

---

## 3-1. JwtAuthenticationTokenFilter

+ **역할**
  + **`HTTP Request-Header 에서 JWT 값을 추출`하고, JWT 값이 올바르다면(`verify`) `인증주체 JwtAuthenticationToken를 생성`합니다.**
+ **`Jwt` 객체를 사용합니다.**
+ **`HeaderKey`를 사용합니다.**
  + 예제에서는 `api_key`라는 http header 키값을 통해 jwt를 받아옵니다.

---

## 3-2. JwtAuthenticationProvider 

+ **역할**
  + **`인증 주체에 대한 유효성을 검증하는 비즈니스 로직을 수행하는 컴포넌트입니다.`** 

+ 아래 네 가지 컴포넌트를 사용합니다.
  + **`Jwt`**
    + 유효한 요청에 대해서는 `실제 Jwt를 만들어야하므로` 사용합니다.
  + **`UserService`**
    + 인증 주체에 대한 `실제 검증은 UserService에서 따져야하므로` 사용합니다. 
  + **`JwtAuthenticationToken`**
    + 인증 주체에 대한 처리를 하므로 인증 주체 인스턴스를 사용합니다.
  + **`JwtAuthentication`**
    + 유효한 요청에 대해서는 인증된 사용자를 만들어줘야 하므로 사용합니다.

```java
//AuthRestController.java 에서...

Authentication authentication = authenticationManager.authenticate(authToken);
```

---

## 3-3. JwtAuthenticationToken

+ **역할**
  + **`인증주체`를 나타냅니다.**
  + **`Authentication 인터페이스의 구현체`입니다.**

+ **`principal`의 타입이 `Object` 라는 것에 주목해야 합니다.**
  + 이것은 로그인 전과 로그인 후 타입이 다르기 때문입니다.
  + 로그인 전에는 String 타입이고, 로그인 후에는 JwtAuthentication 타입입니다.

```java
public class JwtAuthenticationToken extends AbstractAuthenticationToken {

  // 인증 주체에 대한 진짜 정보는 Principal에 담깁니다.
  // 로그인 전에는 String Type의 Email
  // 로그인 후에는 JwtAuthentication Type의 인증된 사용자
  private final Object principal;

  //비밀번호
  private String credentials;

  //...
}
```

---

## 3-4. JwtAuthentication

+ **역할**
  + 인증된 사용자를 표현합니다.

```java
public class JwtAuthentication {

  public final Id<User, Long> id;

  // TODO 이름 프로퍼티 추가(완료)
  public final String name;

  public final Email email;

  //...
}
```

---

## 4. Component Image

+ Component1 : `JwtAuthenticationProvider`
+ Component2 : `JwtAuthenticationTokenFilter`

![KakaoTalk_20210606_205014685](https://user-images.githubusercontent.com/75410527/120923432-71a1e300-c709-11eb-9461-68e4d7e6c987.jpg)

---

+ 출처
  + [[Spring/Security] 초보자가 이해하는 Spring Security - 퍼옴](https://postitforhooney.tistory.com/entry/SpringSecurity-%EC%B4%88%EB%B3%B4%EC%9E%90%EA%B0%80-%EC%9D%B4%ED%95%B4%ED%95%98%EB%8A%94-Spring-Security-%ED%8D%BC%EC%98%B4)
  + [8. [springboot] Spring Security 간단 권한관리 예제 - AuthenticationProvider 방식](https://dkyou.tistory.com/20)
