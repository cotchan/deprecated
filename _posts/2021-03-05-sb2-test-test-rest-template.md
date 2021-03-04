---
title: sb2) TestRestTemplate
author: cotchan 
date: 2021-03-05 02:12:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Test]
tags: [test_annotation] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 의미

+ `TestRestTemplate`을 사용하면 편리하게 `웹 통합 테스트`를 할 수 있습니다.
+ TestRestTemplate은 이름에서 알 수 있듯이 RestTemplate의 테스트를 위한 버전입니다.
+ `@SpringBootTest`에서 Web Environment 설정을 하였다면 `TestRestTemplate`은 그에 맞춰서 **자동으로 설정되어 빈이 생성됩니다.**

---

## 2. 샘플 코드

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class RestApiTest {
    @Autowired
    private TestRestTemplate restTemplate;

    @Test
    public void test() {
        ResponseEntity<Article> response = restTemplate.getForEntity("/api/articles/1", Article.class);
        assertThat(response.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(response.getBody()).isNotNull();
        ...
    }
}
```


---

+ 출처
  + [Spring Boot에서 테스트를 - 1](https://hyper-cube.io/2017/08/06/spring-boot-test-1/)
