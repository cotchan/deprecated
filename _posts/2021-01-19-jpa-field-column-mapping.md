---
title: JPA) 엔티티 매핑2. 필드와 컬럼 매핑
author: cotchan 
date: 2021-01-19 22:05:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 매핑 어노테이션 

+ @Column
+ @Temporal
+ @Enumerated
+ @Lob
+ @Transient

![Desktop View](/assets/img/post/jpa/2021-01-19-jpa-field-column-mapping-1.png)

---

## 2. @Column

+ **@Column이 제일 중요합니다.**

![Desktop View](/assets/img/post/jpa/2021-01-19-jpa-field-column-mapping-2.png)

---

## 2-1. name

+ 필드와 매핑할 테이블의 컬럼명을 정의할 수 있습니다. 

```java
    //DB column이 실제로는 user_name인 경우
    //객체에는 name이라고 쓰고 싶은데, db 컬럼명에는 user_name이라고 쓰고 싶은 경우
    @Column(name = "user_name")
    private String name;
```

---

## 2-2. nullable

+ nullable은 중요합니다.
+ `nullable = false` 이면, `not null` 제약조건이 걸립니다.

```java
    @Column(name = "user_name", nullable = false)
    private String name;
```

## 2-3. 기타사항

+ `unique`는 잘 안씁니다. 그 이유는 유니크 이름이 난수처럼 생성되서 식별이 어렵습니다.

---

## 3. @Enumerated

+ 기본값은 `ORDINAL`입니다.
  + 얘는 DB에 저장할 때 `integer`로 저장합니다.
+ **ORDINAL은 사용해서는 안 됩니다.** 
  + 자유롭게 Enum 값을 추가/삭제하는데 제약이 생깁니다.

```java
    @Enumerated(EnumType.STRING)
    private RoleType roleType;
```

![Desktop View](/assets/img/post/jpa/2021-01-19-jpa-field-column-mapping-3.png)

---

## 4. @Temporal

+ **Java8로 넘어오면서 LocalDate, LocalDateTime을 사용하므로 지금은 크게 쓰이지 않습니다.**
  + **Hibernate 최신 버전을 사용하게 되면 그냥 `LocalDate`와 `LocalDateTime`을 쓰면 됩니다.**

```java
    private LocalDate createDate; // 연월만 존재
    private LocalDateTime modifiedDate;	// 연월일 포함
```

+ 위와 같이 쓰면 Hibernate에서 **어노테이션이 없어도 DB에 적용**이 됩니다.

```java
//SQL
    createDate date,
    modifiedDate timestamp,
    //...    
```

![Desktop View](/assets/img/post/jpa/2021-01-19-jpa-field-column-mapping-4.png)

---

## 5. @Lob

+ @Lob에는 지정할 수 있는 속성이 따로 없습니다.

![Desktop View](/assets/img/post/jpa/2021-01-19-jpa-field-column-mapping-5.png)


---

## 6. @Transient

+ 매핑을 하기 싫으면 @Transient를 사용하면 됩니다.

![Desktop View](/assets/img/post/jpa/2021-01-19-jpa-field-column-mapping-6.png)

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
