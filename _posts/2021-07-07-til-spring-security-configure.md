---
title: TIL) Spring-Security 적용 중 doFilter 삽질
author: cotchan
date: 2021-07-07 21:32:00 +0800
categories: #
tags: [til]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 문제

+ `.permitAll()` 옵션을 걸었는데도 healthCheck API 응답을 받을 수 없었다.

---

## 2. 문제 원인

+ **필터 체인이 끊겨있었습니다.**
+ **필터체인에서 `chain.doFilter(request, response)`을 호출하지 않아 발생한 문제**

```java
//문제 코드

@RequiredArgsConstructor
public class JwtAuthenticationTokenFilter extends GenericFilterBean {

    //...

    //요청이 들어오면 doFilter를 탄다.
    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;

        //아래 코드가 주석 처리 되어있었음
        //chain.doFilter(request, response);
    }
}
```

+ **그래서 request에 대한 Filter가 끊겨서 요청이 `RestController`까지 도달하지 않았었다.**

---

## 3. 해결 방법

+ **`FilterChain` 부분을 연결시켜서 해결하였다.**

```java
@RequiredArgsConstructor
public class JwtAuthenticationTokenFilter extends GenericFilterBean {

    //...

    //요청이 들어오면 doFilter를 탄다.
    @Override
    public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) req;
        HttpServletResponse response = (HttpServletResponse) res;
 
        //do something       
        chain.doFilter(request, response);
    }
}
```

---

## 4. 덕분에 알게된 한 가지

+ **SpringSecurity를 적용 후 `configure` 함수를 오버라이딩하면 URL 별로 접근 권한을 조정할 수 있습니다.**
  + 즉, 모든 API에서 로그인을 요청하지 않고, 사용자 권한과 무관한 HealthCheck API를 사용할 수도 있습니다.

```java
@Configuration
@EnableWebSecurity
public class SecurityConfigure extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        //
    }
}
```

---

+ 출처
  + []()
