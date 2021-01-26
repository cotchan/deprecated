---
title: JPA) 연관관계 매핑3. 일대일[1:1] 
author: cotchan 
date: 2021-01-26 20:40:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 일대일 관계

+ 일대일 관계는 그 반대도 일대일입니다.
+ **주 테이블이나 대상 테이블 중에 외래 키 선택이 가능합니다.**
  + 이론상으로는 아무대나 넣어도 상관없습니다.
  + `주 테이블`에 외래 키
  + `대상 테이블`에 외래 키

+ **외래 키에 데이터베이스 `유니크(UNI) 제약조건` 추가가 되어야 일대일 관계가 됩니다.**

---

## 1-1. 일대일: 주 테이블에 외래 키 단방향

+ **다대일(`@ManyToOne`) 단방향 매핑과 유사합니다.**

![Desktop View](/assets/img/post/jpa/2021-01-26-jpa-association-mapping-1to1_01.png)

---

## 1-2. 코드 예시

+ **멤버가 Locker를 가지는 경우**
  + 아래와 같이 코드를 작성하면 일대일 관계가 끝납니다.

```java
@Entity
public class Member {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @OneToOne
    @JoinColumn(name = "LOCKER_ID")
    private Locker locker;
}
```

---

## 2. 양방향으로의 전환을 원하는 경우

+ ex. Locker도 Member가 누군지 알고싶은 경우
+ ex. Locker에서도 Member를 참조하고 싶은 경우

![Desktop View](/assets/img/post/jpa/2021-01-26-jpa-association-mapping-1to1_02.png)

+ 다대일 양방향 매핑처럼 **외래 키가 있는 곳이 연관관계의 주인입니다.**
+ 반대편은 mappedBy를 적용합니다.

---

+ Locker Entity에 Member 필드 추가

```java
@Entity
public class Locker {

    @Id
    @GeneratedValue
    private Long id;

    private String name;

    /**
     * 이 Locker는 Member Entity 에있는 locker입니다.
     */
    @OneToOne(mappedBy = "locker")
    private Member member;
}
```

---

## 3. 일대일: 대상 테이블에 외래 키 단방향

+ 테이블 상황
  + Member가 연관관계의 주인을 하고 싶은데, 외래 키가 LOCKER에 있는 경우

+ **아래와 같은 그림의 단방향 관계는 JPA가 지원하지 않습니다.**
  + 대상 테이블에 외래 키가 있는 단방향 관계는 지원 X

![Desktop View](/assets/img/post/jpa/2021-01-26-jpa-association-mapping-1to1_03.png)

---

+ 단, 양방향 관계는 지원합니다.
  + 주 테이블은 Member, 대상테이블인 LOCKER에 외래키가 있는 경우
  + 객체 연관관계를 양방향으로 잡으면 Locker에 있는 MEMBER를 연관관계의 주인으로 잡아서 매핑을 하면 됩니다.
  + 그리고 Member 엔티티에 있는 애를 읽기 전용으로 만들면 됩니다.

![Desktop View](/assets/img/post/jpa/2021-01-26-jpa-association-mapping-1to1_04.png)

---

## 4. 일대일 관계 정리

+ **내가 내껏만 관리할 수 있습니다. 즉, 내 엔티티에 있는 외래키는 내가 '직접'관리해야 합니다.**

--- 

## 4-1. 주 테이블에 외래 키를 두는 경우

+ 주 객체가 대상 객체의 참조를 가지는 것처럼 주 테이블에 외래 키를 두고 대상 테이블을 찾습니다.
+ 객체지향 개발자의 입장에서 선호하는 형태
+ JPA 매핑 편리(직관적, 성능상 유리)
+ **장점**: 주 테이블만 조회해도 대상 테이블에 데이터가 있는지 확인 가능
+ **단점**: 값이 없으면 외래 키에 null을 허용합니다.

---

## 4-2. 대상 테이블에 외래 키를 두는 경우

+ 대상 테이블에 외래 키가 존재
+ 전통적인 DBA 입장에서 선호하는 형태
+ **장점**: 주 테이블과 대상 테이블을 일대일에서 일대다 관계로 변경할 때 테이블 구조를 유지할 수 있습니다.
+ **단점**
  + 양방향으로 만들어야 합니다. (주로 Member에서 Locker를 엑세스할텐데, 그러므로 값을 동기화하기 위해 Member와 Locker를 양방향으로 만들어야합니다.)
  + 프록시 기능의 한계로 **지연 로딩으로 설정해도 `항상 즉시 로딩`이 됩니다.**
  + 쉬운 말로, Member에 Locker 값의 유무를 확인하려면 무조건 Locker 테이블을 뒤져야합니다.

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
