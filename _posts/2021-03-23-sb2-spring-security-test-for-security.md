---
title: sb2) 26. [스프링 시큐리티] 기존 테스트에 시큐리티 적용하기
author: cotchan 
date: 2021-03-23 00:00:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 테스트 코드 수정 이유

+ 끝으로 기존 테스트에 시큐리티 적용으로 문제가 되는 부분들을 수정하겠습니다.
+ **문제가 되는 부분**
  + 기존에는 바로 API를 호출할 수 있어 테스트 코드 역시 바로 API를 호출
  + **하지만, 시큐리티 옵션이 활성화되면 인증된 사용자만 API를 호출할 수 있습니다.**

+ 기존의 API 테스트 코드들이 모두 인증에 대한 권한을 받지 못하였으므로, 테스트 코드마다 인증한 사용자가 호출한 것처럼 작동하도록 수정

---

## 2. CustomOAuth2UserService 찾을 수 없음

+ **Error Message**
  + **`No qualifying bean of type 'com.cotchan.review.springboot.config.auth.CustomOAuth2UserService'`**

+ **이유**
  + CustomOAuth2UserService를 생성하는데 필요한 소셜 로그인 관련 설정값들이 없기 때문
  + `src/main`에만 있는 `application-oauth.properties` 설정값을 `src/test`에서는 읽을 수 없음
  + **test에 `application.properties`가 없으면 `main`의 설정을 그대로 가져옵니다.**

+ **해결 방법**
  + 테스트 환경을 위한 `application.properties` 만들기
  + 실제 구글 연동까지 진행할 것은 아니므로 가짜 설정값으로 등록합니다.

```
// src/test/resources/application.properties

spring.jpa.show_sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MySQL5InnoDBDialect
spring.h2.console.enabled=true
spring.session.store-type=jdbc

# Test OAuth
spring.security.oauth2.client.registration.google.client-id=test
spring.security.oauth2.client.registration.google.client-secret=test
spring.security.oauth2.client.registration.google.scope=profile,email
```

---

## 3. 302 Status Code

+ **Error Message**
  + Expected :200 OK
  + Actual   :302 FOUND

+ **이유**
  + 스프링 시큐리티 설정 때문에 인증되지 않은 사용자의 요청은 이동시키기 때문

+ **해결 방법**
  + **이런 API 요청은 `임의로 인증된 사용자를 추가`하여 API만 테스트 해 볼 수 있도록 합니다.**
  + 스프링 시큐리티에서 공식적으로 방법을 지원하고 있습니다.
    + 스프링 시큐리티 테스트를 위한 여러 도구를 지원하는 `spring-security-test`를 build.gradle에 추가합니다.


+ build.gradle에 `spring-security-test` 추가

```
//build.gradle
dependencies {
    testCompile('org.springframework.security:spring-security-test')
}
```

+ PostApiControllerTest의 2개 테스트 메소드에 `임의의 사용자 인증을 추가`

```java
@Test
@WithMockUser(roles = "USER")
public void Posts_등록된다() throws Exception {
//...
```

---

```java
@Test
@WithMockUser(roles = "USER")
public void Posts_수정된다() throws Exception {
//...
```

---

## 4. @WebMvcTest에서 CustomOAuth2UserService 찾을 수 없음

+ HelloControllerTest는 `@WebMvcTest`를 사용합니다.
+ 2번 과정을 통해 스프링 시큐리티 설정은 잘 작동합니다.
+ **그러나 `@WebMvcTest`는 `CustomOAuth2UserService`를 스캔하지 않기 때문입니다.**

+ @WebMvcTest는 WebSecurityConfigurerAdapter, WebMvcConfigurer를 비롯한 `@ControllerAdivce`, `@Controller`를 읽습니다. 
+ **`@Repository`, `@Service`, `@Component`는 스캔 대상이 아닙니다.**
+ 그러니 `SecurityConfig`는 읽었지만, SecurityConfig를 생성하기 위해 필요한 `CustomOAuth2UserService`는 읽을 수 없어서 위와 같은 에러가 발생한 것입니다.

+ 문제 해결을 위해 아래와 같이 **스캔 대상에서 `SecurityConfig`를 제거합니다.**

```java
@RunWith(SpringRunner.class)
@WebMvcTest(controllers = HelloController.class,
        excludeFilters = {
        @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE,
                        classes = SecurityConfig.class)
        }
)
public class HelloControllerTest {
```

---

+ **그리고 추가로 `@WithMockUser`를 사용해서 가짜로 인증된 사용자를 생성합니다.**

```java
//HelloControllerTest
@Test
@WithMockUser(roles = "USER")
public void hello가_리턴된다() throws Exception {
//...
```

---

```java
//HelloControllerTest
@Test
@WithMockUser(roles = "USER")
public void helloDto가_리턴된다() throws Exception {
//...
```

---

## 5. JPA metamodel must not be empty!

+ 4번 과정을 진행하고 다시 테스트를 돌리면 아래와 같은 추가 에러가 발생합니다.
+ `java.lang.IllegalArgumentException: JPA metamodel must not be empty!`

+ 이 에러는 `@EnableJpaAuditing`으로 인해 발생합니다.
  + `@EnableJpaAuditing`을 사용하기 위해선 최소 하나의 `@Entity` 클래스가 필요합니다.
  + `@WebMvcTest`이다 보니 당연히 없습니다.

+ `@EnableJpaAuditing`이 `@SpringBootApplication`과 함께 있다보니 `@WebMvcTest`에서도 스캔을 하게 되었습니다.
+ 그래서 `@EnableJpaAuditing`과 `@SpringBootApplication` 둘을 분리하도록 합니다.

---

+ 1st. **Application.java에서 @EnableJpaAuditing 제거**

```java
// @EnableJpaAuditing 삭제
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

---

+ 2nd. **config 패키지에 `JpaConfig`를 생성하여 @EnableJpaAuditing 추가**
  
```java
//src/main/java/com/cotchan/review/springboot/config/JpaCofig

import org.springframework.context.annotation.Configuration;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

@Configuration
@EnableJpaAuditing  //JPA Auditing 활성화
public class JpaConfig { }
```

---

## 6. 1~5번 코드 수정 결과

+ 모든 테스트를 통과하는 것을 확인할 수 있습니다.

![Desktop View](/assets/img/post/spring-boot2/2021-03-23-spring-security-test.png)

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
