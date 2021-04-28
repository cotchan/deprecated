---
title: JDBC) Result-Set
author: cotchan
date: 2021-04-29 08:29:00 +0800
categories: [Java]
tags: [jdbc]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Result-Set

+ **`execteQuery로 명령하면` `ResultSet`이라는 객체를 돌려주게 됩니다.**
+ **ResultSet은 `DB에 보낸 명령(쿼리)에 대한 반환값`이라고 보면 됩니다.**
  + `execteQuery` : DB에 명령
  + `ResultSet` : 명령에 대한 반환값.

---

## 2. JDBC의 DB요청 처리 방법

+ 자바에서 사용하는 것은 일단 DB로 연결되는 통로를 열고, 그 통로로 명령을 보내고,
+ 그 통로로 보낸 명령에 대해서 DB가 처리해서 값을 돌려보내주는 식으로 처리 됩니다.
  + **간단히 말하면 DB에 명령을 내리는 것입니다.**
  + **그러면 그 명령에 따라서 디비가 작동하고, 작동한 결과 값을 돌려줍니다.**

---

+ 출처
  + [ResultSet 이란??](https://whdvy777.tistory.com/entry/ResultSet-%EC%9D%B4%EB%9E%80)
