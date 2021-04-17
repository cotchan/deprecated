---
title: sb3) 첫 번째 세션
author: cotchan 
date: 2021-04-16 00:27:21 +0800 
categories: []
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 관심사의 분리

---

- 프로그래밍 기초 개념 중 하나
    - **관심사를 분리한다는 건 기능을 쪼갠다는 뜻입니다.**
    - 인터페이스를 도입하고 인터페이스만 의존하게 만듭니다.
        - 생성자를 통해 의존 관계를 느슨하게 한다.
    - **객체에 대한 `생성과 사용을 구분해서 사용`하세요.**
    - 보통 객체 생성에 대한 책임을 지는 애를 `Factory`라고 합니다.
        - ex. DaoFactory가 UserDao의 생성을 책임집니다.

![Desktop View](/assets/img/post/spring-boot3/2021-04-16-00.png)


---

## 2. 직접 생성말고 DI를 사용하는 이유

---

- **`관심사의 분리`를 위해**
    - 기능을 쪼개서 중심 비즈니스 로직에 집중하기 위해
    - **즉, 생성의 역할은 DI 클래스에게 맡겨버리는 것**
- **`변경에 유연하게 대응`할 수 있기 때문에**
    - Application의 요구사항이 바뀌게 되면 의존을 주입하는 부분만 바꾸면 끝
    - 테스트 환경에서 Mock을 사용하여 특정 환경마다, 상황에 따라서 바꿀 수 있다.

---

## 3. 스프링과 서블릿

---

- 웹 서버를 개발하려면 http 서버를 만들어야 합니다.
- 그러나 좀 더 비즈니스 로직에 집중할 수 있도록
    - **서블릿 스펙을 준수하면 → 우리는 서버랑 통신하는 기능을 가진 어플리케이션을 만들 수 있습니다.**
    - **즉, 서블릿 스펙을 준수하면 다양한 서블릿 서버에서 내가 작성한 코드가 동작합니다.**
- 근데 스프링 MVC에서는 Dispatcher Servlet을 제공해줍니다.
    - 그러면 사용자는 이 서블릿에 설정만 하면 (= 여기에 내가 작성한 컨트롤러를 등록하면)
        - 특정 URL 요청이 오면 디스패쳐 서블릿이 URL 매핑에 맞는 컨트롤러를 호출해줌
