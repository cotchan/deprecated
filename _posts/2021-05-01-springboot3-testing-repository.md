---
title: sb3) Repository 테스트 코드 작성하기
author: cotchan 
date: 2021-05-01 20:00:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. Repository 테스트 코드는

+ 대체로 Repository는 테스트를 작성하지 않습니다.
+ 단, Repository 테스트를 하려면 Embedded Database를 띄워야 합니다.
+ 또는 test/resources/application.yml에서 DB 설정값을 따로줘서 할 수 있습니다.
+ **`자세한 내용은 아래 출처 글을 참고하면 됩니다.`**

---

## 2. UserRepository Interface

```java
public interface UserRepository {

    User insert(User user);

    Optional<User> findById(Long id);

    List<User> findAll();
}
```

---

## 3. UserRepositoryImplTest Class

```java
import com.github.prgrms.socialserver.SocialServerApplication;
import org.assertj.core.api.Assertions;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.annotation.Rollback;
import org.springframework.test.context.junit4.SpringRunner;

import java.util.Optional;

@RunWith(SpringRunner.class)
@SpringBootTest(classes = SocialServerApplication.class)
public class UserRepositoryImplTest {

    @Autowired
    private UserRepository userRepository;

    @Test
    @Rollback
    public void insert_findById_테스트() {
        User insertedUser = userRepository.insert(new User(new Email("zhtcks@gmail.com"), "1234"));
        Optional<User> findUsers = userRepository.findById(insertedUser.getSeq());

        boolean isExisted = findUsers.isPresent();

        if (isExisted) {
            User user = findUsers.get();
            Assertions.assertThat(user.getEmail().getEmailAddress()).isEqualTo(insertedUser.getEmail().getEmailAddress());
        } else {
            Assertions.fail("유저가 DataSource에 없습니다.");
        }
    }

    @Test
    @Rollback
    public void findAll_테스트() {

        int loopSize = 5;
        for (int i = 0; i < loopSize; ++i) {
            String emailAdress = "zhtcks" + i + "@gmail.com";
            userRepository.insert(new User(new Email(emailAdress), "1234"));
        }

        Assertions.assertThat(loopSize).isEqualTo(userRepository.findAll().size());
    }
}
```

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
    + [Configuring Separate Spring DataSource for Tests](https://www.baeldung.com/spring-testing-separate-data-source)
