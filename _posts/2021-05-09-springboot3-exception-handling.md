---
title: sb3) 예외 처리 가이드
author: cotchan 
date: 2021-05-09 17:00:21 +0800 
categories: [Spring-Boot3]
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. intro

+ Java에서는 개발자가 예측할 수 없는 케이스가 몇 가지 있습니다.
  + **`네트워크 처리`나 `File I/O`가 발생하는 경우**
    + `Connection Exception`, `IO Exception` 
  + 얘네들은 예상할 수 없는 예외를 던지는 애들입니다.
+ 그래서 Java에서 어떤 라이브러리를 쓰든간에 네트워크 처리 혹은 File I/O가 있다면 위 Exception이 발생할 수 있습니다.

---

## 2. Checked vs UnChecked

+ **반드시 throws를 하거나 try-catch를 해줘야 하는 애들을 `Checked-Exception`이라고 합니다.**
  + 예시
    + **`SQLException`**
    + **`IOException`**

+ **반면에 `UnChecked-Exception`은 throws를 하거나 try-catch를 강제하지 않습니다.**
  + **그러므로 함수에서는 throws를 하고 있는데, 따로 처리를 해주지 않아도 컴파일 에러가 발생하지 않습니다.**

<img width="697" alt="스크린샷 2021-05-09 오후 5 01 03" src="https://user-images.githubusercontent.com/75410527/117564690-41eec380-b0e8-11eb-8e10-b5fae4efa01a.png">

---

## 3. Checked보다는 UnChecked

+ 요새는 RuntimeException이 보편화되고 있는 추세입니다.
+ **그 이유는 사실 Checked-Exception을 캐치해도 별로 할 게 없기 때문입니다.**
  + **예외는 보통 복구가 불가능한 상황에서 발생하고, catch 구문에서 로그를 남기는 것 외에 `딱히 할 수 있는 것이 없습니다.`**
+ 즉, 현실적으로 대부분의 Checked-Exception은 캐치해도 할 수 있는 게 없습니다.
  + ex. "네트워크가 끊겼다?"
    + **`보통은 warning이나 error 레벨로 로그를 남기는 게 전부`**

+ Checked-Exception을 남발하게 되면 해당 코드의 사용자에게 try-catch 구문을 강제하게 됩니다.
  + 이는 코드 사용자로 하여금 기계적으로 예외를 catch하게 만들고 올바른 프로그램 작성을 방해합니다.

+ 그래서 대부분 UnChecked를 사용하고, 너네가 관심있는 것들은 알아서 try-catch로 처리해라라는 뉘앙스입니다.

---

## 4. Checked-Exception 장점

+ **컴파일이 안 된 부분을 통해 예외 처리를 해야함을 알 수 있습니다.**

---

## 5. UnChecked-Exception 단점

+ **예외가 발생하는지 아닌지 `함수 시그니처를 봐야 알 수 있습니다.`**
 

---

## 6. 레이어에 맞는 예외 발생시키기

+ **데이터베이스 관련 예외는 Repository 레이어에서 발생시켜야합니다.**
  + **예시로, java.sql 패키지의 Exception이 Service 레이어나 Controller 레이어에서 등장하면 안됩니다.** 
  + java.sql 패키지는 Repository 레이어에서만 사용해라와 같은 맥락입니다.
+ **서비스 수준의 예외는 Service 레이어에서 발생시켜야합니다.**
  + 서비스 레이어에서는 `SQLException이 아니라` `비즈니스적 의미를 갖는 익셉션을 직접 정의해주고 그걸 처리하도록` 하는 게 좋습니다.

---

## 7. 예외 처리의 최소치

+ **예외를 캐치했으면 먹지 말고 `최소 로그정보는 남겨야 합니다.`**
+ 즉, 반드시 로그를 남기고, 필요한 조치를 수행해야 합니다.



---

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
