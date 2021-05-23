---
title: sb3) JWT 적용을 위한 Spring Security 커스터마이징
author: cotchan 
date: 2021-05-23 18:02:21 +0800 
categories: [Spring, Spring_Security]
tags: [spring-security] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 핵심 컴포넌트 분석 

+ **Spring Security 커스터마이징을 위한 핵심 컴포넌트 분석**

---

## 1-1. UsernamePasswordAuthenticationFilter

+ **`UsernamePasswordAuthenticationFilter`**
  + HTTP 요청에서 사용자 ID, 비밀번호 추출
  + UsernamePasswordAuthenticationToken을 생성해, AuthenticationManager를 호출

---

## 1-2. UsernamePasswordAuthenticationToken

+ **`UsernamePasswordAuthenticationToken`**
  + 사용자 인증 요청을 추상화한 Authentication 인터페이스 구현체
  + principal 멤버 변수는 2개의 의미로 사용
    + 인증 전: 사용자 ID
    + 인증 후: 사용자 도메인 모델

---

## 1-3. DaoAuthenticationProvider

+ **`DaoAuthenticationProvider`**
  + UserDetailsService를 통해 사용자 정보를 데이터베이스에서 조회
  + 실질적인 사용자 인증 처리 로직을 수행함
  + 인증 결과는 UsernamePasswordAuthenticationToken 타입으로 변환

---

## 2. Spring Security 커스터마이징 구현
 
---

## 2-1. AuthenticationRestController

+ **`AuthenticationRestController` (UsernamePasswordAuthenticationFilter 역할)**
  + HTTP 요청에서 사용자 ID, 비밀번호 추출
  + JwtAuthenticationToken을 생성해, AuthenticationManager를 호출

---

## 2-2. JwtAuthenticationToken

+ **`JwtAuthenticationToken` (UsernamePasswordAuthenticationToken 역할)**
  + 사용자 인증 요청을 추상화한 Authentication 인터페이스 구현체
  + principal 멤버 변수는 2개의 의미로 사용
    + 인증 전: 사용자 ID
    + 인증 후: `JwtAuthentication` (사용자 PK, 이메일)

---

## 2-3. JwtAuthenticationProvider

+ **`JwtAuthenticationProvider` (DaoAuthenticationProvider 역할)**
  + UserService를 통해 사용자 정보를 데이터베이스에서 조회
  + 실질적인 사용자 인증 처리 로직을 수행 및 JWT 생성
  + 인증 결과는 JwtAuthenticationToken 타입으로 변환

---

## 2-4. JwtAuthenticationTokenFilter

+ **`JwtAuthenticationTokenFilter`**
  + 필터 체인에서 UsernamePasswordAuthenticationFilter 앞에 위치합니다.
  + HTTP 요청에서 JWT 추출하고 유효성 검증
  + JWT 인증 결과는 JwtAuthenticationToken 타입으로 SecurityContextHolder에 저장 됨
    + REST 컨트롤러의 @AuthenticationPrincipal 어노테이션은 JwtAuthenticationToken.principal을 참조함
    + JwtAuthenticationToken.principal은 JwtAuthentication과 같음

---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 

