---
title: JPA) 값 타입 컬렉션
author: cotchan 
date: 2021-01-30 19:42:21 +0800 
categories: [JPA, JPA_Basic]
tags: [jpa] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 값 타입 컬렉션

+ 값 타입 컬렉션이란 값 타입을 컬렉션에 담아서 쓰는 것을 말합니다.

---

+ 문제가 되는 건 DB 테이블로 구현할 때 문제가 됩니다.

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-collections-01.png)

+ 단순하게 값 타입이 하나일때는 테이블에 필드 속성으로 넣으면 가능했습니다.
+ 문제는 컬렉션이 DB에 들어가야하는데, RDB는 내부적으로 Collections을 담을 수 있는 구조가 내부에 없습니다.
  + 전부 Value로 값만 넣을 수 있습니다.
+ **컬렉션은 결국 1:다 개념이기 때문에, 컬렉션에 대한 별도의 테이블을 뽑아야 합니다.**
+ 그래서 테이블로 보면 위의 그림에서 두 번째 이미지처럼 되는 것입니다.
  + FAVORITE_FOOD 같은 경우는 `MEMBER_ID`와 `FOOD_NAME` 두 가지가 조합되서 PK를 이룹니다.
  + ADDRESS 같은 경우는 `MEMBER_ID`, `CITY`, `STREET`, `ZIPCODE`를 다 묶어서 하나의 PK를 만듭니다.
  + 이렇게 하는 이유는 식별자 개념을 적용하게 되면 얘는 Entity가 되버립니다.  
  + **값 타입은 테이블에 값 타입만 저장이 되고 얘네들을 묶어서 PK로 구성을 합니다.**

---

## 2. 사용 예시 

```java
@Entity
public class Member {

    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private Long id;

    //...

    @Embedded
    private Address homeAddress;
  
    //매핑하는 방법
    @ElementCollection
    //테이블명 지정하기, MEMBER_ID 외래키로 잡기
    @CollectionTable(name = "FAVORITE_FOOD", joinColumns = 
        @JoinColumn(name = "MEMBER_ID")
    )
    @Column(name = "FOOD_NAME") //FOOD는 필드가 하나이므로 예외적으로 이렇게 매핑해줘도 됩니다.
    private Set<String> favortieFoods = new HashSet<>();

    //매핑하는 방법
    @ElementCollection
    //테이블명 지정하기, MEMBER_ID 외래키로 잡기
    @CollectionTable(name = "ADDRESS", joinColumns = 
        @JoinColumn(name = "MEMBER_ID")
    )
    private List<Address> addressHistory = new ArrayList<>();
a}
``` 

---

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-collections-02.png)

---

## 3. 값 타입 컬렉션 사용 

+ **값 타입 컬렉션은 영속성 전이(Cascade) + 고아 객체 제거 기능을 필수로 가진다고 볼 수 있습니다.**
+ **값 타입 컬렉션도 `지연 로딩 전략`을 기본으로 사용합니다.**

---

## 3-1. 값 타입 저장 예제

```java
Member member = new Member();
member.setUsername("member1");
member.setHomeAddress(new Address("homeCity", "street", "10000"));

//컬렉션 값 셋팅
member.getFavoriteFoods().add("치킨");
member.getFavoriteFoods().add("족발");
member.getFavoriteFoods().add("피자");

member.getAddressHistory().add(new Address("old1", "street", "10000"));
member.getAddressHistory().add(new Address("old2", "street", "10000"));

//persist한 번에 모든 값이 셋팅이 됩니다.
em.persist(member);
```

---

+ 결과

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-collections-03.png)

+ 중요한 건 값 타입을 따로 persist하지 않고, member만 persist했는데도 값 타입 컬렉션들이 자동으로 라이프 싸이클이 같이 돌아간 점입니다.

+ 이게 가능한 이유는 값 타입 컬렉션도 값 타입이기 때문입니다.
  + 값 타입의 라이프 싸이클은 자신이 속한 엔티티에 의존합니다.

+ 그래서 값 타입은 별도로 persist 할 필요가 없습니다.


---

## 3-2. 값 타입 조회 예제

