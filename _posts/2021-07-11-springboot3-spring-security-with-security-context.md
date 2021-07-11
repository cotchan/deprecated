---
title: sb3) 권한을 이용한 테스트 코드 작성(@WithMockCustomUser, @WithSecurityContext) 
author: cotchan 
date: 2021-07-11 22:07:21 +0800 
categories: [Spring, Spring_Security]
tags: [spring-security] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ 아래 출처 글을 참고하여 작성하였습니다.

---

## 1. SecurityContext?

+ 스프링 시큐리티는 `SecurityContext에` 인증된 `Authentication 객체`를 넣어두고 현재 스레드 내에서 공유되도록 관리하고 있습니다.

---

## 2-1. @WithSecurityContext
## 2-2. @WithMockCustomUser 

+ **`@WithSecurityContext`를 이용해서 우리가 원하는 커스텀 SecurityContext를 만들 수 있습니다.**
+ **또한 `SecurityContext`에 `Authentication 객체`를 만드는데 필요한 `@WithMockCustomUser` 어노테이션을 정의합니다.**

```java
//sample1

@Retention(RetentionPolicy.RUNTIME)
@WithSecurityContext(factory = WithMockCustomUserSecurityContextFactory.class)
public @interface WithMockCustomUser {

    String username() default "rob";

    String name() default "Rob Winch";
}
```

---

```java
//sample2

import org.springframework.security.test.context.support.WithSecurityContext;

import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

@Retention(RetentionPolicy.RUNTIME)
@WithSecurityContext(factory = WithMockCustomUserSecurityContextFactory.class)
public @interface WithMockCustomUser {

    int id() default 23;

    String userId() default "zoroKevin";

    String nickName() default "test001";

    String role() default "ROLE_USER";
}
```


---

## 3. WithSecurityContextFactory 

+ **위에서 정의한 `@WithMockCustomUser`는 커스텀 SecurityContext에 인증 객체 정보를 담을 때 사용합니다.**
+ **그러므로 Authentication 객체에 SecurityContext를 제공하기 위해 SecurityContextFactory를 커스터마이징 하는 것이 필요합니다.**
  + 커스텀 SecurityContextFactory는 아래 코드처럼 구현할 수 있습니다.

```java
//sample1

public class WithMockCustomUserSecurityContextFactory
    implements WithSecurityContextFactory<WithMockCustomUser> {
    @Override
    public SecurityContext createSecurityContext(WithMockCustomUser customUser) {
        SecurityContext context = SecurityContextHolder.createEmptyContext();

        CustomUserDetails principal =
            new CustomUserDetails(customUser.name(), customUser.username());
        Authentication auth =
            new UsernamePasswordAuthenticationToken(principal, "password", principal.getAuthorities());
        context.setAuthentication(auth);
        return context;
    }
}
```


---

```java
//sample2

import org.springframework.security.core.context.SecurityContext;
import org.springframework.security.core.context.SecurityContextHolder;
import org.springframework.security.test.context.support.WithSecurityContextFactory;

import static org.springframework.security.core.authority.AuthorityUtils.createAuthorityList;

public class WithMockCustomUserSecurityContextFactory
        implements WithSecurityContextFactory<WithMockCustomUser> {
    @Override
    public SecurityContext createSecurityContext(WithMockCustomUser customUser) {
        SecurityContext context = SecurityContextHolder.createEmptyContext();

        JwtAuthenticationToken authentication =
                new JwtAuthenticationToken(
                        new JwtAuthentication(customUser.id(), customUser.userId(), customUser.nickName()),
                        null,
                        createAuthorityList(customUser.role())
                );

        context.setAuthentication(authentication);
        return context;
    }
}
```

---

## 4. @WithMockCustomUser 사용하기

+ **3번까지 과정을 거치면 권한을 테스트하기 위한 준비는 끝났습니다.**
+ **이제는 `@WithMockCustomUser` 어노테이션을 사용하여 인증된 사용자가 필요한 API 테스트 코드를 작성할 수 있습니다.**

+ **`UserRestControllerTest.java`**

```java
//이 요청은 USER 권한이 있는 사용자만 호출할 수 있는 API입니다.

@SpringBootTest
@AutoConfigureMockMvc
@TestInstance(TestInstance.Lifecycle.PER_CLASS)
class UserRestControllerTest {

    private final MockMvc mockMvc;

    @Autowired
    public UserRestControllerTest(MockMvc mockMvc) {
        this.mockMvc = mockMvc;
    }

    @Test
    @WithMockCustomUser
    @DisplayName("내 정보 조회 성공 테스트 (jwt 토큰이 올바른 경우)")
    void meSuccessTest() throws Exception {
        ResultActions result = mockMvc.perform(
                get("/api/user/me")
                        .accept(MediaType.APPLICATION_JSON)
        );

        result.andDo(print())
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.success", is(true)))
                .andExpect(jsonPath("$.response.seq", is(23)))
                .andExpect(jsonPath("$.response.userId", is("zoroKevin")))
                .andExpect(jsonPath("$.response.nickName", is("test001")))
        ;
    }
}
```

---

+ **`UserRestController.java`** 

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
  + [Spring Security Test](https://smjeon.dev/etc/with-mock-user/)
