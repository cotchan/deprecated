---
title: sb2) 12. PostApiController, PostsUpdateRequestDto, PostService (수정 기능 만들기)
author: cotchan 
date: 2021-03-04 23:59:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. PostsUpdateRequestDto 생성

+ PostsUpdateRequestDto를 `web.dto` 패키지에 생성합니다.
+ 수정을 허용할 필드만을 모아 DTO로 래핑합니다.
+ **이 DTO는 향후에 `@RequestBody` 파라미터로 사용합니다.**

```java
@Getter
@NoArgsConstructor
public class PostsUpdateRequestDto {

    private String title;
    private String content;

    @Builder
    public PostsUpdateRequestDto(String title, String content) {
        this.title = title;
        this.content = content;
    }
}
```

---


## 2. PostsApiController 코드 추가

```java
@RequiredArgsConstructor
@RestController
public class PostsApiController {

    private final PostsService postsService;

    //...

    @PutMapping("/api/v1/posts/{id}")
    public Long update(@PathVariable Long id, 
                       @RequestBody PostsUpdateRequestDto requestDto) {
        return postsService.update(id, requestDto);
    }
}
```

---

## 3. PostsService 코드 추가

```java
@RequiredArgsConstructor
@Service
public class PostsService {

    private final PostsRepository postsRepository;

    //...

    @Transactional
    public Long update(Long id, PostsUpdateRequestDto requestDto) {
        Posts posts = postsRepository.findById(id)
                .orElseThrow(() -> new IllegalArgumentException("해당 게시글이 없습니다. id=" + id));

        posts.update(requestDto.getTitle(), requestDto.getContent());
        return id;
    }
}
```

---

## 4. Posts에 수정 비즈니스 로직 추가

+ **도메인에 실제 비즈니스 로직 function을 추가합니다.**

```java
@Getter
@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Entity
public class Posts {

    //...

    public void update(String title, String content) {
        this.title = title;
        this.content = content;
    }
}
```

---

## 5. 수정 기능 검증 테스트

+ `PostsApiControllerTest`

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
    

    //...

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
