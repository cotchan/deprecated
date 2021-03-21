---
title: sb2) 25. [스프링 시큐리티] 네이버 로그인 연동 
author: cotchan 
date: 2021-03-22 05:14:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 선행 작업

+ [1. User Domain 생성, build.gradle 의존성 추가](https://cotchan.github.io/posts/sb2-spring-security-google-login/)
+ [2. SecurityConfig, CustomOAuth2UserService, OAuthAttributes, SessionUser 클래스 생성](https://cotchan.github.io/posts/sb2-spring-security-google-login2/)

---

## 2. application-oauth.properties에 등록

+ **`application-oauth.properties` 파일 생성 후 해당 키값을 등록합니다.**
+ 네이버에서는 스프링 시큐리티를 공식 지원하지 않기 때문에 `Common-OAuth2Provider`에서 해주던 값도 전부 수동으로 입력해야 합니다.

+ `user_name_attribute=response`
  + 기준이 되는 user_name의 이름을 네이버에서는 response로 해야 합니다.
  + 이유는 네이버의 회원 조회 시 반환되는 JSON 형태 때문입니다.

```
# registration
spring.security.oauth2.client.registration.naver.client-id=클라이언트ID
spring.security.oauth2.client.registration.naver.client-secret=클라이언트 시크릿
spring.security.oauth2.client.registration.naver.redirect-uri={baseUrl}/{action}/oauth2/code/{registrationId}
spring.security.oauth2.client.registration.naver.authorization_grant_type=authorization_code
spring.security.oauth2.client.registration.naver.scope=name,email,profile_image
spring.security.oauth2.client.registration.naver.client-name=Naver

# provider
spring.security.oauth2.client.provider.naver.authorization_uri=https://nid.naver.com/oauth2.0/authorize
spring.security.oauth2.client.provider.naver.token_uri=https://nid.naver.com/oauth2.0/token
spring.security.oauth2.client.provider.naver.user-info-uri=https://openapi.naver.com/v1/nid/me
spring.security.oauth2.client.provider.naver.user_name_attribute=response
```

---

## 3. application.properties 수정

+ `application.properties` 파일에 아래 한 줄을 추가해줍니다.

```java
spring.profiles.include=oauth
```

---

## 4. 스프링 시큐리티 설정 등록

---

## 4-1. OAuthAttributes 수정

+ `OAuthAttributes`에 `네이버인지 판단하는 코드`와 `네이버 생성자`를 추가해줍니다.
  + `of` 메소드 수정
  + `ofNaver` 메소드 추가

```java
package com.cotchan.springboot.config.auth.dto;

import com.cotchan.springboot.domain.user.Role;
import com.cotchan.springboot.domain.user.User;
import lombok.Builder;
import lombok.Getter;

import java.util.Map;

@Getter
public class OAuthAttributes {

    //...

    public static OAuthAttributes of(String registrationId,
                                     String userNameAttributeName,
                                     Map<String, Object> attributes) {

        if ("naver".equals(registrationId)) {
            return ofNaver("id", attributes);
        }

        return ofGoolge(userNameAttributeName, attributes);
    }


    public static OAuthAttributes ofNaver(String userNameAttributeName,
                                          Map<String, Object> attributes) {

        Map<String, Object> response = (Map<String, Object>) attributes.get("response");

        return OAuthAttributes.builder()
                .name((String) response.get("name"))
                .email((String) response.get("email"))
                .picture((String) response.get("profile_image"))
                .attributes(response)
                .nameAttributeKey(userNameAttributeName)
                .build();
    }
    
    //...
}
``` 

---

## 4-2. 네이버 로그인 버튼 추가(index.mustache)

+ [index.mustache에 네이버 로그인 버튼 추가](https://gist.github.com/cotchan/845c82d70604646392de469071502d0b)
  + **`/oauth2/authorization/naver`**
    + **네이버 로그인 URL은 application-oauth.properties에 등록한 `redirect-uri` 값에 맞춰 자동으로 등록됩니다.**
    + `/oauth2/authorization/` 까지는 고정이고 마지막 Path만 각 소셜 로그인 코드를 사용하면 됩니다.
    + 여기서는 naver가 마지막 Path가 됩니다.

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
