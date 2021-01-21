---
title: JPA) 연관관계 매핑2. 양방향 연관 관계와 연관관계의 주인1
author: cotchan 
date: 2021-01-21 05:10:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 양방향 연관관계와 연관관계의 주인

+ 이게 어려운 이유?
  + **객체와 테이블의 패러다임의 차이 때문입니다.**
    + 테이블은 `외래키 조인` 사용
    + 객체는 `참조` 사용

+ 양방향 매핑으로 변환
  + **Member와 Team이 양쪽으로 `서로 참조할 수 있게` 만드는 것을 양방향 매핑이라고 합니다.**
  + 지금 Member => Team은 가능하지만 Team => Member는 불가능합니다.
  + **여기서 중요한 건 테이블 연관관계는 바뀐 점이 하나도 없습니다.**
    + `단방향` 연관관계에서나 `양방향` 연관관계에서나 테이블의 연관관계는 동일합니다.
      + 왜냐면 `JOIN`만 하면 알 수 있기 때문입니다.
    + 여기서 중요한 건 테이블에서는 단방향, 양방향의 개념이 없습니다(방향의 개념이 X)
    + **외래키 하나로 둘 다 처리가 가능합니다.**

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-04.png)

+ **하지만 객체는 다릅니다.**
  + 객체는 Team에다가 새로운 필드로 `List members`를 넣어줘야 양방향 매핑이 가능합니다.
  + 이게 테이블의 외래키와 객체의 참조의 가장 큰 차이점입니다.

---

## 2. 양방향 매핑으로의 변환

+ Member Side
  + Member 엔티티는 단방향 매핑 케이스와 코드가 동일합니다.

```java
@Entity 
public class Member {
    
    @Id @GeneratedValue  
    private Long id; 

    @Column(name = "USERNAME")  
    private String name;  
    private int age; 

    @ManyToOne 
    @JoinColumn(name = "TEAM_ID") 
    private Team team;  

    //...
}
```

---

+ Team Side
  + **Team 엔티티는 컬렉션을 새로 추가**

```java
@Entity 
public class Team { 

    @Id @GeneratedValue  
    private Long id; 
    private String name; 
   
    //양방향 매핑으로 바뀌면서 생긴 new field 
    @OneToMany(mappedBy = "team") 
    List<Member> members = new ArrayList<Member>();  
    
    //...
}
```

---

+ 이제는 양방향 매핑이므로 반대방향으로 객체 그래프 탐색이 가능합니다.

```java
//조회 
Team findTeam = em.find(Team.class, team.getId());

//역방향 조회 가능
int memberSize = findTeam.getMembers().size(); 
```

---

## 3. 양방향 매핑이 좋은가?

+ 객체는 가급적 단방향 매핑이 좋습니다. (신경쓸 게 적으므로)

---

## 4. mappedBy의 정체

+ 여기서 mappedBy에 대해 드는 의문
  + mappedBy는 굳이 필요한 값? 

```java
@Entity
public class Team {
 
    //...   

    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();

    //...
}
```

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-05.png)

---

## 4-1. '관계'를 맺음에 있어 객체와 테이블의 차이

+ 양방향 연관관계를 위해서, 객체는 연관관계를 맺는 키 포인트가 2개입니다.
  + **그냥 단방향 연관 관계가 2개입니다.**
    + Member에서 Team으로 가는 연관관계 1개
    + Team에서 Member로 가는 연관관계 1개
  
+ 테이블의 연관관계는 1개입니다. FK로 JOIN을 하면 양쪽의 연관관계를 모두 알 수 있습니다.
  + 테이블 연관관계에서는 FK값 하나로 모든 연관관계가 해결됩니다.
  + 테이블은 방향이 없다고 보는 게 맞습니다.(양쪽 모두 가능)

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-06.png)

---

## 4-2. 객체의 양방향 관계

+ 객체의 양방향 관계는 사실 양방향 관계가 아니라 서로 다른 단방향 관계가 2개입니다.
+ 그래서 객체를 양방향으로 참조하게 하려면 단방향 연관관계를 2개 만들어야 합니다.

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-07.png)
 
---

## 4-3. 테이블의 양방향 연관관계

+ 테이블은 `외래키` 하나로 두 테이블의 연관관계를 관리합니다.

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-08.png)

---

## 4-4. 둘의 차이에서 오는 딜레마

+ 양방향 관계 때문에 객체를 두 방향으로 만들어놓았습니다. (참조가 2개)
  + 그러면 우리는 객체의 참조 포인트 두 개중에 어떤 걸로 FK를 매핑??
  + **즉, 둘 중에 어떤 값을 UPDATE할 때 테이블의 FK값이 업데이트되게 해야하는지 문제가 발생합니다.**
    1. Member 객체의 team 값을 바꿀 때 FK 갱신?
    2. Team 객체의 members 값을 바꿀 때 FK 갱신?

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-09.png)

+ DB 입장에서는 TEAM_ID (FK) 값만 업데이트 되면 됩니다. 둘 중 누가 매핑을 하든 상관 X

---

## 4-5. 연관관계의 주인(Owner)

+ **그래서 둘 중 하나로 `외래키를 관리`할 수 있도록 룰이 생깁니다.**
  + 주인을 정해야 합니다. 이게 바로 **연관관계의 주인개념입니다.**
    1. Member 객체의 team 멤버로 외래키를 관리할지
    2. Team 객체의 members 멤버로 외래키를 관리할지

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-10.png)

---

## 4-6. 그러면 누구를 주인으로?

+ 답은 정해져 있습니다. **테이블 형태 기준으로 `외래키를 필드로 가지고 있는 곳`을 주인으로 정합니다.**
  + Member에서는 이미 @JoinColumn으로 TEAM_ID와 매핑을 해두었습니다.
  + **`mappedBy`가 적힌 곳은 `읽기만` 하는 것입니다.**
  + DB 테이블로 따져서 `다`쪽이 연관관계의 주인이 되면 됩니다.

```java
@Entity
public class Member {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @ManyToOne  
    @JoinColumn(name = "TEAM_ID") //이 관계에서 조인하는 컬럼은 무엇인지?
    private Team team;
    
    //...
}
```

```java
@Entity
public class Team {

    @Id
    @GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;

    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();

    //...
}
```

---

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-11.png)

+ **DB관계를 보면 외래키가 있는 곳이 1:다 관계에서 무조건 `다`입니다.**
  + 외래키가 없는 곳이 무조건 1입니다. 
  + 그러므로 `다`쪽이 무조건 연관관계의 주인이 되면 됩니다.
  + **JPA입장에서는 @ManyToOne, 즉 `ToOne`쪽이 무조건 연관관계의 주인입니다.** 

+ 연관관계의 주인은 DB 테이블로 따져서 `다`쪽이 연관관계의 주인이 되면 됩니다.
  + 즉 `외래키`를 필드로 가지고 있는 곳을 의미합니다.
  + 비즈니스 로직과는 크게 무관한 내용
  + 그냥 `자동차` - `자동차 바퀴`에서 자동차 바퀴가 연관관계의 주인이라는 것입니다.

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
