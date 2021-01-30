---
title: JPA) 값 타입 매핑으로 코드 수정 
author: cotchan 
date: 2021-01-30 22:16:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 값 타입 매핑 예제

+ Address를 ValueType으로 뽑아낸 것을 코드에 적용

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-code-01.png)

---

## 2. 코드 예시

+ Address 선언
  + 중요한 것은 따로 빼내고자 하는 값 타입 필드만 가져오기
  + @Embeddable 선언
  + equals()와 hashCode() 메소드 오버라이딩
    + **단 얘네를 구현할 때 getter로 접근하는 옵션을 사용하는 게 더 좋습니다.**
    + 프록시가 접근해도 문제가 없을 수 있도록

```java
@Embeddable
public class Address {

    private String city;
    private String street;
    private String zipcode;

    public String getCity() {
        return city;
    }

    private void setCity(String city) {
        this.city = city;
    }

    public String getStreet() {
        return street;
    }

    private void setStreet(String street) {
        this.street = street;
    }

    public String getZipcode() {
        return zipcode;
    }

    private void setZipcode(String zipcode) {
        this.zipcode = zipcode;
    }

    //getter로 접근하는 것이 더 바람직합니다.
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Address address = (Address) o;
        return Objects.equals(getCity(), address.getCity()) &&
                Objects.equals(getStreet(), address.getStreet()) &&
                Objects.equals(getZipcode(), address.getZipcode());
    }

    @Override
    public int hashCode() {
        return Objects.hash(getCity(), getStreet(), getZipcode());
    }
}
```

---

## 2-1. Member 필드 교체

```java
@Entity
public class Member extends BaseEntity {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    private String name;

    //생략해도 되지만 명확한 인식을 위해 적어주는 게 좋습니다.
    @Embedded
    private Address address;
}
```

---

```java
//TABLE은 이전과 동일하게 만들어집니다.
create table Member (
   MEMBER_ID bigint not null,
    createdBy varchar(255),
    createdDate timestamp,
    lastModifiedBy varchar(255),
    lastModifiedDate timestamp,
    city varchar(255),
    street varchar(255),
    zipcode varchar(255),
    name varchar(255),
    primary key (MEMBER_ID)
)
```

---

## 2-2. Delivery 필드 교체

```java
@Entity
public class Delivery extends BaseEntity {

    @Id
    @GeneratedValue
    private Long id;

    @Embedded
    private Address address;
}
```

---

```java
//TABLE은 이전과 동일하게 만들어집니다.
create table Delivery (
   id bigint not null,
    createdBy varchar(255),
    createdDate timestamp,
    lastModifiedBy varchar(255),
    lastModifiedDate timestamp,
    city varchar(255),
    street varchar(255),
    zipcode varchar(255),
    primary key (id)
)
```

---

## 3. 값 타입의 장점

+ **값 타입의 장점은 의미있는 비즈니스 메소드를 따로 뗴어내서 만들 수 있습니다.**

```java
@Embeddable
public class Address {

    private String city;
    private String street;
    private String zipcode;

    public String getFullAddress() {
        return getCity() + " " + getStreet() + " " + getZipcode();
    }
}
```

---

+ **공통적으로 사용되는 컬럼명에 Validation을 걸 수 있습니다.**

```java

@Embeddable
public class Address {

    @Column(length = 10)
    private String city;

    @Column(length = 20)
    private String street;

    @Column(length = 5)
    private String zipcode;
}
```

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
