---
title: Web) Timeout이란?
author: cotchan
date: 2021-06-09 10:00:00 +0800
categories: [Web, Web_Basic]
tags: [web]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ 계속 업데이트 할 예정입니다.

---

## 1. Timeout

+ Timeout의 사전적 의미는, **'프로그램이 특정한 시간 내에 성공적으로 수행되지 않아서 진행이 자동적으로 중단되는 것'.**
+ 클라이언트 입장에서 응답을 무한정 기다릴 수 없기 때문에 기다릴 시간을 정해야 합니다.

---

## 2. Timeout 사용 사례

+ Socket(양방향 통신), Http(단방향 통신)에서 다양하게 활용합니다.

---

## 2-1. Socket (양방향 통신)

+ 예시: 채팅 프로그램
  + 채팅 프로그램에서 서버로부터 특정 시간 응답이 없을 때 => `Socket Timeout` 발생

---

## 2-2. Http (단방향 통신)

+ 예시: WEB
  + 클라이언트가 서버로 request를 보낸 후 연결이 되지 않은 상태로 특정시간 이상 대기 => `Connection Timeout` 발생

---

## 3. Socket Timeout

+ **클라이언트와 서버가 연결된 후에 서버는 데이터를 클라이언트에게 전송하게 됩니다. 이 때 하나의 데이터 덩어리가 아닌 여러개의 패킷으로 나눠 전송하게 되는데, `각 패킷이 전송될 때의 시간 차이`가 있습니다. `이 시간 차이의 제한(임계치)`을 `SocketTimeout`이라고 합니다.**

---

## 4. Connection Timeout

+ **클라이언트가 서버측으로 connection을 맺길 원하지만 서버의 장애 상황으로 `connection 조차 맺어지지 못할 때 발생하는 Timeout` 입니다.**
  + **즉, Connection Timeout이 발생했다면 서버로부터 `응답 코드`를 받지 못합니다.**
  + **그러므로 보통은 네트워크를 사용하는 `클라이언트 라이브러리에서 Exception을 발생시킵니다.`**

---

+ 출처
  + [Timeout에 관한 정리](https://effectivesquid.tistory.com/entry/Timeout%EC%97%90-%EA%B4%80%ED%95%9C-%EC%A0%95%EB%A6%AC)
  + [[네트워크]타임아웃(Timeout) 정리](https://tyrionlife.tistory.com/790)
