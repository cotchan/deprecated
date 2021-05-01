---
title: sb3) Controller Layer 테스트 코드 작성하기(with Mockito)
author: cotchan 
date: 2021-05-01 00:00:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **`JUnit4` 기준으로 작성하였습니다.**

---

## 1. @WebMvcTest, @RunWith

+ **@WebMvcTest, @RunWith 어노테이션을 테스트 클래스 상단에 선언합니다.**
+ [@WebMvcTest 의미](https://cotchan.github.io/posts/sb2-test-webmvctest/) 
+ [@RunWith(SpringRunner.class) 의미](https://cotchan.github.io/posts/sb2-test-runwith/)

```java
@WebMvcTest(controllers = UsersApiController.class)
@RunWith(SpringJUnit4ClassRunner.class)
//또는 @RunWith(SpringRunner.class)
public class UsersApiControllerTest {
   //....
}
```

---

## 2. MockMvc

+ **웹 API를 테스트해야하므로 MockMvc를 주입받습니다.**
+ [MockMvc 란](https://cotchan.github.io/posts/sb2-test-mockmvc/)

```java
@Autowired
private MockMvc mockMvc;
```

---

## 3. @MockBean

+ **Controller를 테스트하는데 필요한 `Service 객체를 모킹합니다.`**

```java
@MockBean
UsersService usersService;
```

---

## 4. 1,2,3 정리

+ **Controller 테스트를 위해 `@Test 함수를 작성하기에 앞서 필요한 것들`입니다.**

```java
@WebMvcTest(controllers = UsersApiController.class)
@RunWith(SpringJUnit4ClassRunner.class)
public class UsersApiControllerTest {

    @MockBean
    UsersService usersService;

    @Autowired
    private MockMvc mockMvc;

    //...
}
```

---

## 5. Controller 테스트 함수 구조

+ **Controller를 테스트하는 함수의 구조라 하면 아래와 같습니다.**
  + **`사용할 Service 객체 모킹`**
    + Controller에서 사용하는 Service Layer function을 모킹합니다.
  + **`mockMvc.perform call`**
    + 이 시점에 GET, POST 등 HTTP 메서드로 API를 호출합니다.
  + **`verify`**
    + 모킹했던 Service Layer function이 실제 호출되었는지 확인합니다.

---

## 5-1. findAll 테스트 예시(GET)

```java
@WebMvcTest(controllers = UsersApiController.class)
@RunWith(SpringJUnit4ClassRunner.class)
public class UsersApiControllerTest {

    @MockBean
    UsersService usersService;

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void findAll_테스트() throws Exception {

        //given
        Mockito.when(usersService.findAll()).thenReturn(
                new ArrayList<User>() {{
            add(new User(new Email("zhtcks@gmail.com"), "1234"));
            add(new User(new Email("test01@gmail.com"), "1234"));
            add(new User(new Email("test02@gmail.com"), "1234"));
            add(new User(new Email("test03@gmail.com"), "1234"));
        }});

        //when
        //then
        mockMvc.perform(get("/api/users").contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.success").value("true"))
                .andExpect(jsonPath("$.response.count").value(4));

        verify(usersService).findAll();
    }
}
```

---

## 5-2. findById 테스트 예시(GET)

```java
@WebMvcTest(controllers = UsersApiController.class)
@RunWith(SpringJUnit4ClassRunner.class)
public class UsersApiControllerTest {

    @MockBean
    UsersService usersService;

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void findById_테스트_유저가_있는_경우() throws Exception {

        //given
        //new User(new Email("zhtcks@gmail.com"), "1234");
        User user = createUser();

        Mockito.when(usersService.findById(1L)).thenReturn(Optional.of(user));

        //when
        //then
        mockMvc.perform(get("/api/users/1").contentType(MediaType.APPLICATION_JSON))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.success").value("true"))
                .andExpect(jsonPath("$.response.email").value("zhtcks@gmail.com"))
                .andExpect(jsonPath("$.response.password").value("1234"));

        verify(usersService).findById(anyLong());
    }
}
```

---

## 5-3. join 테스트 예시(POST)

```
@WebMvcTest(controllers = UsersApiController.class)
@RunWith(SpringJUnit4ClassRunner.class)
public class UsersApiControllerTest {

    @MockBean
    UsersService usersService;

    @Autowired
    private MockMvc mockMvc;

    @Test
    public void join_테스트_성공하는_경우() throws Exception {

        //given
        //new User(new Email("zhtcks@gmail.com"), "1234");
        User user =  createUser();

        Mockito.when(usersService.join(any(), any())).thenReturn(user);

        String requestBody = "{\"principal\":\"zhtcks@gmail.com\",\"credentials\":\"1234\"}";

        //when
        //then
        mockMvc.perform(post("/api/users/join").contentType(MediaType.APPLICATION_JSON).content(requestBody))
                .andExpect(status().isOk())
                .andExpect(jsonPath("$.success").value("true"))
                .andExpect(jsonPath("$.response.success").value("true"))
                .andExpect(jsonPath("$.response.response").value("가입완료"));

        verify(usersService).join(any(), any());
    }
}
```

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
    + [Testing Spring Boot RESTful APIs using MockMvc/Mockito, Test RestTemplate and RestAssured](https://medium.com/swlh/https-medium-com-jet-cabral-testing-spring-boot-restful-apis-b84ea031973d)
    + [Testing the Web Layer](https://spring.io/guides/gs/testing-web/)
