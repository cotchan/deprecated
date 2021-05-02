---
title: Java) Optional 사용 방법
author: cotchan 
date: 2021-05-02 20:53:21 +0800 
categories: [Java]
tags: [java] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

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

## 2-2. .orElse

---

## 2-3. .orElseThrow

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
