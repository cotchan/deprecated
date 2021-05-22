---
title: sb3) Validation Check 
author: cotchan 
date: 2021-05-02 19:34:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. Intro

+ **Validation Check의 기본 원칙은 `사용자의 모든 입력값은 Validation Check`가 이뤄져야 합니다.**

---

## 2. Validation Check 라이브러리 

+ Guava Preconditions(checkNull, checkArgument) 등 많습니다.
- 뭘 써야하는지는 중요치 않습니다.
- 중요한 것은 Validation Check를 제대로 했는지가 중요합니다.

---

## 3. Validation은 어디에서?

+ **아래 세 군데에서 Validation Check를 하면 됩니다.**
  1. **`생성자`**
  2. **`내부 필드를 변경하는 모든 setter`**
  3. **`Service Layer`**

+ 여기에 전부 Validation 코드를 넣으면 됩니다.
+ **생각할 수 있는 모든 비관적인 케이스를 넣으면 됩니다.**
+ **Validation Check 코드는 안 넣는 것보다 중복되는 게 낫습니다.**

- 원칙적으로 `Controller`, `Service`, `Repository Layer` 모두에서 입력값 검증을 수행해야 합니다.
  - 그러나 비슷한 검증 로직이 반복되고, 관리가 어려워집니다.
  - 비슷한 로직이 반복되며 성능한 비효율적입니다.

---

## 4. Validation 우선순위 Layer

+ **`Service Layer`와 `Domain Model의 생성자`**
  + 이 두 곳은 Validation Check가 이뤄져야 할 최소영역이자 필수영역입니다.

+ Layer 별 우선순위를 정하자면 
  + **`최소 Service Layer에서는 Validation Check`을 합니다.**
  + **그리고 필요하다면 Controller Layer에서도 Validation Check를 합니다.**
    - 이유: Service는 Controller가 호출하기도 하지만, 다른 Service가 Service를 호출하기 때문입니다.

---

## 5. Validation Check 우선순위 정리

+ 1st. `Model의 생성자`에서 인자들에 대한 Validation
+ 2nd. `Service Layer`에서 Validation
+ 3rd. `Controller Layer`에서 Validation

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
