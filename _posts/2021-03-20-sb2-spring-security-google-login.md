---
title: sb2) 20. [스프링 시큐리티] 구글 로그인 연동 - 1(User Domain 생성, build.gradle 의존성 추가)
author: cotchan 
date: 2021-03-20 16:32:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. User 클래스 만들기

+ **먼저 `사용자 정보를 담당`할 도메인인 `User 클래스`를 생성합니다.**

+ domain 아래에 user 패키지 만들기
  + `domain/user/User`

```java
package com.cotchan.randomproblem_local.domain.user;

import lombok.AccessLevel;
import lombok.Builder;
import lombok.Getter;
import lombok.NoArgsConstructor;

import javax.persistence.*;

@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false)
    private String email;

    @Column
    private String picture;

    @Enumerated(EnumType.STRING)
    @Column(nullable = false)
    private Role role;

    @Builder
    public User(String name, String email, String picture, Role role) {
        this.name = name;
        this.email = email;
        this.picture = picture;
        this.role = role;
    }

    public User update(String name, String picture) {
        this.name = name;
        this.picture = picture;
        return this;
    }

    public String getRoleKey() {
        return this.role.getKey();
    }

}
```

---

+ 각 사용자의 권한을 관리할 Enum 클래스 Role도 생성합니다. 
  + `domain/user/User`
+ **스프링 시큐리티에서는 권한 코드에 항상 `ROLE_`이 앞에 있어야만 합니다.** 
+ **그래서 코드별 키 값을 `ROLE_GUEST`, `ROLE_USER` 등으로 지정합니다.**


```java
package com.cotchan.randomproblem_local.domain.user;

import lombok.Getter;
import lombok.RequiredArgsConstructor;

/**
 * 각 사용자의 권한을 관리할 Enum 클래스
 */
@Getter
@RequiredArgsConstructor
public enum Role {

    GUEST("ROLE_GUEST", "손님"),
    USER("ROLE_USER", "일반 사용자");

    private final String key;
    private final String title;
}
```

---

## 2. UserRepository 생성

+ User의 CRUD를 책임질 UserRepository

+ **`findByEmail`**
  + 소셜 로그인으로 반환되는 값 중 email을 통해 이미 생성된 사용자인지, 처음 가입하는 사용자인지 판단합니다.

```java
package com.cotchan.randomproblem_local.domain.user;

import org.springframework.data.jpa.repository.JpaRepository;

import java.util.Optional;

public interface UserRepository extends JpaRepository<User, Long> {

    Optional<User> findByEmail(String email);
}
```

---

## 3. 스프링 시큐리티 설정

+ `build.gradle`에 스프링 시큐리티 관련 의존성을 추가합니다.

+ `spring-boot-starter-oauth2-client`
  + 소셜 로그인 등 클라이언트 입장에서 소셜 기능 구현 시 필요한 의존성
  + spring-security-oauth2-client와 spring-security-oauth2-jose를 기본으로 관리해줍니다.

```
//build.gradle

compile('org.springframework.boot:spring-boot-starter-oauth2-client')
```

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
