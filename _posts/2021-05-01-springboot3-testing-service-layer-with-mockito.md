---
title: sb3) Service Layer 테스트 코드 작성하기(with Mockito)
author: cotchan 
date: 2021-05-01 00:00:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **`Junit4 기준`으로 작성한 글입니다.**

---

## 0. Intro

+ Service Layer 테스트 코드를 작성하는 순서에 대해 적은 포스팅입니다.
  + Mockito Framework를 어떻게 사용하는지는 나와있지 않습니다.
  + **Framework를 어떻게 사용하는지와 별개로 `Framework를 사용하여 뭘 작성해야 하는지`를 본인이 이해한 내용을 바탕으로 적었습니다.**

---

## 1. @RunWith(MockitoJUnitRunner.class)

+ **테스트 클래스에 `@RunWith(MockitoJUnitRunner.class)` 어노테이션을 선언하고 시작합니다.**
+ 저 같은 경우는 이 어노테이션을 선언하지 않았을 때 계속 `NPE`가 났었고 아래 스택오버플로우 글을 통해 해결했습니다.

```java
//UsersServiceImplTest.java

@RunWith(MockitoJUnitRunner.class)
public class UsersServiceImplTest {
  //...
}
```

---

## 2. 테스트 대상 객체에 @InjectMocks

+ **테스트의 대상인 Service Layer 인스턴스는 `Mock 객체를 주입받는 대상`이 됩니다.**

```java
public class UsersServiceImplTest {

    @InjectMocks
    private UsersServiceImpl usersService;

    @Mock
    UserRepository userRepository;

    //@Test...
}
```

+ 출처
  + [Service Layer Testing in Spring Boot (feat. Mockito)](https://dublin-java.tistory.com/49)

---

## 3. 테스트 대상이 의존하는 객체에 @Mock

+ **테스트 대상인 `Service Layer가 의존하는 객체는 Mock 객체로 만들어서 테스트`를 합니다.**
+ **Mock 객체로 만든다는 것의 의미는 저희가 임의로 INPUT OUTPUT을 정의하여 사용하겠다는 뜻입니다.**

```java
public class UsersServiceImplTest {

    @InjectMocks
    private UsersServiceImpl usersService;

    @Mock
    UserRepository userRepository;

    //@Test...
}
```

+ 출처
  + [Service Layer Testing in Spring Boot (feat. Mockito)](https://dublin-java.tistory.com/49)

---

## 4. @Test 함수 작성 방법

+ **실제 단위 테스트를 작성하는 부분입니다.**
+ 위 1,2,3번은 모두 실제 테스트 함수를 작성하기 전에 필요한 전처리 과정입니다.

---

## 4-1. given - willReturn 정의

+ **`서비스 객체가 의존하는 대상의 동작을 정의하는 부분`입니다.**
+ **`@Mock 어노테이션이 붙은 객체의 동작을 정의`합니다.**
+ 예시를 들자면 서비스 객체는 Repository 객체에 의존하기 때문에 제가 여기서 given - willReturn을 정의할 대상은 Repository 객체입니다.
+ given - willReturn말고 when이나 DoReturn 등 뭘 써도 상관없습니다.
  + **중요한 건 이 구문에서 정의하는 건 `테스트 대상이 의존하는 객체의 동작을 정의`한다는 것입니다.**

```java
@RunWith(MockitoJUnitRunner.class)
public class UsersServiceImplTest {

    @InjectMocks
    private UsersServiceImpl usersService;

    @Mock
    UserRepository userRepository;

    @Test
    public void insert_테스트() {

        //given
        given(userRepository.insert(any()))
                .willReturn(new User(new Email("zhtcks@gmail.com"), "1234"));
    
    //...
    }
}
```

---

## 4-2. Service Layer 함수 호출

+ 4-1에서 서비스 객체의 동작에 필요한 Repository의 동작을 정의했습니다.
+ **그러니 이제 `테스트하고 싶은 Service 객체의 함수를 호출`합니다.**

```java
@RunWith(MockitoJUnitRunner.class)
public class UsersServiceImplTest {

    @InjectMocks
    private UsersServiceImpl usersService;

    @Mock
    UserRepository userRepository;

    @Test
    public void insert_테스트() {

        //given
        given(userRepository.insert(any()))
                .willReturn(new User(new Email("zhtcks@gmail.com"), "1234"));

        User user = new User(new Email("zhtcks@gmail.com"), "1234");

        //when
        User insertedUser = usersService.insert(user);

        //...
    }
}
```

---

## 4-3. Assertions.assertThat

+ 테스트하고자 하는 서비스 함수를 호출했으니 결과가 예상대로 동작했는지 검증해봅니다.

```java
@RunWith(MockitoJUnitRunner.class)
public class UsersServiceImplTest {

    @InjectMocks
    private UsersServiceImpl usersService;

    @Mock
    UserRepository userRepository;

    @Test
    public void insert_테스트() {

        //given
        given(userRepository.insert(any()))
                .willReturn(new User(new Email("zhtcks@gmail.com"), "1234"));

        User user = new User(new Email("zhtcks@gmail.com"), "1234");

        //when
        User insertedUser = usersService.insert(user);

        //then
        verify(userRepository).insert(any());
        Assertions.assertThat(insertedUser.getEmail().getEmailAddress()).isEqualTo(user.getEmail().getEmailAddress());
        Assertions.assertThat(insertedUser.getPassword()).isEqualTo(user.getPassword());
    }
```


---

## 5. 부록: 포스팅에서 쓴 Service Class

+ 이 포스팅에서 예시로 나온 UsersServiceImpl Class 입니다.

```java
@Service
public class UsersServiceImpl implements UsersService {

    final UserRepository userRepository;

    @Autowired
    public UsersServiceImpl(UserRepository userRepository) {
        this.userRepository = userRepository;
    }

    @Override
    @Transactional
    public User join(Email email, String password) throws DataAccessException, IllegalArgumentException {

        if (password == null || password.isEmpty()|| password.length() > MAX_PASSWORD_LENGTH)
        {
            throw new IllegalArgumentException(String.format("비밀번호 필드는 비면 안 됩니다. 또한 비밀번호의 최대 글자 수는 %s 글자입니다.", MAX_PASSWORD_LENGTH));
        }

        User user = new User(email, password);
        return insert(user);
    }

    @Override
    public User insert(User user) {
        return userRepository.insert(user);
    }

    @Override
    @Transactional(readOnly = true)
    public Optional<User> findById(Long id) throws DataAccessException, IllegalArgumentException {
        return userRepository.findById(id);
    }

    @Override
    @Transactional(readOnly = true)
    public List<User> findAll() throws DataAccessException {
        return userRepository.findAll();
    }
}
```

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
    + [Mockito - NullpointerException when stubbing Method](https://stackoverflow.com/questions/33124153/mockito-nullpointerexception-when-stubbing-method)
    + [Mockito.mock() vs @Mock vs @MockBean](https://www.baeldung.com/java-spring-mockito-mock-mockbean)
    + [Java - Mockito를 이용하여 테스트 코드 작성하는 방법](https://codechacha.com/ko/mockito-best-practice/)
