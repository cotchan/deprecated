---
title: Spring-Boot) 폼 객체의 사용(화면 계층 - 서비스 계층 분리)
author: cotchan 
date: 2021-01-10 00:00:21 +0800 
categories: [Spring-Boot, Spring-Boot_Tip]
tags: [spring-boot] 
---

## 1. 폼 객체를 사용하는 이유

+ **폼 객체를 사용하면 `화면 계층`과 `서비스 계층`을 명확하게 분리할 수 있습니다.**
+ 도메인 계층에서 사용하는 프로퍼티와 사용자로부터 입력받아야 하는 프로퍼티는 일치하지 않을 확률이 더 높습니다.
  + 이를, 사용자에게 받는 입력값을 도메인 계층의 프로퍼티로 억지로 맞추는 것이 아니라, `사용자로부터 받는 입력을 전담하는 폼 객체를 만듭니다.`
  + 그리고 나서 `Controller에서` `폼 객체` -> `도메인 객체로 가공`하는 과정을 거치는 것이 더 낫습니다. 

---


## 2. 폼 객체 예시 (회원가입 로직)

+ **`/domain/Member`**
  + 도메인 계층의 실제 비즈니스 로직을 위해 사용되는 `Member 엔티티`
+ **`/controller/MemberForm`**
  + 화면을 통해 사용자가 회원가입을 할 때 받는 입력값을 처리하는 `MemberForm 엔티티`


```java
package jpabook.jpashop.domain;

@Entity
@Getter @Setter
public class Member {

    @Id @GeneratedValue
    @Column(name = "member_id")
    private Long id;

    private String name;

    @Embedded
    private Address address;

    //Member 입장에서는 Member와 Order는 일대다 관계
    //"나는 연관관계의 주인이 아닙니다"를 표시 해줌(mappedBy)
    //"나는 Order 테이블에 있는 member 필드에 의해서 매핑된 겁니다."라고 하는 것. (즉, 읽기 전용 이라는 것)
    @OneToMany(mappedBy = "member")
    private List<Order> orders = new ArrayList<>();
}
```

---

```java
package jpabook.jpashop.controller;

@Getter @Setter
public class MemberForm {

    @NotEmpty(message = "회원 이름은 필수 입니다.")
    private String name;

    private String city;
    private String street;
    private String zipcode;
}
```


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