```java
Member member = new Member();
member.setUsername("member1");
member.setHomeAddress(new Address("homeCity", "street", "10000"));

//컬렉션 값 셋팅
member.getFavoriteFoods().add("치킨");
member.getFavoriteFoods().add("족발");
member.getFavoriteFoods().add("피자");

member.getAddressHistory().add(new Address("old1", "street", "10000"));
member.getAddressHistory().add(new Address("old2", "street", "10000"));

//persist한 번에 모든 값이 셋팅이 됩니다.
em.persist(member);

em.flush();
em.clear();

//결과를 찍어보면 Member만 가져옵니다.(지엱 로딩 사용)
Member findMember = em.find(Member.class, member.getId());

//가져와서 Loop를 돌면서 실제 값을 사용하는 시점에 쿼리가 나갑니다.
List<Address> addressHistory = findMember.getAddressHistory();
```

---

## 3-3. 값 타입 수정 예제

+ **중요**
+ 중요한 건 컬렉션의 값만 변경되도 실제 DB에 쿼리가 날아갑니다.
  + 어떤 게 변경되었는지를 알고 JPA가 알아서 바꿔줍니다.

```java
Member member = new Member();
member.setUsername("member1");
member.setHomeAddress(new Address("homeCity", "street", "10000"));

//컬렉션 값 셋팅
member.getFavoriteFoods().add("치킨");
member.getFavoriteFoods().add("족발");
member.getFavoriteFoods().add("피자");

member.getAddressHistory().add(new Address("old1", "street", "10000"));
member.getAddressHistory().add(new Address("old2", "street", "10000"));

//persist한 번에 모든 값이 셋팅이 됩니다.
em.persist(member);

em.flush();
em.clear();

Member findMember = em.find(Member.class, member.getId());

//homeCity => newCity로 변경
findMember.getHomeAddress().setCity("newCity"); //이런식으로 변경하면 안됩니다.

//바람직한 수정 방법
//값 타입 자체를 통으로 바꿔줍니다.
//값 타입 안의 필드하나만 바꾼다? ㄴㄴㄴㄴㄴ
findMember.setHomeAddress(new Address("newCity", "street", "10000"));


//값 타입 컬렉션 업데이트
//치킨 => 한식
//스트링 또한 값 타입이기 때문에 통째로 갈아껴야합니다. 
findMember.getFavoriteFoods().remove("치킨");
findMember.getFavoriteFoods().add("한식");


//주소 변경
//Address 인스턴스를 통으로 갈아껴야 합니다.
//삭제 시 대부분의 Collection이 동작하는 원리는 equals를 사용합니다.
//그래서 equals에 대한 오버라이딩을 제대로 해줘야 합니다. (equals()와 hashCode())
findMember.getAddressHistory().remove(new Address("old1", "street", "10000"));
findMember.getAddressHistory().add(new Address("newCity1", "street", "10000"));
```

---

## 4. 값 타입 컬렉션의 제약사항

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-collections-04.png)

+ **중요사항**
  + **값 타입 컬렉션에 변경 사항이 발생하면, 주인 엔티티와 연관된 모든 데이터를 삭제하고, 값 타입 컬렉션에 있는 현재 값을 모두 다시 저장합니다.**
  + 값 타입 컬렉션을 매핑하는 테이블은 모든 컬럼을 묶어서 기본키를 구성해야 합니다.
    + **null 입력 X, 중복 저장 X**

---

## 5. 값 타입 컬렉션 대안

+ **결론: Entity로 한 번 래핑**

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-collections-05.png)

---

```java
//Entity로 한 번 래핑을 하는 것입니다.
@Entity
@Table(name = "ADDRESS")
public class AddressEntity { 
    
    @Id
    @GeneratedValue
    private Long id;

    private Address address;
}
```

---

+ Member필드 교체하기

```java
//in Member.class

//before: 값 타입 매핑
    @ElementCollection
    @CollectionTable(name = "ADDRESS", joinColumns =
        @JoinColumn(name = "MEMBER_ID")
    )
    private List<Address> addressHistory = new ArrayList<>();

//after: 엔티티 매핑

    //단방향, 양방향은 자유
    @OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
    @JoinColumn(name = "MEMBER_ID")
    private List<AddressEntity> addressHistory = new ArrayList<>();
```

---

+ 위와 같이 바꾼 결과를 보면 **Address도 독자적인 ID를 갖고 있음**을 알 수 있습니다.
  + Entity 객체임을 알 수 있습니다.

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-collections-06.png)

---

## 6. 정리

+ 값 타입 컬렉션은 언제 쓰는가?
  + 진짜 단순할 때 사용하는 것이 좋습니다.

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-collections-07.png)

---

![Desktop View](/assets/img/post/jpa/2021-01-30-jpa-value-collections-08.png)

---

+ 출처
    + [자바 ORM 표준 JPA 프로그래밍 - 기본편](https://www.inflearn.com/course/ORM-JPA-Basic)
