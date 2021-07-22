---
title: Java) Optional 사용 방법
author: cotchan 
date: 2021-05-02 20:53:21 +0800 
categories: [Java]
tags: [java] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ 아래 출처를 참고하여 작성한 글입니다.

---

## 1. Optional을 사용해야 할 때

+ **어떤 메서드가 Object를 리턴하는데 `null일 가능성이 있다?` 그렇다면 Optional로 래핑하는 것을 권장합니다.**
+ Optional을 쓰면서 생기는 성능적인 미묘한 차이보다 Optional을 씀으로 생기는 이득이 압도적입니다.
  + 일단 return Type이 optional로 되어 있으면 `"이 값이 empty 일 수 있구나"` 이거를 인지하고 시작합니다.


---

## 2. Optional답게 사용하는 방법

+ Optional을 Optional 답게 사용하려면 아래의 함수들로 Optional을 처리하는 게 좋습니다.

---

## 2-1. .map

+ Optional.map은 값이 있으면 이 값을 꺼내와서 .map 안의 코드블럭이 실행됩니다.
+ **java에서 `map이라는 함수는 A type을 B type으로 바꾼다는 뜻`입니다. (매핑한다.)**
  + 여기에는 A type → A type도 포함하는 의미입니다. 

---

## 2-2. .orElse / .orElseGet

+ **차이점**
  + **orElse()는 `Optional 내부 객체의 상태와 상관없이 무조건 실행`됩니다.**
    + Optional 내부 객체가 not null이라서 orElse가 `결과값을 반환하지는 않더라도 실행된다는 점이 중요`합니다.
  + **orElseGet()은 `내부 객체가 null인 경우에만 실행`됩니다.**

---

+ **`.orElse()`**

```java
private static String wontRunThis() { 
    System.out.println("Won't run this"); 
    return "foo"; 
} 

public void optional1() { 
    String o = Optional.of("Hello World!").orElse(wontRunThis()); 
    System.out.println("Result : " + o); 
}


//Result
// Won't run this 
// Result : Hello World!
```

---

+ **`.orElseGet()`**

```java
public void optional2() { 
    String o = Optional.of("Hello World!").orElseGet(() -> wontRunThis()); 
    System.out.println("Result : " + o); 
}


//Result
// Result : Hello World!
```

---

+ **결과에서 확인할 수 있듯이 두 메서드 모두 내부 객체가 null이 아니기 때문에 foo는 반환하지 않고 Hello World!를 반환합니다.**
+ 하지만 orElse()는 Optional 내부 객체의 상태와 상관없이 무조건 실행되어 Won't run this를 출력합니다.
  + 그 이유는 orElse()의 인자로 T other를 받기 때문입니다. 
+ orElseGet()은 내부 객체가 null인 경우에만 실행됩니다.

---

## 2-3. .orElseThrow(Function)

+ `orElseThrow`는 아래와 같은 방식으로 사용합니다.

```java
//Optional<User> findById(Long id);

//example
UserDto ret = userService.findById(userId)
                           .map(UserDto::new)
                           .orElseThrow(() -> new NotFoundException(User.class, userId));
```

```java
public class StTest {
    public static String get() throws Exception {
        Optional<String> opt = Optional.of("hello");
        return opt.orElseThrow(() -> new Exception("throw exception"));
    }

    public static void main(String[] args) throws Exception {
        String hello = get();
    }
}
```

---

## 3. 예시 코드

---

## 3-1. Optional로 내보낼 때 

```java
//JdbcUserRepository.java

@Repository
public class JdbcUserRepository implements UserRepository {

  private final JdbcTemplate jdbcTemplate;

  //...

  @Override
  public Optional<User> findById(Id<User, Long> userId) {
    List<User> results = jdbcTemplate.query(
      "SELECT * FROM users WHERE seq=?",
      mapper,
      userId.value()
    );
    return ofNullable(results.isEmpty() ? null : results.get(0));
  }

  @Override
  public Optional<User> findByEmail(Email email) {
    List<User> results = jdbcTemplate.query(
      "SELECT * FROM users WHERE email=?",
      mapper,
      email.getAddress()
    );
    return ofNullable(results.isEmpty() ? null : results.get(0));
  }

}
```

---

```java
//UserRepositoryImpl.java

@Repository
public class UserRepositoryImpl implements UserRepository {

    private final JdbcTemplate jdbcTemplate;

    //...

    @Override
    public Optional<User> findById(Long id) throws DataAccessException, DateTimeParseException {
        List<User> results = jdbcTemplate.query("select * from Users where seq = ?", userMapper, id);
        return Optional.ofNullable(results.isEmpty() ? null : results.get(0));
    }
}
```

---

## 3-2. Optional 래핑해제 할 때

```java
//UserRestController.java

@RestController
@RequestMapping("api")
public class UserRestController {

  //...

  @GetMapping(path = "user/me")
  @ApiOperation(value = "내 정보")
  public ApiResult<UserDto> me(@AuthenticationPrincipal JwtAuthentication authentication) {
    return OK(
      userService.findById(authentication.id)
        .map(UserDto::new)
        .orElseThrow(() -> new NotFoundException(User.class, authentication.id))
    );
  }

  @GetMapping(path = "user/connections")
  @ApiOperation(value = "내 친구 목록")
  public ApiResult<List<ConnectedUserDto>> connections(@AuthenticationPrincipal JwtAuthentication authentication) {
    return OK(
      userService.findAllConnectedUser(authentication.id).stream()
        .map(ConnectedUserDto::new)
        .collect(toList())
    );
  }
}
```

---

```java
//UsersApiController.java

@RestController
@RequestMapping("/api/users")
public class UsersApiController {

    //...

    @GetMapping(value = "/{id}")
    public ApiResult<UsersResponseDto> findById(@PathVariable("id") Long userId) {

        try {
            Optional<User> findUser = usersService.findById(userId);

            return findUser.map(user -> ApiResult.OK( new UsersResponseDto(user)))
                    .orElse((ApiResult<UsersResponseDto>)ApiResult
                            .ERROR(
                                    messageSource.getMessage("not_existed_user", null, Locale.getDefault())
                                    , HttpStatus.NOT_FOUND
                            )
                    );

        } catch (DataAccessException e) {
            return (ApiResult<UsersResponseDto>)ApiResult.ERROR(e, HttpStatus.INTERNAL_SERVER_ERROR);
        } catch (IllegalArgumentException e) {
            return (ApiResult<UsersResponseDto>)ApiResult.ERROR(e, HttpStatus.BAD_REQUEST);
        } catch (DateTimeParseException e) {
            return (ApiResult<UsersResponseDto>)ApiResult.ERROR(e, HttpStatus.INTERNAL_SERVER_ERROR);
        } catch (RuntimeException e) {
            return (ApiResult<UsersResponseDto>)ApiResult.ERROR(e, HttpStatus.INTERNAL_SERVER_ERROR);
        }
    }
}
```

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
    + [Optional .orElseThrow(Function) 사용법](https://krksap.tistory.com/1515)
    + [[Java 8] Optional.orElse() vs Optional.orElseGet()](https://itstory.tk/entry/Java-8-OptionalorElse-vs-OptionalorElseGet)
