---
title: sb2) @SpringBootTest에서 MockMvc를 사용하는 방법
author: cotchan 
date: 2021-03-22 04:23:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Test]
tags: [test_annotation] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. MockMvc 사용 샘플 코드

+ `PostsApiControllerTest.java`

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class PostsApiControllerTest {

    //...

    @Autowired
    private WebApplicationContext context;

    private MockMvc mvc;

    @Before //11111
    public void setup() {
        mvc = MockMvcBuilders.webAppContextSetup(context)
                .apply(springSecurity())
                .build();
    }

    //...

    @Test
    @WithMockUser(roles = "USER")
    public void Posts_등록된다() throws Exception {

        //...

        //when
        mvc.perform(post(url)   //22222
                .contentType(MediaType.APPLICATION_JSON_UTF8)
                .content(new ObjectMapper().writeValueAsString(requestDto)))
                .andExpect(status().isOk());

        //then
        List<Posts> all = postsRepository.findAll();
        assertThat(all.get(0).getTitle()).isEqualTo(title);
        assertThat(all.get(0).getContent()).isEqualTo(content);
    }

    //...

    @Test
    @WithMockUser(roles = "USER")
    public void Posts_수정된다() throws Exception {

        //...

        //when
        mvc.perform(put(url)
                .contentType(MediaType.APPLICATION_JSON_UTF8)
                .content(new ObjectMapper().writeValueAsString(requestDto)))
                .andExpect(status().isOk());

        //then
        List<Posts> all = postsRepository.findAll();
        assertThat(all.get(0).getTitle()).isEqualTo(expectedTitle);
        assertThat(all.get(0).getContent()).isEqualTo(expectedContent);
    }
}
```

---

## 2. 샘플 코드 해석

+ **`11111`**
  + `@Before`
    + 매번 테스트가 시작되기 전에 MockMvc 인스턴스를 생성합니다.

+ **`22222`**
  + `mvc.perform`
    + 생성된 MockMvc를 통해 API를 테스트합니다.
    + 본문(Body) 영역은 문자열로 표현하기 위해 ObjectMapper를 통해 문자열 JSON으로 변환합니다.


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
