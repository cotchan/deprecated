---
title: sb2) 7. 도메인 패키지 / 도메인 클래스 / 도메인 레포지토리 생성
author: cotchan 
date: 2021-03-03 23:12:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 도메인 클래스란

+ **`@Entity`**
+ 도메인이란 게시글, 댓글, 회원, 정산, 결제 등 **소프트웨어에 대한 요구사항 혹은 문제 영역이라고 생각하면 됩니다.**
+ **JPA를 사용하면 DB 데이터에 작업할 경우 Entity 클래스의 수정을 통해 작업합니다.**

---

## 2. 도메인 패키지 posts 생성

+ domain 패키지에 `posts 패키지`를 생성합니다.

---

## 3. 도메인 클래스 Post 생성

+ posts 패키지안에 `Posts 클래스`를 만듭니다.

```java
import lombok.AccessLevel;
import lombok.Builder;
import lombok.NoArgsConstructor;

import javax.persistence.*;

@NoArgsConstructor(access = AccessLevel.PROTECTED)
@Entity
public class Posts {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 500, nullable = false)
    private String title;

    @Column(columnDefinition = "TEXT", nullable = false)
    private String content;

    private String author;

    @Builder
    public Posts(String title, String content, String author) {
        this.title = title;
        this.content = content;
        this.author = author;
    }
}
```

---

## 4. 도메인 레포지토리 생성

+ **`PostsRepository`**

+ **도메인 클래스 생성이 끝났다면, 도메인 클래스로 DB를 접근하게 해줄 JpaRepository를 생성합니다.**

+ **중요한 건 `Entity 클래스`와 기본 `Entity Repository`는 `함께 위치`해야 합니다.**
  + 둘은 아주 밀접한 관계이고, Entity 클래스는 **기본 Repository 없이는 제대로 역할을 할 수가 없습니다.**

![Desktop View](/assets/img/post/spring-boot2/2020-03-03-domain-repository.png)

---

```java
import org.springframework.data.jpa.repository.JpaRepository;

public interface PostsRepository extends JpaRepository<Posts, Long> {
}
```


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
