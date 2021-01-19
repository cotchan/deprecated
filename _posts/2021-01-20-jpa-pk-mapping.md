---
title: JPA) 
author: cotchan 
date: 2021-01-14 00:00:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 기본키 매핑 어노테이션

+ 사용할 수 있는 어노테이션은 크게 아래 두 가지가 있습니다.

+ `@Id`
  + Id만 사용하면 PK를 직접할당 해줄 수 있습니다.
+ `@GeneratedValue`
  + PK를 자동으로 생성할 수 있습니다.

![Desktop View](/assets/img/post/jpa/2021-01-20-jpa-pk-mapping-01.png)

---

## 2. 자동할당

---

## 2-1. AUTO 전략

+ 기본값입니다.
+ AUTO는 DB방언에 따라서 TABLE, SEQUENCE, IDENTITY 중 하나가 자동으로 선택됩니다.

```java
    @Id
    @GeneratedValue(strategy = GenerationType.AUTO) 
    private Long id;
```

---

## 2-2. IDENTITY 전략

+ 기본 키 생성을 DB에 위임합니다.
  + "난 모르겠고, DB 너가 알아서 해줘"
+ 대표적인 예가 Mysql의 AUTO_INCREMENT

```java
    //PK 매핑
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
```

```java
//Hibernate
    create table Member (
       id bigint not null auto_increment,
        name varchar(255) not null,
        primary key (id)
    ) engine=MyISAM
```

![Desktop View](/assets/img/post/jpa/2021-01-20-jpa-pk-mapping-02.png)

--- 

## 2-2-1. IDENTITY 전략 추가사항

+ IDENTITY 전략은 내가 ID에 값을 넣으면 안됩니다.
+ 즉, `null`로 값을 넘기면 그때 DB에서 insert로 값을 셋팅해줍니다.
+ 그러면 뭐가 문제냐면 ID값을 아는 시점이 DB에 값이 들어가는 시점에 ID값을 알 수 있습니다.

+ **근데 영속성 컨텍스트에서 관리가 되려면 무조건 PK값이 있어야 합니다.**
  + 그래서 어떤 제약이 생기냐면 em.persist를 호출한 시점에 바로 DB에 insert 쿼리를 날려버립니다.
    + commit하는 시점에 insert 쿼리가 나가는 원칙과는 달라집니다.
  + 결론은 IDENTITY 전략에서는 모아서 INSERT하는 게 불가능합니다. (단점)

```java
    try {

        Member member = new Member();
        member.setUsername("C");

        System.out.println("=====================");
        em.persist(member);
        System.out.println("=====================");

        tx.commit();

    } catch (Exception e) {
        tx.rollback();
    } finally {
        em.close();
    }
```


---

## 2-3. SEQUENCE 전략

+ 데이터베이스 시퀀스는 시퀀스 오브젝트를 만들어냅니다.
  + `데이터베이스 시퀀스`는 유일한 값을 순서대로 생성하는 특별한 데이터베이스 오브젝트입니다.

+ 오라클, PostgreSQL, DB2, H2 데이터베이스에서 사용합니다.

+ SEQUENCE전략은 데이터베이스에 있는 시퀀스 오브젝트를 통해 값을 생성합니다.
  + 시퀀스 오브젝트를 통해 값을 가져오고 난 후 값을 셋팅합니다.

```java
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE)
    private Long id;
```

![Desktop View](/assets/img/post/jpa/2021-01-20-jpa-pk-mapping-03.png)

---

![Desktop View](/assets/img/post/jpa/2021-01-20-jpa-pk-mapping-04.png)

---

## 2-3-1. SEQUENCE 전략 추가사항

```java
@Entity
@SequenceGenerator(
    name = "MEMBER_SEQ_GENERATOR",
    sequenceName = "MEMBER_SEQ",
    initialValue = 1, allocationSize = 1)
public class Member {

    @Id
    @GeneratedValue(strategy = GenerationType.TABLE,
            generator = "MEMBER_SEQ_GENERATOR")
    private Long id;
```

1. `em.persist`를 할 때 영속성 컨텍스트에 넣어야 합니다. 그럴려면 항상 PK가 있어야 합니다.    
2. 그러면 시퀀스 오브젝트를 먼저 가져온 다음, 시퀀스에서 내 PK를 가져와야 합니다.
3. 그러면 JPA가 시퀀스 전략인 걸 보고 SEQUENCE에서 값을 가져오는 매커니즘으로 동작합니다.
4. 그래서 아래와 같은 쿼리가 나가는 것을 볼 수 있습니다.

