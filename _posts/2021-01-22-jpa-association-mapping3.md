---
title: JPA) 연관관계 매핑2. 양방향 연관 관계와 연관관계의 주인2 
author: cotchan 
date: 2021-01-22 04:45:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 양방향 매핑시 가장 많이 하는 실수

---

## 1-1. 잘못된 사용 예시

+ **연관관계의 주인에 값을 입력하지 않음**

```java
Member member = new Member();
member.setUsername("member1");
em.persist(member);

Team team = new Team();
team.setName("TeamA");
team.getMembers().add(member);
em.persist(team);

tx.commit();
```

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-12.png)

---

## 1-2. 올바른 사용 예시

```java
Team team = new Team();
team.setName("TeamA");
em.persist(team);

Member member = new Member();
member.setUsername("member1");
member.setTeam(team);
em.persist(member);

tx.commit();
```

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-13.png)

---

## 1-3. BASE CASE

+ **BEST CASE는 항상 양쪽에 모두 값을 입력하는 게 좋습니다.**

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-14.png)

+ **양쪽에 값을 셋팅해주지 않으면 두 군데에서 문제가 발생합니다.**
  
1. `em.flush()`, `em.clear()`를 안해주는 경우
  + 이 경우에는 값이 **모두 1차 캐시에만 존재합니다. (메모리 상에서만 존재)**
  + 1차 캐시에서 조회된 게 그대로 결과로 나가게 됩니다.
  + 즉, 이 때 연관관계 주인에만 값을 셋팅해줬다면 members Collections에는 값이 없습니다.
2. 테스트 케이스에서의 문제
  + 테스트 케이스는 JPA 없이도 동작하도록 순수하게 자바 코드 상태로 작성하게 됩니다.
  + 이렇게 JPA 없이도 동작하는 테스트 케이스에서 문제가 될 수 있습니다.

+ **결론은 양방향 연관관계 매핑을 사용한다면 `양쪽 모두에 값을 셋팅`해주는게 바람직합니다.**


---

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-15.png)

---

## 2. 양방향 연관관계 사용 시 주의점

---

## 2-1. 연관관계 편의 메소드

+ 연관관계 편의 메소드란 
  + 양방향 연관관계에서 
  + 엔티티 양쪽 모두에 값을 셋팅하도록
  + 한 쪽에서 값을 셋팅할 때 자동으로 반대편에도 값을 셋팅하도록 메소드로 묶는 것.
  + 매 번 양쪽을 따로따로 업데이트하다보면 잊어버릴 수 있으므로 사용합니다. :) 

```java
@Entity
public class Member {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    @ManyToOne  
    @JoinColumn(name = "TEAM_ID") 
    private Team team;

    //...

    //연관관계 편의 메소드 추가
    public void setTeam(Team team) {
        this.team = team;

        team.getMembers().add(this);
    }

    //...
}
```

+ **추가로, 연관관계 편의 메서드나 JPA 상태를 변경하는 코드는 `SETTER 사용을 지양`합시다.**
  + 단 이것 또한 `둘 중에 한군데에서만` 셋팅하는 것이 바람직합니다.

```java
//case1.
@Entity
public class Member {
    public void changeTeam(Team team) {
        this.team = team;
        team.getMembers().add(this);
    }
}

//case2.
//또는 team.addMember(member) 메서드로 추가하기
@Entity
public class Team {
    public void addMember(Member member) {
        member.setTeam(this);
        members.add(member);
    }
}
```

---

## 2-2. 양방향 매핑시에 무한 루프 조심

+ **toString()**
  + member.toString()을 호출하면서 team.toString()을 호출하게 되고 
  + team.toString()을 호출하면서 다시 member.toString()을 호출하게 되면서 무한루프

+ **JSON 생성 라이브러리**
  + 문제가 생기는 경우
    + 엔티티를 직접 컨트롤러에서 Response로 보내고, 엔티티가 가진 연관관계가 양방향일 때 발생
    + Response 생성시 엔티티를 Json으로 바꿀 때 문제가 됩니다. 
      + member를 json으로 바꾸는데 => '어? team이 있네' => team 호출
      + team을 호출해서 json으로 바꾸는데 => '어? member가 있네' => member 호출 무한루프

+ **`해결방법`**
  1. LOMBOK에서 toString() 만드는 메소드 사용금지
  2. 컨트롤러에서는 절대 Entity 반환 금지
    + Entity는 `DTO`(값만 있는 형태)로 변환해서 반환하는 것을 권장합니다.

---

## 3. 양방향 매핑 정리

+ **가장 중요한 건, `단방향 매핑`만으로도 이미 연관관계 매핑은 완료된 것입니다.**
  + 단방향 매핑만으로 설계를 끝내는 게 정말 중요합니다.
  + 양방향 매핑을 하면 안 됩니다.
  + **JPA를 쓰기로 했다면 `단방향 매핑`만으로 설계를 완료해야합니다.**
    + 그리고 나서 양방향 매핑을 적용한다는 건, 반대 방향으로 `조회 기능이 추가`되는 것입니다.

+ 양방향 매핑이 들어가는 시점은
  + JPQL에서 역방향으로 탐색할 일이 많습니다.
  + 단방향 매핑을 잘해놓고 양방향은 필요할 때 추가하면 됩니다. 
  + 단방향 => 양방향으로의 전환이 **`테이블에 영향을 주지않습니다.`**

---

+ 단방향 연관관계일 때의 TABLE 형태

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-16.png)

---

+ 양방향 연관관계일 때의 TABLE 형태

![Desktop View](/assets/img/post/jpa/2021-01-21-jpa-association-mapping-17.png)

---

## 3-1. 연관관계 주인을 정하는 기준

+ 비즈니스 로직을 기준으로 연관관계 주인을 선택하면 안됩니다.
  + 자동차 - 자동차 바퀴 관계에서 비즈니스 로직적으로 자동차가 더 중요하더라도, 연관관계 주인은 자동차 바퀴입니다.

+ **연관관계의 주인은 `외래 키의 위치를 기준`으로 정해야 합니다.**
  + 이러면 헷갈일 일이 없습니다.
  + DB 설계를 생각했을 때 '어? 이 테이블에 외래키가 들어가네?' 그러면 얘를 연관관계 주인으로 정하면 됩니다.
    + 그 과정에서 반대쪽에도 값을 셋팅해야한다면 `연관관계 편의 메서드`를 사용합니다.

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
