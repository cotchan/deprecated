---
title: sb2) 7-2. 도메인에 생성시간/수정시간 자동화하기 (JPA Auditing)
author: cotchan 
date: 2021-03-03 23:20:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_DEV]
tags: [spring-boot2] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. LocalTime, LocalDate

+ Java8부터 `LocalDate`와 `LocalDateTime`이 등장했습니다.


---

## 2. BaseTimeEntity 생성

+ domain 패키지에 BaseTimeEntity 클래스를 생성합니다.
+ BaseTimeEntity 클래스는 모든 Entity의 상위 클래스가 되어 **`Entity들의 createdDate, modifiedDate를 자동으로 관리하는 역할`**입니다.

```java
import lombok.Getter;
import org.springframework.data.annotation.CreatedDate;
import org.springframework.data.annotation.LastModifiedDate;
import org.springframework.data.jpa.domain.support.AuditingEntityListener;

import javax.persistence.EntityListeners;
import javax.persistence.MappedSuperclass;
import java.time.LocalDateTime;

@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)
public abstract class BaseTimeEntity {

    @CreatedDate
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime modifiedDate;
}
```

---

1. **`@MappedSuperclass`**
  + JPA Entity 클래스들이 BaseTimeEntity를 상속할 경우 필드들(createdDate, modifiedDate)도 칼럼으로 인식하도록 합니다.
2. **`@EntityListeners(AuditingEntityListener.class)`**
  + BaseTimeEntity 클래스에 Auditing 기능을 포함시킵니다.
3. **`@CreatedDate`**
  + Entity가 생성되어 저장될 때 시간이 자동 저장됩니다.
4. **`@LastModifiedDate`**
  + 조회한 Entity의 값을 변경할 때 시간이 자동 저장됩니다.

---

## 3. 엔티티가 BaseTimeEntity 상속하도록 변경

+ 앞으로 추가될 엔티티들은 더이상 등록일/수정일로 고민하지 않아도 됩니다.
+ **BaseTimeEntity만 상속받으면 자동으로 해결됩니다.**

```java
//...
public class Posts extends BaseTimeEntity {
    //...
}
```

---

## 4. JPA Auditing 활성화

+ `@EnableJpaAuditing`
+ JPA Auditing 어노테이션들을 모두 활성화할 수 있도록 `Application` 클래스에 활성화 어노테이션을 추가해줍니다.

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.data.jpa.repository.config.EnableJpaAuditing;

@EnableJpaAuditing  // JPA Auditing 활성화
@SpringBootApplication
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

---

## 5. JPA Auditing 테스트 코드 작성

+ `PostsRepositoryTest` 클래스에 테스트 메소드 추가

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class PostsRepositoryTest {

    @Autowired
    PostsRepository postsRepository;

    //...


    @Test
    public void BaseTimeEntity_등록() {
        //given
        LocalDateTime now = LocalDateTime.of(2019, 6, 4, 0, 0, 0);
        postsRepository.save(Posts.builder()
                .title("title")
                .content("content")
                .author("author")
                .build());

        //when
        List<Posts> postsList = postsRepository.findAll();

        //then
        Posts posts = postsList.get(0);

        System.out.println(">>>>> createDate=" + posts.getCreatedDate() + ", modifiedDate=" + posts.getModifiedDate());

        assertThat(posts.getCreatedDate()).isAfter(now);
        assertThat(posts.getModifiedDate()).isAfter(now);
    }
}
```


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
