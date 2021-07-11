---
title: sb3) @AuthenticationPrincipal
author: cotchan 
date: 2021-07-11 23:09:21 +0800 
categories: [Spring, Spring_Security]
tags: [spring-security] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ 아래 포스팅을 참고하여 작성하였습니다.

---

## 1. @AuthenticationPrincipal?

+ @AuthenticationPrincipal는 인증 과정이후 **현재 인증된 세션유저를 가져오기 위해 `@AuthenticationPrincipal` 어노테이션을 통해 `Authentication` 인터페이스를 구현한 인증 객체의 `Principal`값을 주입할 때 사용하는 편입니다.**

---

## 2. 동작 과정

1. `SecurityContextHolder.getContext().getAuthentication()`을 통해 `Authentication` 객체를 가져옵니다.

2. 인증 객체에서 `principal` 값을 얻습니다.

3. `@AuthenticationPrincipal` 어노테이션이 붙은 파라미터가 존재하는지 찾습니다.

4. **`@AuthenticationPrincipal` 어노테이션이 붙은 파라미터가 존재한다면 해당 `principal 값을 리턴`합니다.**

---

## 3. Sample Code

---

## 3-1. Authentication 객체

```java
//인증 주체를 나타냅니다. (Authentication 객체)

public class JwtAuthenticationToken extends AbstractAuthenticationToken {

    //인증이 완료되면 principal은 JwtAuthentication 값으로 타입캐스팅 됩니다.
    private final Object principal;

    //비밀번호
    private String credentials;

    //...
}
```

---

## 3-2. principal 값

```java
/**
 * 인증된 사용자를 의미합니다.
 */
public class JwtAuthentication {

    public final Integer seq;

    public final String userId;

    public final String nickName;

    //...
}
```

---

## 3-3. @AuthenticationPrincipal 사용

```java
@RestController
@RequestMapping("api")
@RequiredArgsConstructor
public class UserRestController {

    private final Jwt jwt;
    private final UserService userService;

    //...

    @GetMapping(path = "user/me")
    public ApiResult<UserDto> me(@AuthenticationPrincipal JwtAuthentication authentication) {
        return OK(
                userService.findById(authentication.seq)
                        .map(UserDto::new)
                        .orElseThrow(() -> new NotFoundException(User.class, authentication.seq))
        );
    }
}
```

---

+ 출처
  + [[Spring Security] @AuthenticationPrincipal 어노테이션은 어떻게 동작할까??](https://sas-study.tistory.com/410)
