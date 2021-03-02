---
title: sb2) 주요 @어노테이션@
author: cotchan 
date: 2021-03-03 00:25:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Main]
tags: [dev_annotation] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **계속 업데이트할 예정입니다.**

---

## @Autowired

+ 스프링이 관리하는 빈(Bean)을 주입받습니다.

---

## @GetMapping

+ HTTP Method `Get` 요청을 받을 수 있는 API를 만들어 줍니다.
+ 예전: `@RequestMapping(method = RequestMethod.GET)`

---

## @Getter

+ 선언된 모든 필드의 get 메소드를 생성해 줍니다.

---

## @RequestParam

+ **외부에서 API로 넘긴 파라미터를 가져오는 어노테이션입니다.**
+ 여기서는 외부에서 name (@RequestParam("name")) 이란 이름으로 넘긴 파라미터를 메소드 파라미터 name(String name)에 저장하게 됩니다.

```java
@RestController
public class HelloController {

    @GetMapping("/hello/dto")
    public HelloResponseDto helloDto(@RequestParam("name") String name,
                                     @RequestParam("amount") int amount) {
        return new HelloResponseDto(name, amount);
    }
}
```

---

## @RequiredArgsConstructor

+ 선언된 모든 `final` 필드가 포함된 생성자를 생성해 줍니다.
+ **final이 없는 필드는 생성자에 포함되지 않습니다.**

---

## @RestController

+ **JSON을 반환하는 컨트롤러를 만들어줍니다.**
+ 예전에는 `@ResponseBody`를 각 메소드마다 선언했던 것을 한 번에 사용할 수 있게 해준다고 보면 됩니다.

---

## @SpringBootApplication

+ 이 어노테이션을 사용하면
+ **스프링 부트의 자동 설정, 스프링 Bean 읽기와 생성이 모두 자동으로 설정됩니다.**
+ **또한 이 어노테이션이 있는 위치부터 설정을 읽어나갑니다.**
  + 그러므로 `항상 프로젝트 최상단에 위치`해야만 합니다.

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
