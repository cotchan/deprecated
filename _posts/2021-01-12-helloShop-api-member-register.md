---
title: HelloShop) [웹 계층] (API 방식) 1.회원 등록 API 
author: cotchan 
date: 2021-01-12 22:20:21 +0800 
categories: [Project, HelloShop]
tags: # [] 
---

## 1. API 패키지 / 컨트롤러 생성

+ API 로직을 처리하기 위한 패키지를 별도로 생성합니다.
  + 그 이유는 API 컨트롤러는 렌더링을 위한 컨트롤러와 로직이 다르기 때문입니다.
    + ex.예외 처리 등
      + 렌더링 방식 -> 404 페이지 반환
      + API 방식 -> Error Response

---

## 2. API 요청 스펙에 맞추어 별도의 DTO를 만듭니다.

+ **엔티티를 외부에 노출해서는 안됩니다.**
+ **API를 만들 때는 엔티티를 Parameter로 받으면 안됩니다.**

+ **`Good Case`**

```java
//Good Case
package jpabook.jpashop.api;

//@Controller + @ResponseBody
@RestController
@RequiredArgsConstructor
public class MemberApiController {

    private final MemberService memberService;
    
    @PostMapping("/api/v2/members")
    public CreateMemberResponse saveMemberV2(@RequestBody @Valid CreateMemberRequest request) {

        Member member = new Member();
        member.setName(request.getName());

        Long id = memberService.join(member);
        return new CreateMemberResponse(id);
    }

    @Data
    static class CreateMemberRequest {
        @NotEmpty
        private String name;
    }

    @Data
    static class CreateMemberResponse {
        private Long id;

        public CreateMemberResponse(Long id) {
            this.id = id;
        }
    }
}
``` 

---

+ `Bad Case`

```java
//BadCase
package jpabook.jpashop.api;

//@Controller + @ResponseBody
@RestController
@RequiredArgsConstructor
public class MemberApiController {

    private final MemberService memberService;

    @PostMapping("/api/v1/members")
    public CreateMemberResponse saveMemberV1(@RequestBody @Valid Member member) {
        Long id = memberService.join(member);
        return new CreateMemberResponse(id);
    }

    @Data
    static class CreateMemberRequest {
        @NotEmpty
        private String name;
    }

    @Data
    static class CreateMemberResponse {
        private Long id;

        public CreateMemberResponse(Long id) {
            this.id = id;
        }
    }
}
```

---

## 2. 엔티티를 그대로 사용하는 경우의 문제점

+ **`엔티티가 바뀌면` `API의 스펙 자체가 바뀌는 것`이 가장 큰 문제점입니다.**
  + 엔티티는 여러 곳에서 쓰기 때문에 바뀔 확률이 굉장히 높습니다.

+ Presentation Layer를 위한 검증 로직이 Entity에 들어가있게 됩니다.
+ 엔티티에 API 검증을 위한 로직이 들어갑니다.
  + `@NotEmpty` 등등

---

## 3. 별도의 DTO를 사용하는 방식의 장점

+ **Entity의 변화에도 `API의 스펙이 변하지 않습니다.`**
+ 엔티티를 parameter로 그대로 사용하는 경우 parameter로 어느 필드값이 넘어올지 모릅니다.
  + 하지만 API 스펙을 위한 DTO는 명확합니다.      

```java
/**
 * DTO를 보면 API 스펙 자체를 알 수 있습니다.
 * 그래서 외부 API 스펙랑 매핑되는 별도의 DTO를 만드는 게 API를 만드는 정석입니다.
 * Entity를 외부에 노출하지 말기
 */
@Data
static class CreateMemberRequest {
    @NotEmpty
    private String name;
}
```

+ 엔티티와 프레젠테이션 계층을 위한 로직을 분리할 수 있습니다.
+ **엔티티와 API 스펙을 명확하게 분리할 수 있습니다.**

+ **그러므로 API 스펙을 위한 별도의 DTO를 만들어야 합니다.**
  + **DTO를 parameter로 받아서 사용하기**


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용2 - API 개발과 성능 최적화](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-API%EA%B0%9C%EB%B0%9C-%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94/dashboard)
