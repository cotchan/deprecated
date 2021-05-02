---
title: sb3) 코드 재사용성을 높이는 방법(feat. 생성자로 주입받기)
author: cotchan 
date: 2021-05-02 20:00:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 생성자로 주입받는 것의 장점

+ 코드 변경없이 요구사항 변경에 대처할 수 있는가?

+ 1st: **특정 비즈니스 로직을 `대신 처리해줄 수 있는 애들을 만들면 됩니다.`**
+ 2nd: **`그리고 그 애들을 주입받으면 됩니다.`**
+ 3rd: **그러면 변경되는 요구사항에도 주입받는 클래스 자체는 변경될 필요가 없습니다.**
  + 요구사항이 변경되면 ApplicationConfig 부분(대신 처리해줄 수 있는 애들을 주입받는 부분)만 변경하면 됩니다.

---

## 2. 예시코드 (Bean을 사용하는 부분)

+ **`ConnectionBasedVoter.java`**

```java
//ConnectionBasedVoter.java

public class ConnectionBasedVoter implements AccessDecisionVoter<FilterInvocation> {

  private final RequestMatcher requiresAuthorizationRequestMatcher;

  private final Function<String, Id<User, Long>> idExtractor;

  private UserService userService;

  public ConnectionBasedVoter(RequestMatcher requiresAuthorizationRequestMatcher, Function<String, Id<User, Long>> idExtractor) {
    checkArgument(requiresAuthorizationRequestMatcher != null, "requiresAuthorizationRequestMatcher must be provided.");
    checkArgument(idExtractor != null, "idExtractor must be provided.");

    this.requiresAuthorizationRequestMatcher = requiresAuthorizationRequestMatcher;
    this.idExtractor = idExtractor;
  }

  @Override
  public int vote(Authentication authentication, FilterInvocation fi, Collection<ConfigAttribute> attributes) {
    HttpServletRequest request = fi.getRequest();

    if (!requiresAuthorization(request)) {
      return ACCESS_GRANTED;
    }

    //...

    Id<User, Long> targetId = obtainTargetId(request);

    //...

    // 친구IDs 조회 후 targetId가 포함되는지 확인한다.
    List<Id<User, Long>> connectedIds = userService.findConnectedIds(jwtAuth.id);
    if (connectedIds.contains(targetId)) {
      return ACCESS_GRANTED;
    }

    return ACCESS_DENIED;
  }

  private boolean requiresAuthorization(HttpServletRequest request) {
    return requiresAuthorizationRequestMatcher.matches(request);
  }

  private Id<User, Long> obtainTargetId(HttpServletRequest request) {
    return idExtractor.apply(request.getRequestURI());
  }

  //...
}
```

---

## 3. 예시코드 (Bean을 주입받는 부분)

+ **`WebSecurityConfigure.java`**

```java
//WebSecurityConfigure.java

@Configuration
@EnableWebSecurity
public class WebSecurityConfigure extends WebSecurityConfigurerAdapter {

  public WebSecurityConfigure(Jwt jwt, JwtTokenConfigure jwtTokenConfigure, JwtAccessDeniedHandler accessDeniedHandler, EntryPointUnauthorizedHandler unauthorizedHandler) {
    this.jwt = jwt;
    this.jwtTokenConfigure = jwtTokenConfigure;
    this.accessDeniedHandler = accessDeniedHandler;
    this.unauthorizedHandler = unauthorizedHandler;
  }

  @Override
  public void configure(WebSecurity web) {
    web.ignoring().antMatchers("/swagger-resources", "/webjars/**", "/static/**", "/templates/**", "/h2/**");
  }

  @Autowired
  public void configureAuthentication(AuthenticationManagerBuilder builder, JwtAuthenticationProvider authenticationProvider) {
    // AuthenticationManager 는 AuthenticationProvider 목록을 지니고 있다.
    // 이 목록에 JwtAuthenticationProvider 를 추가한다.
    builder.authenticationProvider(authenticationProvider);
  }

  //...

  @Bean
  public ConnectionBasedVoter connectionBasedVoter() {
    final String regex = "^/api/user/(\\d+)/post/.*$";
    final Pattern pattern = Pattern.compile(regex);
    RequestMatcher requiresAuthorizationRequestMatcher = new RegexRequestMatcher(pattern.pattern(), null);
    return new ConnectionBasedVoter(
      requiresAuthorizationRequestMatcher,
      (String url) -> {
        /* url에서 targetId를 추출하기 위해 정규식 처리 */
        Matcher matcher = pattern.matcher(url);
        long id = matcher.matches() ? toLong(matcher.group(1), -1) : -1;
        return Id.of(User.class, id);
      }
    );
  }

  //...
}
```



---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
