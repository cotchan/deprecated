---
title: Spring-Boot) 테스트 케이스 작성 기본 개념 
author: cotchan 
date: 2020-12-14 00:40:21 +0800 
categories: [Spring-Boot, Spring-Boot_Test]
tags: [spring-boot] 
---

## Intro

자바는 `JUnit`이라는 프레임워크로 테스트를 실행합니다.    
**테스트는 각각 독립적으로 실행되어야 합니다. 테스트 순서에 의존관계가 있는 것은 좋은 테스트가 아닙니다.**

---

## 1. src/test/java 하위 폴더에 생성


---

## 2. @Test

@Test 어노테이션을 붙임으로써 특정 메서드를 테스트 메서드로 만들 수 있습니다.

```java
class MemberServiceTest {

    @Test
    void 회원가입() {
        //...
    }
}
```
---

## 3. Assertions

**Assertions 클래스는 `org.assertj.core.api.Assertions`를 일반적으로 사용합니다.**          
그리고 `Assertions.assertThat`을 통해 테스트 결과를 검증합니다.    

```java
import org.assertj.core.api.Assertions;


Assertions.assertThat(result).isEqualTo(member);

//static import를 사용하면
import static org.assertj.core.api.Assertions.*;

//아래와 같은 형태로 사용할 수 있습니다.
assertThat(member).isEqualTo(result);
```    


---


## 4. given, when, then

**테스트를 위의 `3단계로 개념화`하여 나누고 작성하면 더 직관적인 테스트 코드를 만들 수 있습니다.**  

1. given: 뭔가가 주어졌을 때(테스트 전 필요한 선행 조건을 의미)
2. when: 이걸 실행했을 때 (테스트 하고자 하는 로직을 의미)
3. then: 어떤 결과가 나와야 한다 (Green Case 의미)

```java
class MemberServiceTest {

    MemberService memberService;;
    MemoryMemberRepository memberRepository;

    @Test
    void 회원가입() {
        //given: 뭔가가 주어졌을 때
        Member member = new Member();
        member.setName("hello");

        //when: 이걸 실행했을 때
        Long saveId = memberService.join(member);

        //then: 결과가 이게 나와야 한다.
        Member findMember = memberService.findOne(saveId).get();
        assertThat(member.getName()).isEqualTo(findMember.getName());
    }

    @Test
    public void 중복_회원_예외() {
        //given
        Member member1 = new Member();
        member1.setName("spring");

        Member member2 = new Member();
        member2.setName("spring");


        //when
        memberService.join(member1);

        //arg2의 로직(LAMDA)을 수행할 때, arg1의 예외가 터져야 합니다.
        IllegalStateException e = assertThrows(IllegalStateException.class, 
					       () -> memberService.join(member2));

        assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");

//        try {
//            memberService.join(member2);
//            fail();
//        } catch (IllegalStateException e) {
//            assertThat(e.getMessage()).isEqualTo("이미 존재하는 회원입니다.");
//        }
        
        //then
    }
}
```


---

## 5. @BeforeEach, @AfterEach

+ @BeforeEach
	+ **각 테스트가 실행되기 전에 호출됩니다.**
	+ 예시코드에서는 테스트가 서로 영향이 없도록 항상 새로운 객체를 생성하고 의존관계를 맺어줍니다.              

+ @AfterEach
	+ **각 테스트가 종료될 때 마다 호출됩니다.**
	+ 예를 들면, 한 번에 여러 테스트를 실행하면 메모리 DB에 직전 테스트의 결과가 남을 수 있습니다. 이렇게 되면 이전 테스트 때문에 다음 테스트가 실패할 가능성이 있습니다.

```java
class MemberServiceTest {

    MemberService memberService;;
    MemoryMemberRepository memberRepository;

    @BeforeEach
    public void beforeEach() {
        memberRepository = new MemoryMemberRepository();
        memberService = new MemberService(memberRepository);
    }

    @AfterEach
    public void afterEach() {
        memberRepository.clearStore();
    }
}
```


---

+ 출처
	+ [스프링 입문 - 코드로 배우는 스프링 부트, 웹 MVC, DB 접근 기술](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-%EC%9E%85%EB%AC%B8-%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8/dashboard)
