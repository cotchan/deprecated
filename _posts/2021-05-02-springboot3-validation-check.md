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

+ **`원칙`: 기본적으로 `사용자의 모든 입력값은 Validation Check`가 이뤄져야 합니다.**

---

## 2. Validation Check 라이브러리 

+ Guava Preconditions(checkNull, checkArgument) 등 많습니다.
- 뭘 써야하는지는 중요치 않습니다.
- 중요한 것은 Validation Check를 제대로 했는지가 중요합니다.

---

## 3. Validation은 어디에서?

- 원칙적으로 `Controller`, `Service`, `Repository` Layer 모두에서 입력값 검증을 수행해야 합니다.
  - 그러나 비슷한 검증 로직이 반복되고, 관리가 어려워집니다.
  - 비슷한 로직이 반복되며 성능한 비효율적입니다.

---

## 4. Validation 우선순위 Layer

+ **`Service Layer`와 `Domain Model의 생성자`**
  + 이거는 Validation의 최소 필수치입니다.

+ Layer 별 우선순위를 정하자면 
  + **`최소 Service Layer에서는 Validation Check`을 합니다`**
  + **그리고 필요하다면 Controller Layer에서도 Validation Check를 합니다.**
    - 이유: Service는 Controller가 호출하기도 하지만, 다른 Service가 Service를 호출하기 때문입니다.

---

## 5. Validation Check 우선순위 정리

+ 1st. `Model의 생성자`에서 인자들에 대한 Validation
+ 2nd. `Service Layer`에서의 Validation
+ 3rd. Controller Layer

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
