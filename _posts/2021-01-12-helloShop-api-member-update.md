---
title: HelloShop) [웹 계층] (API 방식) 2.회원 수정 API
author: cotchan
date: 2021-01-12 22:25:21 +0800
categories: [Project, HelloShop]
tags: # []
---

## 1. 회원 수정 메서드 추가 

+ **회원 수정도 `DTO`를 요청 파라미터에 매핑하는 게 좋습니다.**

---

## 1-1. Presentation Layer

```java
package jpabook.jpashop.api;

//@Controller + @ResponseBody
@RestController
@RequiredArgsConstructor
public class MemberApiController {

    //데이터 자체를 바로 json이나 XML로 보낼 때 사용합니다.
    private final MemberService memberService;

    //기존 메서드...

    @PutMapping("/api/v2/members/{id}")
    public UpdateMemberResponse updateMemberV2(
            @PathVariable("id") Long id,
            @RequestBody @Valid UpdateMemberRequest request) {

        memberService.update(id, request.getName());
        Member findMember = memberService.findOne(id);
        return new UpdateMemberResponse(findMember.getId(), findMember.getName());
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

## 1-2. Service Layer

+ **`변경 감지를 이용`해서 데이터를 수정합니다.**

```java
@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor    //final이 있는 필드만 가지고 생성자를 만들어줍니다.
public class MemberService {

    private final MemberRepository memberRepository;

    /**
     * 회원 수정
     */
    @Transactional
    public void update(Long id, String name) {
        Member member = memberRepository.findOne(id);
        member.setName(name);
    }
}
```

---

+ 출처
    + [실전! 스프링 부트와 JPA 활용2 - API 개발과 성능 최적화](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-API%EA%B0%9C%EB%B0%9C-%EC%84%B1%EB%8A%A5%EC%B5%9C%EC%A0%81%ED%99%94/dashboard)
