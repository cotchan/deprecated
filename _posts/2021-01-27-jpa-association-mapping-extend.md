---
title: JPA) 연관관계 매핑4. 상속 관계 매핑
author: cotchan 
date: 2021-01-27 00:00:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 상속관계 매핑

+ **상속관계 매핑이란 DB의 논리모델링 기법을 3 가지 물리적 기법 중 어떤 것으로 구현하든 전부 매핑하도록 지원해주는 것을 말합니다.**

![Desktop View](/assets/img/post/jpa/2021-01-27-jpa-association-mapping-extend-01.png)

---

## 2. 물리적 모델 기법 3 가지

+ RDB의 슈퍼타입 서브타입 논리 모델을 실제 물리모델로 구현하는 기법에는 3 가지가 있습니다.
  1. **조인 전략** => 각각 테이블로 변환 (부모 - 자식)
  2. **싱글 테이블 전략** => 부모-자식을 모두 포함하는 하나의 통합 테이블로 변환
  3. **구현 클래스마다 테이블 전략** => 모두 서브타입 테이블로 변환

---

## 3. 주요 어노테이션

```java
/**
 * 부모 클래스에서 상속 전략을 정할 수 있습니다.
 */
@Entity
@Inheritance(strategy = InheritanceType.JOINED) //조인 전략
@DiscriminatorColumn    //DTYPE COLUMN을 보여줍니다.
public abstract class Item {
    //...
}
```

---

![Desktop View](/assets/img/post/jpa/2021-01-27-jpa-association-mapping-extend-03.png)

---

## 3-1. 조인 전략

+ **`기본 전략`이라고 보면 됩니다.**

---

```java
@Entity
@Inheritance(strategy = InheritanceType.JOINED) //조인 전략
@DiscriminatorColumn    //DTYPE COLUMN을 보여줍니다.
public abstract class Item {
    //...
}
```

+ 장점
  + 테이블이 정규화 되어있습니다.
  + 외래 키 참조 무결성 제약조건 활용 가능
    + 예를 들어, 다른 테이블에서 ITEM_ID을 검색할 때 ITEM TABLE만 보면 됩니다.
  + 저장 공간의 효율화 (정규화가 되어 있기 때문)

+ 단점
  + 조회 시 조인을 많이 사용합니다. => 성능 저하
  + 조회 쿼리가 복잡합니다.
  + 데이터 저장 시 INSERT SQL 2번 호출

![Desktop View](/assets/img/post/jpa/2021-01-27-jpa-association-mapping-extend-04.png)

---

## 3-2. 싱글 테이블 전략

+ 논리 모델을 하나의 테이블로 합쳐버리는 전략
+ 한 테이블에 다 집어넣고 `DTYPE`으로 구분하는 형태 

---

```java
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE) //싱글 테이블 전략
@DiscriminatorColumn    //DTYPE COLUMN을 보여줍니다.
public abstract class Item { 
    //...
}
```

+ 장점
  + 조인이 필요 없으므로 일반적으로 조회 성능이 빠릅니다.
  + 조회 쿼리가 단순합니다.

+ 단점
  + **자식 엔티티가 매핑한 컬럼은 모두 null을 허용합니다.**
    + 예를 들어, NAME, PRICE를 제외하고는 모두 null을 허용해야 합니다.
  + 단일 테이블에 모든 것을 저장하므로 테이블이 커질 수 있습니다. 상황에 따라서 조회 성능이 오히려 느려질 수 있습니다.

![Desktop View](/assets/img/post/jpa/2021-01-27-jpa-association-mapping-extend-02.png)

---

## 3-3. 구현 클래스마다 테이블 전략

+ **이 테이블 전략은 쓰지 않는 전략입니다.**
  + 데이터베이스 설계자와 ORM 전문가 둘 다 추천 X

+ 단점
  + 여러 자식 테이블을 함께 조회할 때 성능이 느립니다. (`UNION SQL` 필요)
  + 자식 테이블을 통합해서 쿼리하기 어렵습니다.

![Desktop View](/assets/img/post/jpa/2021-01-27-jpa-association-mapping-extend-05.png)


---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
