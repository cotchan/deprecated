---
title: sb2) 10. PostApiController, PostsResponseDTO, PostService (조회 기능 만들기)
author: cotchan 
date: 2021-03-04 23:48:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. PostsResponseDto 생성

+ **조회 기능의 핵심은 DTO 형태로 가공해 `Response를 돌려주는 것`입니다.**
+ PostsResponseDto를 `web.dto` 패키지에 생성합니다.
+ PostsResponseDto는 **Entity의 필드 중 일부만 사용합니다.**
  + 그러므로 생성자로 Entity를 받아 필드에 값을 넣습니다.
  + 굳이 모든 필드를 가진 생성자가 필요하진 않으므로 Dto는 Entity를 받아 처리합니다.


```java
package com.cotchan.review.springboot.web.dto;

import com.cotchan.review.springboot.domain.posts.Posts;
import lombok.Getter;

@Getter
public class PostsResponseDto {

    private Long id;
    private String title;
    private String content;
    private String author;

    public PostsResponseDto(Posts entity) {
        this.id = entity.getId();
        this.title = entity.getTitle();
        this.content = entity.getContent();
        this.author = entity.getAuthor();
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

    @GetMapping("/api/v1/posts/{id}")
    public PostsResponseDto findById(@PathVariable Long id) {
        return postsService.findById(id);
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

    @Transactional
    public Long save(PostsSaveRequestDto requestDto) {
        return postsRepository.save(requestDto.toEntity()).getId();
    }

    //...

    public PostsResponseDto findById(Long id) {
        Posts entity = postsRepository.findById(id)
                .orElseThrow(() -> new IllegalArgumentException("해당 게시글이 없습니다. id=" + id));
        return new PostsResponseDto(entity);
    }
}
```


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
