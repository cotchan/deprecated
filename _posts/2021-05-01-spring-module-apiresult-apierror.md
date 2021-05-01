---
title: sb3) ApiResult, ApiError (in Controller Layer)
author: cotchan 
date: 2021-05-01 20:42:21 +0800 
categories: [Spring-Boot3]
tags: [spring-module] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. ApiResult, ApiError ?

+ **`컨트롤러의 리턴값은 ApiResult`입니다.**
+ **이게 Api의 응답 포맷인데 `모든 응답은 ApiResult 스펙을 따릅니다.`**
+ **모든 응답형태를 받을 수 있도록 <T> 제네릭을 적용합니다.**

---

## 2. ApiResult

```java
import org.springframework.http.HttpStatus;

public class ApiResult<T> {

    private final boolean success;

    private final T response;

    private final ApiError error;

    private ApiResult(boolean success, T response, ApiError error) {
        this.success = success;
        this.response = response;
        this.error = error;
    }

    public static<T> ApiResult<T> OK(T response) {
        return new ApiResult<>(true, response, null);
    }

    public static ApiResult<?> ERROR(Throwable throwable, HttpStatus httpStatus) {
        return new ApiResult<>(false, null, new ApiError(throwable, httpStatus));
    }

    public static ApiResult<?> ERROR(String errorMessage, HttpStatus httpStatus) {
        return new ApiResult<>(false, null, new ApiError(errorMessage, httpStatus));
    }

    public boolean isSuccess() {
        return success;
    }

    public T getResponse() {
        return response;
    }

    public ApiError getError() {
        return error;
    }
}
```

---

## 3. ApiError

```java
import org.springframework.http.HttpStatus;

public class ApiError {

    private final String message;

    private final int status;

    ApiError(Throwable throwable, HttpStatus httpStatus) {
        this(throwable.getMessage(), httpStatus);
    }

    ApiError(String message, HttpStatus status) {
        this.message = message;
        this.status = status.value();
    }

    public String getMessage() {
        return message;
    }

    public int getStatus() {
        return status;
    }
}
```

---

## 4. 사용 예시

+ **`UsersApiController`에서 `ApiResult와 ApiError를 사용하여 응답`을 내려주는 예시코드입니다.** 

```java
//UsersApiController.java

@RestController
@RequestMapping("/api/users")
public class UsersApiController {

    private static final String SUCCESS_MESSAGE = "true";
    private static final String FAIL_MESSAGE = "fail";
    private static final String SUCCESS_RESPONSE = "가입완료";
    private static final String FAIL_RESPONSE = "가입실패";
    private static final String NOT_FOUND_RESPONSE = "해당하는 회원이 없습니다";

    private final UsersService usersService;

    @Autowired
    public UsersApiController(UsersService usersService) {
        this.usersService = usersService;
    }

    @GetMapping(value = "")
    public ApiResult<Result> findAll() {

        try {

            List<User> users = usersService.findAll();

            List<UsersResponseDto> usersResponseDto = users.stream()
                                                        .map(user -> new UsersResponseDto(user))
                                                        .collect(Collectors.toList());

            return ApiResult.OK( new Result(usersResponseDto.size(), usersResponseDto));

        } catch (DataAccessException e) {
            return (ApiResult<Result>)ApiResult.ERROR(e, HttpStatus.INTERNAL_SERVER_ERROR);
        } catch (DateTimeParseException e) {
            return (ApiResult<Result>)ApiResult.ERROR(e, HttpStatus.INTERNAL_SERVER_ERROR);
        } catch (RuntimeException e) {
            return (ApiResult<Result>)ApiResult.ERROR(e, HttpStatus.INTERNAL_SERVER_ERROR);
        }
    }

    @GetMapping(value = "/{id}")
    public ApiResult<UsersResponseDto> findById(@PathVariable("id") Long userId) {

        try {
            Optional<User> findUser = usersService.findById(userId);

            return findUser.map(user -> ApiResult.OK( new UsersResponseDto(user)))
                    .orElse((ApiResult<UsersResponseDto>)ApiResult.ERROR(NOT_FOUND_RESPONSE ,HttpStatus.NOT_FOUND));

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

    @PostMapping("/join")
    public ApiResult<UsersJoinResponseDto> join(@RequestBody UsersJoinRequestDto requestDto) {

        try {

            User joinedUser = usersService.join(new Email(requestDto.getPrincipal()), requestDto.getCredentials());

            if (joinedUser == null)
            {
                return (ApiResult<UsersJoinResponseDto>)ApiResult.ERROR(FAIL_RESPONSE, HttpStatus.BAD_REQUEST);
            }
            else
            {
                return ApiResult.OK( new UsersJoinResponseDto(SUCCESS_MESSAGE, SUCCESS_RESPONSE));
            }

        } catch (DataAccessException e) {
            return (ApiResult<UsersJoinResponseDto>)ApiResult.ERROR(e, HttpStatus.INTERNAL_SERVER_ERROR);
        } catch (IllegalArgumentException e) {
            return (ApiResult<UsersJoinResponseDto>)ApiResult.ERROR(e, HttpStatus.BAD_REQUEST);
        } catch (RuntimeException e) {
            return (ApiResult<UsersJoinResponseDto>)ApiResult.ERROR(e, HttpStatus.INTERNAL_SERVER_ERROR);
        }
    }

    static class Result<T> {
        private int count;
        private T data;

        public Result(int count, T data) {
            this.count = count;
            this.data = data;
        }

        public int getCount() {
            return count;
        }

        public T getData() {
            return data;
        }
    }
}

```

---

## 5. 메시지 (정상 처리 시)

<img width="473" alt="스크린샷 2021-05-01 오후 8 46 27" src="https://user-images.githubusercontent.com/75410527/116781871-bf02b300-aac0-11eb-9a5c-da0bbff6bda2.png">

---

<img width="634" alt="스크린샷 2021-05-01 오후 8 47 01" src="https://user-images.githubusercontent.com/75410527/116781873-c1650d00-aac0-11eb-8064-d213b1e284bb.png">


---

## 6. 메시지 (예외 발생 시)

<img width="514" alt="스크린샷 2021-05-01 오후 8 59 49" src="https://user-images.githubusercontent.com/75410527/116781876-c45ffd80-aac0-11eb-8cae-a79c06d53574.png">

---

<img width="699" alt="스크린샷 2021-05-01 오후 9 02 06" src="https://user-images.githubusercontent.com/75410527/116781877-c6c25780-aac0-11eb-82af-d8a4972147be.png">

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
