---
title: Spring-Boot) 웹 개발방식2. API 방식 
author: cotchan 
date: 2020-12-12 16:00:21 +0800 
categories: [Spring-Boot, Spring-Boot_Basic]
tags: [spring-boot] 
---

## API 방식이란 

**`서버에게 데이터를 요청`할 때 `API방식을 사용`합니다.**

    
---


## @ResponseBody

**@ResponseBody의 의미는 http에서 Http Body에 데이터를 직접 넣어주겠다는 의미입니다.**    
@ResponseBody를 사용하면 뷰 리졸버(viewResolver)를 사용하지 않습니다.        


---


## API 방식 사용 방법


```java
@Controller
public class HelloController {

	//주석1
	//이렇게 사용하는 방식을 API 방식이라고 합니다.
	@GetMapping("hello-api")
	@ResponseBody
	public Hello helloApi(@RequestParam("name") String name) {
		Hello hello = new Hello(); 
		hello.setName(name); 
		return hello;	//문자가 아닌 객체를 리턴
	}
	//주석1
      
	static class Hello {
		private String name;
		public String getName() {
			return name;
		}
		public void setName(String name) { 
			this.name = name;
		} 
	}
}
```

위와 같이 컨트롤러를 작성하고 브라우저에 `localhost:8080/hello-api?name=spring`를 쳐보면,    

```java
{ "name": "spring" } 
```

위와 같이 `Json` 형태로 데이터를 서버에서 내려주는 것을 확인할 수 있습니다.    

    
---


## API 방식의 동작방식

+ @ResponseBody를 사용하는 API 방식의 동작방식

![Desktop View](/assets/img/post/spring-boot/2020-12-12-api_logic.png)


1. 웹브라우저에서 먼저 `localhost:8080/hello-api`라고 요청을 보냅니다.
2. 그러면 톰캣 내장 서버에서 브라우저로부터 받은 요청을 스프링에게 던집니다.
3. 스프링은 컨트롤러에 `hello-api`라는 요청을 처리하는 메소드가 있는 것을 확인을 합니다.
4. **그런데 이 메소드에는 `@ResponseBody` 어노테이션이 있습니다.**
	+ 이런게 없으면 스프링은 이전의 MVC & template 방식처럼 `viewResolver`한테 요청을 던집니다. "나한테 맞는 템플릿을 찾아서 돌려줘"
5. @ResponseBody가 있으면 `HttpMessageConverter`가 동작을 하고 `"HTTP 응답에 그냥 이 데이터를 넘겨야겠다"`고 처리합니다.
6. 그런데 요청을 처리하는 컨트롤러의 `method의 return type`이 `객체`인 경우 
	+ 기본 정책(default): 그냥 `Json` 방식으로 데이터를 만들어서 HTTP 응답에 반환하겠다.
7. Return type 객체를 보고 HttpMessageConverter가 동작하는 경우
	+ 단순 문자열이면 StringConverter가 동작, 객체라면 JSonConverter가 기본으로 동작합니다.    
8. Json으로 데이터를 파싱 후 요청을 한 웹브라우저나 서버에게 HTTP Response로 응답을 보내줍니다.
 






---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
