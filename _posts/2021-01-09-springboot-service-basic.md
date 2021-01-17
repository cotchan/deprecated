---
title: Spring-Boot) Service 클래스 생성방법(1)
author: cotchan 
date: 2021-01-05 00:00:21 +0800 
categories: [Spring-Boot, Spring-Boot_Service]
tags: [spring-boot] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. Service 클래스 생성에 필요한 것

+ **서비스 클래스를 생성할 때 아래의 어노테이션이 필요합니다.**

```java
@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class ServiceClass {

    private final RepositoryClass repositoryClass;

}
```

---

## 2. 적용된 기술 설명


+ **`@Service`**
  + ComponentScan의 대상이 되어 빈으로 등록됩니다. @Component 어노테이션을 래핑  
+ **`@Transactional(readOnly = true)`**
  + **JPA의 모든 데이터 변경 로직들은 전부 트랜잭션 안에서 실행되어야 합니다. 그러므로 추가해줘야 합니다.**
  + 기본적으로 readOnly 옵션을 걸어두고, 데이터를 추가/변경하는 메서드에만 `@Transactional`을 추가해줍니다. 
+ **`@RequiredArgsConstructor`**
  + `final` 키워드가 선언 되어 있는 필드만 가지고 생성자를 만들어줍니다.
  + 생성자 주입방식을 사용하기 위해 이렇게 사용합니다.
  + 스프링은 생성자가 1개인 경우 별도의 어노테이션이 없어도 자동으로 @Autowired 옵션을 적용해줍니다.

---

## 3. Sample Code

+ Sample1: `MemberService`

```java
package jpabook.jpashop.service;

import jpabook.jpashop.domain.Member;
import jpabook.jpashop.repository.MemberRepository;
import lombok.AllArgsConstructor;
import lombok.RequiredArgsConstructor;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.util.List;

@Service
//JPA의 모든 데이터 변경, 로직들은 전부 트랜잭션 안에서 실행되어야 합니다.
@Transactional(readOnly = true)
//@AllArgsConstructor //모든 필드를 가지고 생성자를 만들어줍니다.
@RequiredArgsConstructor    //final이 있는 필드만 가지고 생성자를 만들어줍니다.
public class MemberService {

    private final MemberRepository memberRepository;

//    @AllArgsConstructor, @RequiredArgsConstructor가 아래와 같은 생성자 생성자를 만들어줍니다.
//    @Autowired
//    public MemberService(MemberRepository memberRepository) {
//        this.memberRepository = memberRepository;
//    }

    /**
     * 회원 가입
     */
    @Transactional  //write에는 readOnly = false가 적용되게 합니다.
    public Long join(Member member) {

        //중복회원 검증 로직
        validateDuplicateMember(member);
        memberRepository.save(member);
        return member.getId();
    }

    /**
     * 중복회원 검증 로직
     */
    private void validateDuplicateMember(Member member) throws IllegalStateException {
        //EXCEPTION
        List<Member> findMembers = memberRepository.findByName(member.getName());
        if (!findMembers.isEmpty())
        {
            throw new IllegalStateException("이미 존재하는 회원입니다.");
        }
    }

    //회원 전체 조회
    public List<Member> findMembers() {
        return memberRepository.findAll();
    }

    //한 건 조회
    public Member findOne(Long memberId) {
        return memberRepository.findOne(memberId);
    }
}
```

+ Sample2: `ItemService`

```java
@Service
@Transactional(readOnly = true)
@RequiredArgsConstructor
public class ItemService {

    private final ItemRepository itemRepository;

    @Transactional
    public void saveItem(Item item) {
        itemRepository.save(item);
    }

    public List<Item> findItems() {
        return itemRepository.findAll();
    }

    public Item findOne(Long id) {
        return itemRepository.findOne(id);
    }
}
```


---

+ 출처
    + [실전! 스프링 부트와 JPA 활용1 - 웹 애플리케이션 개발](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-JPA-%ED%99%9C%EC%9A%A9-1/dashboard)
