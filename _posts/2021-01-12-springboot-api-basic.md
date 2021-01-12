---
title: Spring-Boot) API 기본  
author: cotchan 
date: 2021-01-12 19:35:21 +0800 
categories: [Spring-Boot, Spring-Boot_API]
tags: [spring-boot] 
---

## 1. API Controller 샘플 코드

```java
package jpabook.jpashop.api;

import jpabook.jpashop.domain.Member;
import jpabook.jpashop.service.MemberService;
import lombok.AllArgsConstructor;
import lombok.Data;
import lombok.RequiredArgsConstructor;
import org.springframework.web.bind.annotation.*;

import javax.validation.Valid;
import javax.validation.constraints.NotEmpty;

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

    @PutMapping("/api/v2/members/{id}")
    public UpdateMemberResponse updateMemberV2(
            @PathVariable("id") Long id,
            @RequestBody @Valid UpdateMemberRequest request) {

        memberService.update(id, request.getName());
        Member findMember = memberService.findOne(id);
        return new UpdateMemberResponse(findMember.getId(), findMember.getName());
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

    @Data
    static class UpdateMemberRequest {
        @NotEmpty
        private String name;
    }

    @Data
    @AllArgsConstructor
    static class UpdateMemberResponse {
        private Long id;
        private String name;
    }
}
```

---


## 2. API 컨트롤러 구성 요소

+ API Controller를 만들려면 아래의 요소들이 필요합니다.

---

## 2-1. @RestController

+ @RestController는 까보면 `@Controller` + `@ResponseBody`로 구성되어 있습니다.
+ 데이터 자체를 바로 json이나 XML로 보낼 때 사용합니다. 

---

## 2-2. @RequestBody

+ **@RequestBody는 json으로 넘어 온 `RequestBody`를 Member에 `바인딩`해서 전부 넣어줍니다.**

```java
@PostMapping("/api/v1/members")
public CreateMemberResponse saveMemberV1(@RequestBody @Valid Member member) {
    Long id = memberService.join(member);
    return new CreateMemberResponse(id);
}
```

---

## 2-3. @PostMapping, @PutMapping 등

+ HTTP 메소드로 요청에 대한 `알맞은 응답`을 보내기 위해 사용합니다.

+ Sampele Code
  + @PostMapping(`"/api/v2/members"`)
  + @PutMapping(`"/api/v2/members/{id}"`)

---

## 2-4. @PathVariable
 
```java
@PutMapping("/api/v2/members/{id}")
public UpdateMemberResponse updateMemberV2(
        @PathVariable("id") Long id,
        @RequestBody @Valid UpdateMemberRequest request) {

    memberService.update(id, request.getName());
    Member findMember = memberService.findOne(id);
    return new UpdateMemberResponse(findMember.getId(), findMember.getName());
}
```

---

## 2-5. API 용 DTO(@Data)

+ **`@Data`** 
  + `@Data`에 포함되어 있는 Lombok은 다음과 같습니다.
    ```java
    @ToString
    @EqualsAndHashCode
    @Getter //모든 필드  
    @Setter //정적 필드가 아닌 모든 필드  
    @RequiredArgsConstructor
    ```
+ Sample Code

```java
package jpabook.jpashop.api;

//@Controller + @ResponseBody
@RestController
@RequiredArgsConstructor
public class MemberApiController {

    private final MemberService memberService;

    //Method...

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

    /**
     * 응답값
     */
    @Data
    static class CreateMemberResponse {
        private Long id;

        public CreateMemberResponse(Long id) {
            this.id = id;
        }
    }

    @Data
    static class UpdateMemberRequest {
        @NotEmpty
        private String name;
    }

    @Data
    @AllArgsConstructor
    static class UpdateMemberResponse {
        private Long id;
        private String name;
    }
}
```

---

+ 출처
    + [실전! 스프링 부트와 JPA 활용2 - API 개발과 성능 최적화](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-API%EA%B0%9C%EB%B0%9C-%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94/dashboard)
    + [[Java] Lombok @Data 어노테이션](https://hilucky.tistory.com/238)
