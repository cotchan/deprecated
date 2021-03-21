---
title: sb2) 22. [스프링 시큐리티] 구글 로그인 연동 - 3(로그인 테스트) 
author: cotchan 
date: 2021-03-20 21:00:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. index.mustache 수정

+ [수정된 index.mustache 코드](https://gist.github.com/cotchan/4ebba403a7cd00ce29151d3cfae32c04)

+ **<<#userName>>**
  + 머스테치는 다른 언어와 같은 if문을 제공하지 않습니다.
  + `true/false` 여부만 판단할 뿐입니다.
  + 그래서 머스테치에서는 항상 최종값을 넘겨줘야 합니다.
  + 여기서도 역시 userName이 있다면 userName을 노출시키도록 구성했습니다.

+ **`a href="/logout"`**
  + 스프링 시큐리티에서 기본적으로 제공하는 로그아웃 URL 입니다.
  + 즉, 개발자가 별도로 저 URL에 해당하는 컨트롤러를 만들 필요가 없습니다.
  + SecurityConfig 클래스에서 URL을 변경할 수는 있습니다. 여기서는 기본 URL을 사용합니다.

+ **`<<^userName>>`**
  + 머스테치에서 해당 값이 존재하지 않을 경우에는 `^`를 사용합니다.
  + 여기서는 userName이 없다면 로그인 버튼을 노출시키도록 구성했습니다.

+ **`a href="/oauth2/authorization/google"`**
  + 스프링 시큐리티에서 기본적으로 제공하는 로그인 URL입니다.
  + 로그아웃 URL과 마찬가지로 개발자가 별도의 컨트롤러를 생성할 필요가 없습니다.

---

## 2. IndexController 수정

+ `index.mustache`에서 userName을 사용할 수 있게 `IndexController`에서 userName을 Model에 저장하는 코드를 추가합니다.

```java
package com.cotchan.review.springboot.web;

import com.cotchan.review.springboot.config.auth.dto.SessionUser;
import com.cotchan.review.springboot.service.posts.PostsService;
import com.cotchan.review.springboot.web.dto.PostsResponseDto;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Controller;
import org.springframework.ui.Model;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;

import javax.servlet.http.HttpSession;

@RequiredArgsConstructor
@Controller
public class IndexController {

    private final PostsService postsService;
    private final HttpSession httpSession;

    @GetMapping("/posts/save")
    public String postsSave() {
        return "posts-save";
    }

    @GetMapping("/posts/update/{id}")
    public String postsUpdate(@PathVariable Long id, Model model) {
        PostsResponseDto dto = postsService.findById(id);
        model.addAttribute("post", dto);
        return "posts-update";
    }

    //...

    @GetMapping("/")
    public String index(Model model) {
        model.addAttribute("posts", postsService.findAllDesc());

        SessionUser user = (SessionUser) httpSession.getAttribute("user");

        if (user != null) {
            model.addAttribute("userName", user.getName());
        }

        return "index";
    }
}
```

---

+ **`(SessionUser)httpSession.getAttribute("user")`**
  + 앞서 작성된 `CustomOAuth2UserService`에서 로그인 성공 시 세션에 SessionUser를 저장하도록 구성했습니다.
  + 즉, 로그인 성공 시 `httpSession.getAttribute("user")`에서 값을 가져올 수 있습니다.

+ **`if (user != null)`**
  + 세션에 저장된 값이 있을 때만 model에 userName으로 등록합니다.
  + 세션에 저장된 값이 없으면 model엔 아무런 값이 없는 상태이니 로그인 버튼이 보이게 됩니다.

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
