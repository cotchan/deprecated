---
title: sb3) CompletableFuture
author: cotchan 
date: 2021-05-13 22:24:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. CompletableFuture란?

+ **함수형 프로그래밍 기법을 지원하는 비동기 병렬 프로그래밍 기법 지원**

+ **특징**
  + 명시적 Thread Pool 선언이 필요 없음
  + 함수형 프로그래밍 방식 지원
    + 간결한 코드, 높은 가독성
    + 각 병렬 TASK 들의 손쉬운 결합, 예외처리 지원

---

## 2. CompletableFuture 작업 기초

---

## 2-1. 반환값이 필요없을 때

+ **`runAsync`**

```java
static CompletableFuture<Void> runAsync(Runnable runnable)
```

---

## 2-2. 반환값이 필요할 때

+ **`supplyAsync`**

```java
static<U> CompletableFuture<U> supplyAsync(Supplier<U> supplier)
```

---

## 3. 비동기 작업 완료 콜백

---

## 3-1. .thenAcceept

+ **비동기 작업이 완료됐을 때 완료 결과를 소비(Consume) 처리함.**
+ 즉, `작업의 끝을 의미`합니다.

```java
CompletableFuture<Void> thenAccept(Consumer<? super T> action)
```

---

## 3-2. .thenApply

+ **비동기 작업이 완료됐을 때, `결과 T를` `새로운 값 U로 변환`하는 함수를 실행**
  + 결과값은 다시 `CompletableFuture`

```java
<U> CompletableFuture<U> thenApply(Function<? super T, ? extends U> fn)
```

---

## 3-3. .exceptionally

+ **`비동기 작업에서 예외가 발생했을 때`, 예외를 `Throwable`로 받고, 결과값 T를 생성**

```java
CompletableFuture<T> exceptionally(Function<Throwable, ? extends T> fn)
```

---

## 4. 많이 쓰는 사용 패턴 정리

---

## 4-1. allOf

+ 동시에 N개의 비동기 작업을 실행하고, 모든 작업이 완료되면 진행하기

```java
static CompletableFuture<Void> allOf(CompletableFuture<?>... cfs)
```

---


## 4-2. anyOf
 
+ 동시에 N개의 비동기 작업을 실행하고, 하나라도 작업이 완료되면 진행하기

```java
static CompletableFuture<Object> anyOf(CompletableFuture<?>... cfs)
```

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 

