---
title: sb2) 11. PostApiController, PostsResponseDTO, PostService (조회 기능 만들기)
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
    
    //...

    public PostsResponseDto findById(Long id) {
        Posts entity = postsRepository.findById(id)
                .orElseThrow(() -> new IllegalArgumentException("해당 게시글이 없습니다. id=" + id));
        return new PostsResponseDto(entity);
    }
}
```

---

## 4. 조회 기능 검증 테스트

+ **조회 기능은 실제로 톰캣을 실행**확인 해보겠습니다.

---

## 4-1. 웹 콘솔 옵션 활성화

+ `application.properties`

```
spring.h2.console.enabled=true
```

---

## 4-2. 웹 콘솔에 접속

1. main 메소드를 실행합니다.
  + 정상적으로 실행됐다면 톰캣이 8080 포트로 실행됐습니다.
2. **`http://localhost:8080/h2-console`**
  + 위 주소로 접속하면 웹 콘솔화면이 등장합니다.

![Desktop View](/assets/img/post/spring-boot2/2021-03-05-h2-web-console.png)

---

## 4-3. 쿼리 작성

+ 아래와 같이 `POSTS 테이블`이 보여야 합니다. 

![Desktop View](/assets/img/post/spring-boot2/2021-03-05-posts-table.png)

---

+ **간단하게 insert 쿼리를 실행해보고 이를 API로 조회해보겠습니다.**

```
insert into posts (author, content, title) values ('author', 'content', 'title');
```

---

## 4-4. API로 검증 

+ 데이터가 제대로 DB에 반영되었는지 확인 후 API를 요청해봅니다.
+ `http://localhost:8080/api/v1/posts/1`

![Desktop View](/assets/img/post/spring-boot2/2021-03-05-api-result.png)

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
