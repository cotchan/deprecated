---
title: JPA) 영속성 전이: CASCADE와 고아객체
author: cotchan 
date: 2021-01-30 00:00:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. INTRO

+ **CASCADE는 즉시 로딩, 지연 로딩, 연관관계 셋팅과 전혀 무관한 내용입니다.**

+ CASCADE는 "부모를 저장할 때 연관된 자식도 함께 자동으로 Persist를 호출하고싶다!" 이럴 때 사용합니다.

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-cascade-01.png)


---

+ 기본적으로는 parent, child를 각 각 persist 해줘야 합니다.

```java
Child child1 = new Child();
Child child2 = new Child();

Parent parent = new Parent();
parent.addChild(child1);
parent.addChild(child2);

//그리고 em.persist를 세 번 호출해야 합니다.
em.persist(parent);
em.persist(child1);
em.persist(child2);
```

```java
//SQL
Hibernate: 
    /* insert jpabook.jpashop.domain.Parent
        */ insert 
        into
            Parent
            (name, parent_id) 
        values
            (?, ?)
Hibernate: 
    /* insert jpabook.jpashop.domain.Child
        */ insert 
        into
            Child
            (name, parent_id, id) 
        values
            (?, ?, ?)
Hibernate: 
    /* insert jpabook.jpashop.domain.Child
        */ insert 
        into
            Child
            (name, parent_id, id) 
        values
            (?, ?, ?)
```

---

+ 코드를 짤 때 parent 중심으로 하고 싶은 경우
  + parent를 persist할 때 자동으로 child도 persist해주고 싶은 경우

+ 이럴 때 CASCADE 옵션을 사용합니다.

```java
@Entity
public class Parent {

    @Id
    @GeneratedValue
    @Column(name = "parent_id")
    private Long id;

    private String name;

    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL)
    private List<Child> childList = new ArrayList<>();
    
    //...
}
```

---

+ 그러면 persist를 Parent에서 한 번만 호출해도 3번의 insert가 일어납니다.

```java
//Cascade 옵션 적용 후
Child child1 = new Child();
Child child2 = new Child();

Parent parent = new Parent();
parent.addChild(child1);
parent.addChild(child2);

em.persist(parent);
```

```java
//SQL
Hibernate: 
    /* insert jpabook.jpashop.domain.Parent
        */ insert 
        into
            Parent
            (name, parent_id) 
        values
            (?, ?)
Hibernate: 
    /* insert jpabook.jpashop.domain.Child
        */ insert 
        into
            Child
            (name, parent_id, id) 
        values
            (?, ?, ?)
Hibernate: 
    /* insert jpabook.jpashop.domain.Child
        */ insert 
        into
            Child
            (name, parent_id, id) 
        values
            (?, ?, ?)
```

---

+ 심플하게 "Parent를 persist할 때 Parent 안에 있는 Collection에 있는 애들을 전부 persist 날려줄거야"라는 의미입니다.
+ **Cascade는 연관관계랑은 전혀 무관한 것입니다.**
  + 연관관계를 매핑하는 것과 아무런 관련이 없습니다.
+ 엔티티를 영속화할 때 연관된 엔티티도 함께 영속화하는 편리함을 제공하는 것 뿐입니다.

---

## 2. Cascade 종류

+ **`ALL`: 모두 적용**
  + 라이프 사이클을 함께 가져가는 경우
+ **`PERSIST`: 영속**
  + 저장할 때만 라이프사이클을 맞추고 싶은 경우
+ **REMOVE: 삭제**
+ MERGE: 병합
+ REFRESH: REFRESH
+ DETACH: DETACH


+ 위에서 쓸만한 것은 ALL이나 PERSIST 옵션입니다.

---

## 3. 사용시 주의점

+ 하나의 부모가 자식들을 관리할 때는 Cascade 옵션이 의미가 있습니다.
  + 즉, Parent 엔티티만 Child 엔티티에 접근할 때
+ 파일을 여러 군데에서 관리하거나 다른 엔티티에서도 접근한다면 사용해서는 안 됩니다.


---

## 4. 고아 객체

+ 부모 엔티티와 연관관계가 끊어진 자식 엔티티를 자동으로 삭제하는 기능

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-cascade-02.png)

---

+ `orphanRemoval` 옵션을 사용하면 컬렉션에서 빠진 애는 자동으로 삭제가 됩니다.

```java
@Entity
public class Parent {

    @Id
    @GeneratedValue
    @Column(name = "parent_id")
    private Long id;

    private String name;

    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Child> childList = new ArrayList<>();

    //...
}
```

---

```java
Child child1 = new Child();
Child child2 = new Child();

Parent parent = new Parent();
parent.addChild(child1);
parent.addChild(child2);

em.persist(parent);

em.flush();
em.clear();

Parent findParent = em.find(Parent.class, parent.getId());

//이 시점에 Child 엔티티도 제거됩니다.            
findParent.getChildList().remove(0);
```

---

## 5. 고아 객체 주의점

+ **꼭 참조하는 곳이 하나일 때 사용해야합니다.**

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-cascade-03.png)


---

## 6. 영속성 전이 + 고아 객체, 생명주기

+ **CascadeType.ALL + orphanRemovel=true**
+ 스스로 생명주기를 관리하는 엔티티는 다음과 같이 관리해줍니다.
  + `em.persist()`로 영속화
  + `em.remove()`로 제거

+ 두 옵션을 모두 활성화하면 부모 엔티티를 통해서 자식의 생명주기를 관리할 수 있습니다.
+ 영속성 전이 + 고아 객체는 도메인 주도 설계(DDD)의 Aggregate Root 개념을 구현할 때 유용합니다.



---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
