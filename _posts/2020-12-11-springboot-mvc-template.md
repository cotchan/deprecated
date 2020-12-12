---
title: Spring-Boot) 웹 개발방식2. MVC와 template 사용 방법  
author: cotchan 
date: 2020-12-11 01:00:21 +0800 
categories: [Spring-Boot, Spring-Boot_Basic]
tags: [spring-boot] 
---

## 1. Controller (example)

```java
@Controller
public class HelloController {
	@GetMapping("hello-mvc")
	public String helloMvc(@RequestParam("name") String name, Model model) {
		model.addAttribute("name", name);
		return "hello-template";
	}
}
```


---


## 2. View (example)

+ 리소스 위치: `resources/template/hello-template.html`

```html
<html xmlns:th="http://www.thymeleaf.org">
<body>
<p th:text="'hello ' + ${name}">hello! empty</p>
</body>
</html>
```


---


## 3. 동작 원리

+ 요청 URL : `http://localhost:8080/hello-mvc?name=spring!`
 
![Desktop View](/assets/img/post/spring-boot/2020-12-11-mvc-template.png)

1. 웹 브라우저에서 URL 요청 
2. 내장 톰캣 서버에서 브라우저의 URL 요청을 Spring-Boot에게 전달 
3. Spring-Boot는 요청이 `HelloController의 메소드에 Mapping`이 되어있는 것을 확인 후 `메소드 호출`
4. Controller는 `string value`를 return 합니다. 
5. Controller는 또한 model에 key-value 쌍을 넣어줍니다.
6. 스프링은 4,5번 data를 `view resolver`에게 전달해줍니다.
	+ view resolver는 뷰를 찾아주고, 템플릿 엔진을 연결시켜주는 역할
7. view resolver는`resources/template 이하 경로에` `Controller의 return String Name과 똑같은 파일`을 찾아서 템플릿 엔진에게 처리하라고 넘깁니다. 
8. 그러면 템플릿 엔진이 렌더링을 해서 `변환한 html`을 웹 브라우저에 반환합니다.



---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
