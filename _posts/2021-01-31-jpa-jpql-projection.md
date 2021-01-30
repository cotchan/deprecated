---
title: JPQL) 프로젝션(SELECT)
author: cotchan 
date: 2021-01-31 02:30:21 +0800 
categories: [JPA, JPA_JPQL]
tags: [jpql] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 프로젝션이란

+ 프로젝션은 SELECT 절에 조회할 대상을 지정하는 것입니다.
  + "~을 가져올 거야"

+ 프로젝션 대상(3가지)
  + 엔티티 타입
  +  임베디드 타입
  + 스칼라 타입(숫자, 문자 등 기본 데이터 타입)

+ **관계형 데이터베이스 같은 경우는 SELECT 결과가 스칼라 타입만 나온다는 점과 차이가 있습니다.**

![Desktop View](/assets/img/post/jpa/2021-01-31-jpa-jpql-projection-01.png)

---

+ **중요한 점**

```java
//결과로 나온 List<Member>는 영속성 컨텍스트에서 관리가 됩니다.
List<Member> result = em.createQuery("select m from Member m", Member.class)
    .getResultList();

Member findMember = result.get(0);

//update query가 나갑니다.
findMember.setAge(20);
```

+ **엔티티 프로젝션을 하면 SELECT 절의 대상이 되는 엔티티가 전부 영속성 컨텍스트에서 관리됩니다.** 

---

## 2. 프로젝션 - 여러 값 조회


```java
SELECT m.username, m.age FROM Member m
```

+ 위와 같이 여러 필드가 나열되어 있을 때는 3가지 방법으로 조회를 할 수 있습니다.

+ Query 타입으로 조회
  + 타입을 명시할 수 없으니 Object로 돌려줍니다.

```java
List resultList = em.createQuery("select m.username, m.age from Member m")
    .getResultList();

Object o = resultList.get(0);
Object[] result = (Object[]) o;

//username = member1
//age = 10
System.out.println("username = " + result[0]);
System.out.println("age = " + result[1]);
```

---

+ Object[] 타입으로 조회

```java
List<Object[]> resultList = em.createQuery("select m.username, m.age from Member m")
    .getResultList();

Object[] result = resultList.get(0);

//username = member1
//age = 10
System.out.println("username = " + result[0]);
System.out.println("age = " + result[1]);
```

---

## 2-1. new 명령어로 조회

+ **제일 깔끔한 방식은 new 명령어로 조회하는 것입니다.**
  + 단순 값을 DTO로 바로 조회하는 방법입니다.
  + 패키지 명을 포함한 전체 클래스명을 입력합니다.
  + 순서와 타입이 일치하는 생성자가 필요합니다.

```java
//MemberDTO 선언
public class MemberDTO {

    private String username;
    private int age;

    public MemberDTO(String username, int age) {
        this.username = username;
        this.age = age;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }
}
```

---

```java
//Main.java
//두 번째 parameter로 응답 클래스에 대한 type 값을 줄 수 있습니다.
//"난 타입을 MemberDTO로 뽑고 싶어"
List<MemberDTO> result = em.createQuery("select new jpql.MemberDTO(m.username, m.age) from Member m", MemberDTO.class)
    .getResultList();

MemberDTO memberDTO = result.get(0);

//username = member1
//age = 10
System.out.println("username = " + memberDTO.getUsername());
System.out.println("age = " + memberDTO.getAge());
```

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
