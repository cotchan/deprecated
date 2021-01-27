---
title: JPA) 연관관계 매핑5. @MappedSuperClass
author: cotchan 
date: 2021-01-28 04:30:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. @MappedSuperclass

+ 공통 매핑 정보가 필요할 때 사용합니다. (id, name)

![Desktop View](/assets/img/post/jpa/2021-01-28-jpa-association-mapped-super-class-01.png)

---

## 2. 사용 예시

+ 특정 속성(정보)가 모든 테이블에 있어야 할 때 

```java
//필드
private String createdBy;
private LocalDateTime createdDate;
private String lastModifiedBy;
private LocalDateTime lastModifiedDate;
```

---

## 2-1. BaseEntity 생성

```java
@MappedSuperclass
public abstract class BaseEntity {

    private String createdBy;
    private LocalDateTime createdDate;
    private String lastModifiedBy;
    private LocalDateTime lastModifiedDate;
}
```

---

## 2-2. Team Entity에서 사용

```java
@Entity
public class Team extends BaseEntity {
    //...
}
```

---

## 2-3. Member Entity에서 사용

```java
@Entity
public class Member extends BaseEntity {
    //...
}
```

---

+ **@MappedSuperclass는 속성을 같이 쓰고 싶을 때 사용합니다.**

```java
create table Member (
   MEMBER_ID bigint not null,
    createdBy varchar(255),
    createdDate timestamp,
    lastModifiedBy varchar(255),
    lastModifiedDate timestamp,
    USERNAME varchar(255),
    ...	
)
```

---

```java
create table Team (
   TEAM_ID bigint not null,
    createdBy varchar(255),
    createdDate timestamp,
    lastModifiedBy varchar(255),
    lastModifiedDate timestamp,
    name varchar(255),
    primary key (TEAM_ID)
)
```

---

## 3. @MappedSuperclass 정리

![Desktop View](/assets/img/post/jpa/2021-01-28-jpa-association-mapped-super-class-02.png)

---

![Desktop View](/assets/img/post/jpa/2021-01-28-jpa-association-mapped-super-class-03.png)

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
