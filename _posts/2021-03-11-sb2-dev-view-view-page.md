---
title: sb2) 16. [화면개발] 전체 조회 화면 만들기 
author: cotchan 
date: 2021-03-11 22:18:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 전체 조회 위해 index.mustache UI 변경

+ [index.mustache Code](https://gist.github.com/cotchan/6d6c1014aa529308d8ae8c74999dd25e)

+ 머스테치 문법
  + **`<<#posts>>`**
    + posts라는 List를 순회합니다.
    + Java등의 for문과 동일하다고 보면 됩니다.
  + **`<<변수명>>`**
    + List에서 뽑아낸 객체의 필드를 사용합니다.

---

## 2. Repository 코드 작성

+ 기존에 있던 `PostsRepository` 인터페이스에 **쿼리가 추가**됩니다.
+ SpringDataJpa에서 제공하지 않는 메소드는 `@Query`로 쿼리를 작성할 수 있습니다.

```java
package com.cotchan.review.springboot.domain.posts;

import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;

import java.util.List;

public interface PostsRepository extends JpaRepository<Posts, Long> {

    //add this
    @Query("SELECT p FROM Posts p ORDER BY p.id DESC")
    List<Posts> findAllDesc();
}
```

---

## 3. Service 코드 추가

+ `PostsService`에 코드를 추가합니다.

+ 새로 추가한 코드
  + postsRepository 결과로 넘어온 Posts의 Stream을 map을 통해 PostsListResponseDto 변환 -> List로 반환


```java
package com.cotchan.review.springboot.service.posts;

import com.cotchan.review.springboot.domain.posts.Posts;
import com.cotchan.review.springboot.domain.posts.PostsRepository;
import com.cotchan.review.springboot.web.dto.PostsListResponseDto;
import com.cotchan.review.springboot.web.dto.PostsResponseDto;
import com.cotchan.review.springboot.web.dto.PostsSaveRequestDto;
import com.cotchan.review.springboot.web.dto.PostsUpdateRequestDto;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;
import java.util.stream.Collectors;

@RequiredArgsConstructor
@Service
public class PostsService {

    private final PostsRepository postsRepository;

    //...

    @Transactional(readOnly = true)
    public List<PostsListResponseDto> findAllDesc() {
        return postsRepository.findAllDesc().stream()
                .map(PostsListResponseDto::new)
                .collect(Collectors.toList());
    }
}
```

---

```java
.map(PostsListResponseDto::new)

//위 코드는 아래와 같습니다.
.map(posts -> new PostsListResponseDto(posts))
```

---

## 4. PostsListResponseDto 만들기

+ `/web/dto` 패키지에 DTO 클래스를 생성합니다.

```java
package com.cotchan.review.springboot.web.dto;

import com.cotchan.review.springboot.domain.posts.Posts;
import lombok.Getter;

import java.time.LocalDateTime;

@Getter
public class PostsListResponseDto {
    private Long id;
    private String title;
    private String author;
    private LocalDateTime modifiedDate;

    public PostsListResponseDto(Posts entity) {
        this.id = entity.getId();
        this.title = entity.getTitle();
        this.author = entity.getAuthor();
        this.modifiedDate = entity.getModifiedDate();
    }
}
```


---

## 5. Controller 코드 수정

+ `IndexController`

+ **`Model`**
  + 서버 템플릿 엔진에서 사용할 수 있는 객체를 저장할 수 있습니다.
  + 여기서는 postsService.findAllDesc()로 가져온 결과를 `posts`라는 이름으로 `index.musstache`에 전달합니다.

```java
@RequiredArgsConstructor
@Controller
public class IndexController {

    private final PostsService postsService;

    @GetMapping("/")
    public String index(Model model) {
        model.addAttribute("posts", postsService.findAllDesc());
        return "index";
    }
}
```

---

## 6. 완성된 화면

![Desktop View](/assets/img/post/spring-boot2/2021-03-11-get-list.png)

---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
