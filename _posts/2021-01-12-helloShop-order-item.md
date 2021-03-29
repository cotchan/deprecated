---
title: HelloShop) [웹 계층] 6.상품 주문
author: cotchan
date: 2021-01-12 08:39:21 +0800
categories: [Project, HelloShop]
tags: [helloshop]     # TAG names should always be lowercase
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 상품 주문 컨트롤러 추가

+ **주문 폼 이동**
  + 메인 화면에서 상품 주문을 선택하면 `/order`를 GET 방식으로 호출
  + `OrderController`의 `createForm()` 메서드
    + 주문 화면에는 주문할 고객정보와 상품 정보가 필요하므로 `model` 객체에 담아서 뷰에 넘겨줍니다.

+ **주문 실행**
  + 주문할 회원과 상품 그리고 수량을 선택해서 Submit 버튼을 누르면 `/order` URL을 POST 방식으로 호출
  + 컨트롤러의 `order()` 메서드 실행
    + 이 메서드는 고객 식별자(`memberId`), 주문할 상품 식별자(`itemId`), 수량(`count`) 정보를 받아서 주문 서비스에 주문을 요청합니다.
    + 주문이 끝나면 상품 주문 내역이 있는 `/orders` URL로 리다이렉트

```java
package jpabook.jpashop.controller;

import jpabook.jpashop.domain.Member;
import jpabook.jpashop.domain.Order;
import jpabook.jpashop.domain.item.Item;
import jpabook.jpashop.repository.OrderSearch;
import jpabook.jpashop.service.ItemService;
import jpabook.jpashop.service.MemberService;
import jpabook.jpashop.service.OrderSerivce;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@Controller
@RequiredArgsConstructor
public class OrderController {

    //고객이랑 아이템이랑 전부 선택할 수 있어야 합니다.

    final OrderSerivce orderSerivce;
    final MemberService memberService;
    final ItemService itemService;

    @GetMapping("/order")
    public String createFrom(Model model) {

        List<Member> members = memberService.findMembers();
        List<Item> items = itemService.findItems();

        model.addAttribute("members", members);
        model.addAttribute("items", items);

        return "order/orderForm";
    }

    /**
     * @RequestParam은 form-submit 방식으로 오면, 선택된 id의 value가 오게 됩니다.
     * <select name="memberId">
     * <select name="itemId">
     * <select name="count">
     */
    @PostMapping("/order")
    public String order(@RequestParam("memberId") Long memberId,
                        @RequestParam("itemId") Long itemId,
                        @RequestParam("count") int count) {

        //조회가 아닌 핵심 비즈니스로직의 경우에는
        //컨트롤러에서는 식별자만 넘기고 (바깥에서 Entity를 찾아서 넘기기보다는)
        //Service 계층에서 Entity를 찾는 것부터 시작해서 시작합니다. (영속 상태로 로직 시작이 가능합니다.)
        orderSerivce.order(memberId, itemId, count);

        //주문목록 리스트로 redirect
        return "redirect:/orders";
    }
}
```

---

## 2. 상품 주문 폼 화면(View)
  

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head th:replace="fragments/header :: header" />

<body>
<div class="container">
    <div th:replace="fragments/bodyHeader :: bodyHeader"/>

    <form role="form" action="/order" method="post">

        <div class="form-group">
            <label for="member">주문회원</label>
            <select name="memberId" id="member" class="form-control">
                <option value="">회원선택</option>
                <option th:each="member : ${members}"
                        th:value="${member.id}"
                        th:text="${member.name}" />
            </select>
        </div>

        <div class="form-group">
            <label for="item">상품명</label>
            <select name="itemId" id="item" class="form-control">
                <option value="">상품선택</option>
                <option th:each="item : ${items}"
                        th:value="${item.id}"
                        th:text="${item.name}" />
            </select>
        </div>

        <div class="form-group">
            <label for="count">주문수량</label>
            <input type="number" name="count" class="form-control" id="count"
                   placeholder="주문 수량을 입력하세요">
        </div>
        <button type="submit" class="btn btn-primary">Submit</button>
    </form>
    <br/>
    <div th:replace="fragments/footer :: footer" />

</div> <!-- /container -->

</body>
</html>
```

---


+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)


