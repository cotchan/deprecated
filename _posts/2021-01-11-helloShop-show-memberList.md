---
title: HelloShop) [웹 계층] 2.회원 목록 조회
author: cotchan
date: 2021-01-11 08:22:21 +0800
categories: [Project, HelloShop]
tags: [helloshop]     # TAG names should always be lowercase
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 회원 목록 컨트롤러 추가

+ 조회한 회원을 뷰에 전달하기 위해 스프링 MVC가 제공하는 모델(`Model`)객체에 보관합니다.
+ 실행할 `뷰 이름을 반환`합니다.

```java
package jpabook.jpashop.controller;

import jpabook.jpashop.domain.Address;
import jpabook.jpashop.domain.Member;
import jpabook.jpashop.service.MemberService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.validation.BindingResult;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PostMapping;

import javax.validation.Valid;
import java.util.List;

@Controller
@RequiredArgsConstructor
public class MemberController {

    private final MemberService memberService;

    //...

    //추가
    @GetMapping("/members")
    public String list(Model model) {
        List<Member> members = memberService.findMembers();
        model.addAttribute("members", members);
        return "members/memberList";
    }
}
```

---

## 2. 회원 목록 뷰

+ 회원 목록 뷰(`templates/members/memberList.html`) 
+ 타임리프에서 `?`을 사용하면 `null`을 무시합니다.

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org"> <head th:replace="fragments/header :: header" /> <body>
<div class="container">
    <div th:replace="fragments/bodyHeader :: bodyHeader" />
    <div>
        <table class="table table-striped"> <thead>
        <tr>
            <th>#</th>
            <th>이름</th>
            <th>도시</th>
            <th>주소</th>
            <th>우편번호</th>
        </tr>
        </thead>
            <tbody>
            <tr th:each="member : ${members}">
                <td th:text="${member.id}"></td>
                <td th:text="${member.name}"></td>
                <td th:text="${member.address?.city}"></td>
                <td th:text="${member.address?.street}"></td>
                <td th:text="${member.address?.zipcode}"></td>
            </tr>
            </tbody>
        </table>
    </div>
    <div th:replace="fragments/footer :: footer" />
</div> <!-- /container -->
</body>
</html>
```

---


+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)


