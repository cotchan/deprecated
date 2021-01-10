---
title: HelloShop) [웹 계층] 3.상품 등록
author: cotchan
date: 2021-01-11 08:30:21 +0800
categories: [Project, HelloShop]
tags: # [TAG]     # TAG names should always be lowercase
---

## 1. 폼 객체를 사용해서 화면 계층과 서비스 계층 분리

---

## 1-1. 상품 등록 폼 객체

+ 설명을 위해 `Book` 엔티티의 품 객체만 만들었습니다.

```java
package jpabook.jpashop.controller;

@Getter @Setter
public class BookForm {

    //==상품의 공통 속성==//
    /**
     * 상품 수정을 위해 id값을 받습니다.
     */
    private Long id;

    private String name;
    private int price;
    private int stockQuantity;
    //==상품의 공통 속성==//

    //==책과 관련된 속성==//
    private String author;
    private String isbn;
    //==책과 관련된 속성==//
}
```

---


## 1-2. 상품 등록 컨트롤러

+ **상품 등록**
  + 상품 등록 폼에서 데이터를 입력하고 Submit 버튼을 누르면 `/items/new`를 POST 방식으로 요청합니다.
  + 상품 저장이 끝나면 상품 목록 화면(`redirect:/items`)으로 리다이렉트합니다.

```java
package jpabook.jpashop.controller;

import jpabook.jpashop.domain.item.Book;
import jpabook.jpashop.domain.item.Item;
import jpabook.jpashop.service.ItemService;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.ModelAttribute;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;

import java.util.List;

@Controller
@RequiredArgsConstructor
public class ItemController {

    private final ItemService itemService;

    @GetMapping("/items/new")
    public String createForm(Model model) {
        model.addAttribute("form", new Book());
        return "items/createItemForm";
    }

    /**
     * 더 나은 설계는 Book.createBook() 같은 static한 메서드를 사용해서 생성하는 게 더 바람직합니다.
     * 이런 방식으로 setter를 제거하는 것이 좀 더 바람직
     */
    @PostMapping("/items/new")
    public String create(BookForm bookForm) {

        Book book = new Book();
        book.setName(bookForm.getName());
        book.setPrice(bookForm.getPrice());
        book.setStockQuantity(bookForm.getStockQuantity());
        book.setAuthor(bookForm.getAuthor());
        book.setIsbn(bookForm.getIsbn());

        itemService.saveItem(book);
//        return "redirect:/items";   //저장된 책 목록으로 redirect
        return "redirect:/";
    }
}
```

---

## 2. 상품 등록 폼 화면

+ 상품 등록 폼 화면(`templates/items/createItemForm.html`)

```html
<!DOCTYPE HTML>
<html xmlns:th="http://www.thymeleaf.org">
<head th:replace="fragments/header :: header" />
<body>

<div class="container">
    <div th:replace="fragments/bodyHeader :: bodyHeader"/>

    <form th:action="@{/items/new}" th:object="${form}" method="post">
        <div class="form-group">
            <label th:for="name">상품명</label>
            <input type="text" th:field="*{name}" class="form-control" placeholder="이름을 입력하세요">
        </div>
        <div class="form-group">
            <label th:for="price">가격</label>
            <input type="number" th:field="*{price}" class="form-control" placeholder="가격을 입력하세요">
        </div>
        <div class="form-group">
            <label th:for="stockQuantity">수량</label>
            <input type="number" th:field="*{stockQuantity}" class="form-control" placeholder="수량을 입력하세요">
        </div>
        <div class="form-group">
            <label th:for="author">저자</label>
            <input type="text" th:field="*{author}" class="form-control" placeholder="저자를 입력하세요">
        </div>
        <div class="form-group">
            <label th:for="isbn">ISBN</label>
            <input type="text" th:field="*{isbn}" class="form-control" placeholder="ISBN을 입력하세요">
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


