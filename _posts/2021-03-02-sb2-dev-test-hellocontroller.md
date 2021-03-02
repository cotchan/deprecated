---
title: sb2) 3. helloController 테스트 코드 작성  
author: cotchan 
date: 2021-03-02 00:40:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. web 패키지 생성

+ **`web` 패키지에 컨트롤러와 관련된 클래스들을 모두 담습니다.** 


---

## 2. HelloController 클래스 작성

```java
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class HelloController {

    @GetMapping("/hello")
    public String hello() {
        return "hello";
    }
}
```

---

## 3. HelloControllerTest 클래스 작성

+ 작성한 코드가 제대로 동작하는지 테스트하겠습니다.
+ WAS를 사용하지 않고 테스트 코드로 검증합니다.

+ `src/test/java` 디렉토리에 앞에서 생성한 패키지를 그대로 다시 생성해줍니다.
+ 인테리제이 단축키: `cmd + shfit + T`로도 생성가능합니다. 


```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.autoconfigure.web.servlet.WebMvcTest;
import org.springframework.test.context.junit4.SpringRunner;
import org.springframework.test.web.servlet.MockMvc;

import static org.springframework.test.web.servlet.request.MockMvcRequestBuilders.get;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.content;
import static org.springframework.test.web.servlet.result.MockMvcResultMatchers.status;


@RunWith(SpringRunner.class)
@WebMvcTest(controllers = HelloController.class)
public class HelloControllerTest {

    @Autowired
    private MockMvc mvc;

    @Test
    public void hello가_리턴된다() throws Exception {
        String hello = "hello";

        mvc.perform(get("/hello"))
                .andExpect(status().isOk())
                .andExpect(content().string(hello));
    }
}
```


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
