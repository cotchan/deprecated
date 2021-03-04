---
title: sb2) @SpringBootTest
author: cotchan 
date: 2021-03-05 01:52:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Test]
tags: [test_annotation] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 의미

+ 어플리케이션 `통합테스트` 를 위한 어노테이션입니다.
+ **@SpringBootTest 어노테이션을 사용하면 스프링이 실행되고 ApplicationContext를 생성하여 동작합니다.**
  + 별다른 설정 없이 @SpringBootTest를 사용할 경우 **H2 데이터베이스**를 자동으로 실행해줍니다.
+ 별도의 클래스를 지정하지 않으면 전체 애플리케이션을 로드하고 전체 컴포넌트의 모든 Bean을 생성합니다.
+ 그러므로 필요한 Bean을 모두 올려서 테스트할 수 있습니다. 이 방법은 쉽게 컴포넌트를 주입할 수 있습니다.
+ **테스트 코드에 데이터베이스 연동 로직이 있다면, 스프링 실행 없이 테스트를 하는 경우에는 실패합니다.**
  + 실패 이유는 그 어떤 Bean도 생성되지 않았기 때문입니다.

+ 왜냐면, 스프링이 실행되어야 
  + JPA 연동 설정이 자동으로 실행되고,
  + DataSource를 생성하고,
  + Repository를 Bean으로 등록하기 때문입니다.

+ 이럴 때는 Mock 객체를 사용해서 테스트를 하면 됩니다.

---


## 2. 샘플 코드

```java
@RunWith(SpringRunner.class)
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
public class PostsApiControllerTest {

    @LocalServerPort
    private int port;

    @Autowired
    private TestRestTemplate restTemplate;

    @Autowired
    private PostsRepository postsRepository;

    @Test
    public void Posts_등록된다() throws Exception {
        //given
        String title = "title";
        String content = "content";
        PostsSaveRequestDto requestDto = PostsSaveRequestDto.builder()
                .title(title)
                .content(content)
                .author("author")
                .build();

        String url = "http://localhost:" + port + "/api/v1/posts";

        //when
        ResponseEntity<Long> responseEntity = restTemplate.postForEntity(url, requestDto, Long.class);

        //then
        assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(responseEntity.getBody()).isGreaterThan(0L);

        List<Posts> all = postsRepository.findAll();
        assertThat(all.get(0).getTitle()).isEqualTo(title);
        assertThat(all.get(0).getContent()).isEqualTo(content);
    }


    @Test
    public void Posts_수정된다() throws Exception {

        //given
        Posts savedPosts = postsRepository.save(Posts.builder()
                                            .title("title")
                                            .content("content")
                                            .author("author")
                                            .build());

        Long updateId = savedPosts.getId();
        String expectedTitle = "title2";
        String expectedContent = "content2";

        PostsUpdateRequestDto requestDto = PostsUpdateRequestDto.builder()
                                            .title(expectedTitle)
                                            .content(expectedContent)
                                            .build();

        String url = "http://localhost:" + port + "/api/v1/posts/" + updateId;

        HttpEntity<PostsUpdateRequestDto> requestEntity = new HttpEntity<>(requestDto);


        //when
        ResponseEntity<Long> responseEntity = restTemplate.exchange(url, HttpMethod.PUT, requestEntity, Long.class);

        //then
        assertThat(responseEntity.getStatusCode()).isEqualTo(HttpStatus.OK);
        assertThat(responseEntity.getBody()).isGreaterThan(0L);

        List<Posts> all = postsRepository.findAll();
        assertThat(all.get(0).getTitle()).isEqualTo(expectedTitle);
        assertThat(all.get(0).getContent()).isEqualTo(expectedContent);
    }
}
```





---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
  + [스프링부트 테스트(1)](https://brunch.co.kr/@springboot/207)
