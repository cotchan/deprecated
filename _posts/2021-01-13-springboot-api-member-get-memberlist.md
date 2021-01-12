---
title: Spring-Boot) 3.회원 조회 API 
author: cotchan 
date: 2021-01-13 04:48:21 +0800 
categories: [Spring-Boot, Spring-Boot_API]
tags: [spring-boot] 
---

## 1. API 요청 스펙에 맞추어 별도의 DTO를 만듭니다.

+ **엔티티를 외부에 노출해서는 안됩니다.**
+ **엔티티를 DTO로 변환해서 반환합니다.**
  + 엔티티가 변해도 API 스펙이 변경되지 않습니다.

---

## 1-1. Good Case

+ **추가로 `Result` 클래스로 컬렉션을 감싸서 결과를 반환합니다.**
  + `껍데기 Object` 추가하기
  + 이 방식은 향후에 API 스펙 확장 시 필요한 필드를 추가할 수 있습니다.
  

```java
@RestController
@RequiredArgsConstructor
public class MemberApiController {

    private final MemberService memberService;
    
    /**
     * Entity를 Dto로 바꾸는 로직이 추가되었습니다.
     * 회원 조회할 때 Entity를 직접 반환하면 안 됩니다.
     * 항상 API spec에 맞는 DTO로 다 바꾸고 DTO를 노출시킵니다.
     */
    @GetMapping("/api/v2/members")
    public Result memberV2() {
        List<Member> findMembers = memberService.findMembers();

        //memberDTO로 바꿔서 넘겨줍니다.
        List<MemberDto> collect = findMembers.stream()
                .map(m -> new MemberDto(m.getName()))
                .collect(Collectors.toList());

        return new Result(collect.size(), collect);
    }

    /**
     * Object 타입으로 바꾸는 것 때문에 껍데기 Object를 한 번 씌워줍니다.
     * { }
     * 데이터 필드의 값은 list가 나가게 됩니다.
     */
    @Data
    @AllArgsConstructor
    static class Result<T> {
        private int count;
        private T data;
    }

    /**
     * 내가 노출할 것만 Dto 스펙에 노출시켜줍니다.
     */
    @Data
    @AllArgsConstructor
    static class MemberDto {
        private String name;
    }
}
``` 

---

+ API 요청 결과
  + GET `http://localhost:8080/api/v2/members`

```json
{
    "count": 3,
    "data": [
        {
            "name": "new-hello"
        },
        {
            "name": "member1"
        },
        {
            "name": "member2"
        }
    ]
}
```

---

## 1-2. Bad Case

```java
@RestController
@RequiredArgsConstructor
public class MemberApiController {

    private final MemberService memberService;

    /**
     * 엔티티를 그대로 노출하면 회원 정보에 불필요한 정보(ex. order)도 포함됩니다.
     * @JsonIgnore 옵션을 사용하면 그 필드를 제외할 수 있습니다.
     * array를 바로 반환하면 스펙이 굳어버립니다. (스펙 확장이 어렵습니다.)
     */
    @GetMapping("/api/v1/members")
    public List<Member> membersV1() {
        return memberService.findMembers();
    }
}
```

---

+ API 요청 결과
  + GET `http://localhost:8080/api/v1/members` 

```json
[
    {
        "id": 1,
        "name": "new-hello",
        "address": null,
        "orders": []
    },
    {
        "id": 2,
        "name": "member1",
        "address": {
            "city": "서울",
            "street": "삼청동",
            "zipcode": "139"
        },
        "orders": []
    },
    {
        "id": 3,
        "name": "member2",
        "address": {
            "city": "부산",
            "street": "스끼다시",
            "zipcode": "123"
        },
        "orders": []
    }
]
```


---

## 2. 엔티티를 그대로 사용하는 경우의 문제점

+ **`엔티티가 바뀌면` `API의 스펙 자체가 바뀌는 것`이 가장 큰 문제점입니다.**
  + 엔티티는 여러 곳에서 쓰기 때문에 바뀔 확률이 굉장히 높습니다.

1. 엔티티에 프레젠테이션 계층을 위한 로직이 추가됩니다.
2. 기본적으로 엔티티의 모든 값이 노출됩니다.
3. 응답 스펙을 맞추기 위해 로직이 추가됩니다. (`@JsonIgnore`, 별도의 뷰 로직 등등)

+ 실무에서는 같은 엔티티에 대해 API가 용도에 따라 다양하게 만들어지는데, 한 엔티티에 각각의 API를 위한 프레젠테이션 응답 로직을 담기는 어렵습니다.

+ **추가로 `컬렉션을 직접 반환`하면 향후 API 스펙을 변경하기가 어렵습니다.** 
  + 별도의 `Result` 클래스 생성으로 해결합니다.

---

## 3. 별도의 DTO를 사용하는 방식의 장점

+ **Entity의 변화에도 `API의 스펙이 변하지 않습니다.`**
+ 엔티티를 그대로 return하는 경우 노출되면 안 되는 필드 또한 API 결과로 노출됩니다.
  + 하지만 API 스펙을 위한 DTO는 명확합니다.      

```java
@RestController
@RequiredArgsConstructor
public class MemberApiController {

    private final MemberService memberService;

    @GetMapping("/api/v2/members")
    public Result memberV2() {
        //...
    }

    /**
     * Object 타입으로 바꾸는 것 때문에 껍데기 Object를 한 번 씌워줍니다.
     * { }
     * 데이터 필드의 값은 list로 나가게 됩니다.
     */
    @Data
    @AllArgsConstructor
    static class Result<T> {
        private int count;
        private T data;
    }

    /**
     * 내가 노출할 것만 Dto 스펙에 노출시켜줍니다.
     */
    @Data
    @AllArgsConstructor
    static class MemberDto {
        private String name;
    }
}
```

+ 엔티티와 프레젠테이션 계층을 위한 로직을 분리할 수 있습니다.
+ **엔티티와 API 스펙을 명확하게 분리할 수 있습니다.**

+ **그러므로 API 스펙을 위한 별도의 DTO를 만들어야 합니다.**
  + **DTO를 `return value`로 전달하기**

---

+ 출처
    + [실전! 스프링 부트와 JPA 활용2 - API 개발과 성능 최적화](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-API%EA%B0%9C%EB%B0%9C-%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94/dashboard)