```java
Hibernate:
    call next value for MEMBER_SEQ
```

1. 시퀀스에서 값을 얻어와서 member에 Id값을 넣어줍니다.
2. 그리고 영속성 컨텍스트에 넣어줍니다.
  + IDENTITY 전략과 다르게 이 시점에 insert 쿼리가 나가지는 않습니다.

+ 즉 시퀀스 방식은 버퍼링하는 게 가능합니다.

+ **`allocationSize`**
  + 시퀀스 객체를 자주 호출하는 게 병목으로 느껴질 수 있습니다.
  + 그래서 allocationSize는 default가 50입니다.
  + 즉, 한 번 호출마다 DB에 미리 50개를 땡겨놓습니다. (한 번 호출할때마다 50씩 느는 방식)
  + DB에는 미리 50으로 셋팅을해놓고 메모리에서 1씩 갖다 씁니다.
  + 이러다가 꽉 차면 다시 call next를 통해 호출해줍니다. 
  + 그래서 DB에 미리 올려놓고 메모리에서 갖다쓰는 방식을 활용합니다.
  + 이 방식을 사용하면 여러 웹서버가 있어도 동시성 이슈 없이 다양한 문제를 해결합니다.

---

```java
@Entity
@SequenceGenerator(
    name = "MEMBER_SEQ_GENERATOR",
    sequenceName = "MEMBER_SEQ",
    initialValue = 1, allocationSize = 50)
public class Member {

    @Id
    @GeneratedValue(strategy = GenerationType.TABLE,
            generator = "MEMBER_SEQ_GENERATOR")
    private Long id;
```

```java
//SEQUENCE 전략 사용

    Member member1 = new Member();
    member1.setUsername("A");

    Member member2 = new Member();
    member2.setUsername("B");

    Member member3 = new Member();
    member3.setUsername("C");

    System.out.println("=====================");

    //DB SEQ = 1 (첫 번째 호출) | APP SEQ = 1
    //DB SEQ = 51 (두 번째 호출) | APP SEQ = 2
    //DB SEQ = 51 | APP SEQ = 3
    em.persist(member1);    //1, 51
    em.persist(member2);    //Memory에서 호출
    em.persist(member3);    //Memory에서 호출
    System.out.println("=====================");

    tx.commit();
```



---

## 2-4. TABLE 전략

+ Table 전략은 KEY 생성 전용 테이블을 하나 만들어서 데이터베이스의 시퀀스를 흉내내는 전략입니다.
  + 장점: 모든 데이터베이스에 적용 가능
  + 단점: 성능(최적화가 되어있지 않습니다.)

```java
@Entity
@TableGenerator(
        name = "MEMBER_SEQ_GENERATOR",
        table = "MY_SEQUENCES",
        pkColumnValue = "MEMBER_SEQ", allocationSize = 1)
public class Member {

    //PK 매핑
    @Id
    @GeneratedValue(strategy = GenerationType.TABLE,
            generator = "MEMBER_SEQ_GENERATOR")
    private Long id;
```

+ 그러면 추가로 아래와 같은 테이블이 생성됩니다.

```java
//SQL
    create table MY_SEQUENCES (
       sequence_name varchar(255) not null,
        next_val bigint,
        primary key (sequence_name)
    )
```

![Desktop View](/assets/img/post/jpa/2021-01-20-jpa-pk-mapping-05.png)


+ Table에 있는 allcationSize은 SEQUENCE에서의 allocationSize 전략과 동일합니다.


---

## 3. 권장하는 PK 전략

+ **기본 키 제약 조건**
  1. `null X`
  2. `유일해야` 합니다.
  3. **`변하면 안 된다.`**
    + 사실 이 조건을 찾기가 가장 까다롭습니다.
    + Application 생애 동안 절대로 변해서는 안 됩니다.
    + 미래까지 이 조건을 만족하는 자연키는 찾기가 어렵습니다. 그러므로 대체키를 사용하는 게 낫습니다.
      + 대체키 - 비즈니스와 전혀 상관없는 키(@GeneratedValue나 난수 등)
    + **예를 들어 주민등록번호도 기본 키로 적절하지 않습니다.**

+ **권장하는 방법**
  1. Type은 `Long`형
  2. 대체키 사용 - (UUID 등)
  3. 키 생성전략 활용
  + 즉, `AUTO_INCREMENT`나 `Sequence Object` 둘 중에 하나 사용을 권장

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
