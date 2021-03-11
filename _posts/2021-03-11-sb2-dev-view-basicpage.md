---
title: sb2) 13. [화면개발] mustache 기본 페이지 만들기
author: cotchan 
date: 2021-03-11 19:32:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 플러그인 설치

+ Plugins에서 `"mustache"` 검색 후 install
  + cmd + shift + A

---

## 2. build.gradle 의존성 등록

+ `compile('org.springframework.boot:spring-boot-starter-mustache')`
+ 머스테치는 스프링 부트에서 공식 지원하는 템플릿 엔진
+ 의존성 하나만 추가하면 다른 스타터 패키지와 마찬가지로 추가 설정 없이 설치가 끝입니다.

```
dependencies {
    compile('org.springframework.boot:spring-boot-starter-mustache')
}
```

---

## 3. 머스테치의 기본 파일 위치

+ **`src/main/resources/templates`**
  + 이 위치에 머스테치 파일을 두면 스프링 부트에서 자동으로 로딩합니다.

---

## 4. index.mustache 생성

+ src/main/resources/templates 경로에 `index.mustache` 파일을 생성합니다.

```html
<!DOCTYPE HTML>
<html>
<head>
    <title>스프링 부트 웹 서비스</title>
    <meta http-equiv="Content-Type" content="text/html"; charset="UTF-8" />
</head>

<body>
    <h1>스프링 부트로 시작하는 웹 서비스</h1>
</body>

</html>
```

---

## 5. IndexController 생성

+ `web` 패키지 안에 IndexController를 생성합니다.

```java
package com.cotchan.review.springboot.web;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.GetMapping;

@Controller
public class IndexController {

    @GetMapping("/")
    public String index() {
        return "index";
    }
}
```

---

## 6. IndexController Test

+ 이번 테스트는 실제로 URL 호출 시 페이지의 내용이 제대로 호출되는지에 대한 테스트
+ **`HTML`도 결국은 규칙이 있는 문자열** 
+ 그러므로 TestRestTemplate을 통해 `"/"`를 호출했을 때 `index.mustache`에 포함된 코드들이 있는지 확인

```java
package com.cotchan.review.springboot.web;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.boot.test.web.client.TestRestTemplate;
import org.springframework.test.context.junit4.SpringRunner;

import static org.assertj.core.api.Assertions.assertThat;


@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class IndexControllerTest {

    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    public void 메인페이지_로딩() {

        //given

        //when
        String body = this.restTemplate.getForObject("/", String.class);

        //then
        assertThat(body).contains("스프링 부트로 시작하는 웹 서비스");
    }
}
```


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
