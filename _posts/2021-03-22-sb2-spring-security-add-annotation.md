---
title: sb2) 23. [스프링 시큐리티] 구글 로그인 연동 - 4(세션 어노테이션 코드로 수정) 
author: cotchan 
date: 2021-03-22 04:00:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. @LoginUser 사용 환경 만들기

+ index 메소드 외에 다른 컨트롤러와 메소드에서 세션값이 필요하면 그때마다 직접 세션에서 값을 가져와야 합니다.
+ **그래서 이 부분을 메소드 인자로 세션값을 바로 받을 수 있도록 변경합니다.**

```java
//중복 코드 개선
SessionUser user = (SessionUser) httpSession.getAttribute("user");
```

---

## 1-1. @LoginUser 생성

+ `config.auth` 패키지

+ **`@Target(ElementType.PARAMETER)`**
  + 이 어노테이션이 생성될 수 있는 위치를 지정합니다.
  + `PARAMETER`로 지정했으니 **메소드의 파라미터로 선언된 객체에서만 사용**할 수 있습니다.
  + 이 외에도 클래스 선언문에 쓸 수 있는 TYPE 등이 있습니다.

+ **`@interface`**
  + 이 파일을 어노테이션 클래스로 지정합니다.
  + LoginUser라는 이름을 가진 어노테이션이 생성되었습니다.

```java
package com.cotchan.review.springboot.config.auth;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.PARAMETER)
@Retention(RetentionPolicy.RUNTIME)
public @interface LoginUser {

}
```

---


## 1-2. LoginUserArgumentResolver 생성

+ 이 클래스는 한 가지 기능을 지원합니다.
+ 조건에 맞는 경우 메소드가 있다면 `HandlerMethodArgumentResolver`의 구현체가 지정한 값으로 해당 메소드의 파라미터로 넘길 수 있습니다.

+ `config.auth` 패키지


```java
package com.cotchan.review.springboot.config.auth;

import com.cotchan.review.springboot.config.auth.dto.SessionUser;
import lombok.RequiredArgsConstructor;
import org.springframework.core.MethodParameter;
import org.springframework.stereotype.Component;
import org.springframework.web.bind.support.WebDataBinderFactory;
import org.springframework.web.context.request.NativeWebRequest;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.method.support.ModelAndViewContainer;

import javax.servlet.http.HttpSession;

@RequiredArgsConstructor
@Component
public class LoginUserArgumentResolver implements HandlerMethodArgumentResolver {

    private final HttpSession httpSession;

    @Override
    public boolean supportsParameter(MethodParameter parameter) {   //11111
        boolean isLoginUserAnnotation = parameter.getParameterAnnotation(LoginUser.class) != null;
        boolean isUserClass = SessionUser.class.equals(parameter.getParameterType());
        return isLoginUserAnnotation && isUserClass;
    }

    @Override   //22222
    public Object resolveArgument(MethodParameter parameter,
                                  ModelAndViewContainer mavContainer,
                                  NativeWebRequest webRequest,
                                  WebDataBinderFactory binderFactory) throws Exception {
        return httpSession.getAttribute("user");
    }
}
```

++ **`11111`**
  + `supportsParameter()`
    + 컨트롤러 메서드의 특정 파라미터를 지원하는지 판단합니다.
    + 여기서는 파라미터에 @LoginUser 어노테이션이 붙어 있고, 파라미터 클래스 타입이 SessionUser.class인 경우 true를 반환합니다.

++ **`22222`**
  + `resolveArgument()`
    + 파라미터에 전달할 객체를 생성합니다.
    + 여기서는 세션에서 객체를 가져옵니다.

---

## 2. WebMvcConfigurer에 추가

+ 1번 과정을 통해 `@LoginUser`를 사용하기 위한 환경이 구성되었습니다.
+ 이제 이렇게 생성된 `LoginUserArgumentResolver`가 스프링에서 인식될 수 있도록 `WebMvcConfigurer`에 추가합니다.
+ `config` 패키지
+ `HandlerMethodArgumentResolver`는 항상 `WebMvcConfigurer`의 `addArgumentResolvers()`를 통해 추가해야합니다.
  + 다른 Handler-MethodArgumentResolver가 필요하다면 같은 방식으로 추가해주면 됩니다.


```java
package com.cotchan.review.springboot.config;

import com.cotchan.review.springboot.config.auth.LoginUserArgumentResolver;
import lombok.RequiredArgsConstructor;
import org.springframework.context.annotation.Configuration;
import org.springframework.web.method.support.HandlerMethodArgumentResolver;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

import java.util.List;

@RequiredArgsConstructor
@Configuration
public class WebConfig implements WebMvcConfigurer {

    private final LoginUserArgumentResolver loginUserArgumentResolver;

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> argumentResolvers) {
        argumentResolvers.add(loginUserArgumentResolver);
    }
}
```

---

## 3. 이전 코드 @LoginUser로 수정

+ **after**

```java
//IndexController
@GetMapping("/")
public String index(Model model, @LoginUser SessionUser user) {
    model.addAttribute("posts", postsService.findAllDesc());

    if (user != null) {
        model.addAttribute("userName", user.getName());
    }

    return "index";
}
```

+ **`@LoginUser SessionUser user`**
  + 기존에 `(SessionUser) httpSession.getAttribute("user")`로 가져오던 세션 정보 값이 개선되었습니다.
  + 이제는 어느 컨트롤러든지 @LoginUser만 사용하면 세션 정보를 가져올 수 있게 되었습니다.

---

+ **before**

```java
//IndexController
@GetMapping("/")
public String index(Model model) {
    model.addAttribute("posts", postsService.findAllDesc());

    SessionUser user = (SessionUser) httpSession.getAttribute("user");

    if (user != null) {
        model.addAttribute("userName", user.getName());
    }

    return "index";
}
```

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
