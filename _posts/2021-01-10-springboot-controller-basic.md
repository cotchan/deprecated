---
title: Spring-Boot) Controller 클래스 생성방법(1)
author: cotchan 
date: 2021-01-05 00:00:21 +0800 
categories: [Spring-Boot, Spring-Boot_Controller]
tags: [spring-boot] 
---

## 1. Controller 클래스 생성에 필요한 것

+ **컨트롤러 클래스를 생성할 때 아래의 어노테이션이 필요합니다.**

```java
@Controller
@RequiredArgsConstructor
public class ControllerClass {

    private final ServiceClass serviceClass;
```

---

## 2. 적용된 기술 설명


+ **`@Controller`**
  + ComponentScan의 대상이 되어 빈으로 등록됩니다. @Component 어노테이션을 래핑  
+ **`@RequiredArgsConstructor`**
  + `final` 키워드가 선언 되어 있는 필드만 가지고 생성자를 만들어줍니다.
  + 생성자 주입방식을 사용하기 위해 이렇게 사용합니다.
  + 스프링은 생성자가 1개인 경우 별도의 어노테이션이 없어도 자동으로 @Autowired 옵션을 적용해줍니다.

---

## 3. Sample Code

+ Sample1: `HomeController`

```java
package jpabook.jpashop.controller;

import lombok.extern.slf4j.Slf4j;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;

@Controller
@Slf4j
public class HomeController {

    @RequestMapping("/")
    public String home() {
        log.info("home controller");
        return "home";
    }
}

```

+ Sample2: `MemberController`

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

@Controller
@RequiredArgsConstructor
public class MemberController {

    private final MemberService memberService;

    /**
     * Controller에서 이 메서드를 타고 *.html 파일이 열리는 방식입니다.
     * Model이라는 것은 컨트롤러에서 뷰로 데이터를 실어서 넘겨서 렌더링이 될 때
     * model에 attribute를 넘기면, 화면에서는 이 model 객체에 접근할 수 있게 됩니다.
     */
    @GetMapping("/members/new")
    public String createForm(Model model) {
        model.addAttribute("memberForm", new MemberForm());
        return "members/createMemberForm";
    }

    //@Valid 어노테이션을 쓰면 Validation check를 해줍니다.
    /**
     * BindingResult의 역할
     * 원래는 오류가 있으면 Controller의 메서드가 실행되지 않고 팅겨버립니다.
     * 그러나 BindingResult가 있으면, 오류가 BindingResult에 담겨서 코드가 실행이 됩니다.
     */
    @PostMapping("/members/new")
    public String create(@Valid MemberForm form, BindingResult result) {

        if (result.hasErrors())
        {
            return "members/createMemberForm";
        }

        Address address = new Address(form.getCity(), form.getStreet(), form.getZipcode());

        Member member = new Member();
        member.setName(form.getName());
        member.setAddress(address);

        memberService.join(member);
        return "redirect:/";    // home 화면으로 redirect
    }
}
```


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