- 스프링 부트를 써도 서블릿은 만들어집니다.
- 보통 스프링 기반 프로젝트는 WAS 위에 서버를 띄웁니다.
- WAS 서버 위에 Dispatcher 서블릿을 사용할 수 있습니다. (여러 개 또는 한 개 사용 가능

---

## 4. Application Context, Servlet Context

---

- **`Context란, 객체들이 들어가있는 공간을 의미`합니다. (즉, 빈들이 모여있는 곳)**
- Context는 계층 구조를 가지고 있습니다. (각 Context는 독립적으로 존재하는 게 X)
    - 하나의 root Context가 있고, 여기 자식으로 Servlet Context가 만들어집니다.
    - 자손들 끼리는 서로의 Bean Scope를 침범할 수 없지만
    - 자식 컨텍스트들에 정의된 Bean들은 부모 컨텍스트에 등록된 빈을 쓸 수 있습니다.
- **Application이 즉, Application Context 입니다.**
- Application Context도 주입받을 수 있습니다.

---

## 5. 스프링 부트 시작하기

---

- 스프링부트는 최적의 Convention으로 스프링 애플리케이션을 빠르게 개발하고 시작하게 해줍니다.
- 스프링부트는 web.xml이 없고, @SpringBootApplication만 있으면 뜹니다.
- 장점
    - `Embedded WAS`를 통해 main 클래스를 실행해서 서블릿을 바로 실행시킬 수 있습니다.
        - Embedded Tomcat, Embedded netty 등
    - **`AutoConfiguration`**
        - Bean을 등록하기 위한 일종의 템플릿
        - 여러가지 빈들을 일일이 등록하는 게 아니라, 자동으로 빈이 등록되게 해줍니다.
        - 어떤 Bean이 AutoConfiguration을 통해 등록되는지 공부하는 게 좋습니다.
    - `Packaging Excutable Jar`
        - JAR를 묶어서 java JAR cmd로 바로 서버가 뜰 수 있게 해줍니다.
        - MSA, 클라우드 환경에 적합하게 발전
    - 쉬운 외부 환경 설정

---

## 6. 트랜잭션이란 (트랜잭션 처리란)

---

- **`작업을 하나의 단위로 보는 것을 의미합니다.`**
- 즉, 하나의 단위로 본다는 것은 단위 작업 중에 하나라도 실패하면 `rollback`되게 하는 것.
    - 이렇게 트랜잭션 단위로 묶습니다.

```java
//UserService.java
//이 중에서 하나라도 exception이 t hrows 되면 전체를 다 취소한다.
@Transactional
public void addUsers(List<User> users) throws Exception {
	for (User user : users) {
		userRepository.add(user);
	}
}
```

---

## 7. Entity, DTO, VO

---

```java
//어떠한 필드 들이 불변이고 가변인지를 고려하며 Entity를 작성해야 합니다.
//어떤 건 변경이 안 되는지 명확하게 구분을 해야 합니다.
//setter를 주지 말고, 바뀌지 않게 불변 property로 만들어야 한다.

//final 필드와 아닌 필드의 차이
//setter가 필요하면 명확히 로직을 넣어야 합니다.

public class User {
	
	private final Long seq;

	private final Email email;

	private final String password;

	private int loginCount;

	private LocalDataTime lastLoginAt;

	private final LocalDataTime createAt;
}
```

## 7-1. Entity

---

- 도메인 모델에서의 Entity란
    - **`Identifier`가 있고, 시간에 따라서 내용이 계속 바뀌는 것을 Entity라 합니다.**
        - 바뀌는 이유 → 비즈니스 로직은 계속 변하기 때문
- 어떠한 필드들이 불변이고, 가변인지를 고려해서 Entity를 작성해야 합니다.
- setter를 주지 않습니다. 또한 setter가 필요하다면 명확한 로직(메서드)를 추가해야 합니다.
- Entity는 서비스 영역의 코드와 컨트롤러의 영역

## 7-2. VO(Value Object)

---

```java
public class Email {
	private final String address;
}
```

- 도메인을 만들 때 값 자체가 의미가 있는 경우에 VO로 만듭니다.
    - 왜 굳이 String이 아니고 Email VO를 만드는 가?
        - 도메인 상에 의미가 있는 경우에 VO로 만듭니다.
        - 위 예시의 경우 address 필드는 명확한 규칙이 있는 것.
- **VO의 특징은 Entity와 다르게 한 번 만들면 절대 바뀌지 않습니다.**

---

## 7-3. DTO

---

- DTO는 그저 JSON을 표현하기 위한 타입 객체
- DTO는 로직이 필요없음.
- Controller 영역에서는 DTO 위주로

---

## 8. Layer

![Desktop View](/assets/img/post/spring-boot3/2021-04-16-01.png)

---

![Desktop View](/assets/img/post/spring-boot3/2021-04-16-02.png)

---

![Desktop View](/assets/img/post/spring-boot3/2021-04-16-03.png)


---

## 8-1. Controller Layer

---

- **`User input Validation`은 Controller Layer에서 처리하기.**
- **Controller 영역에서는 `DTO 위주`로**
    - 그러므로 Controller에서는 parameter로 의미있는 dto를 사용하세요. (Entity X)
    - Controller에서 엔티티를 바로 전달받으면 안 됩니다. 꼭 DTO를 거치도록 한다.
        - → 그러면 엔티티에 비즈니스 로직을 위한 코드와 외부에 전달할 내용이 섞여버림

## 8-2. Service Layer

---

- **`Throw Exception` 여기서 던지기**
- Business Role은 이 Layer에 적용됩니다.
- Service 영역에서는 Entity 위주


---

+ 출처
