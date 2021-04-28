---
title: Java) equals와 hashCode의 관계
author: cotchan
date: 2021-04-24 22:44:00 +0800
categories: [Java]
tags: [java]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ 계속 내용을 업데이트 할 예정입니다.

---

## 1. equals와 hashCode의 관계

+ **`equals가 리턴값이 true이면` `hashcode의 결과값도 반드시 같아야 합니다.`**

```java
User a;
User b;

//만약, a.equals(b) == true 라면, a.hashCode() == b.hashCde() 값이 같아야 합니다.
```

---

+ **그러나 `이 반대는 성립하지 않습니다.`**
  + hashCode 값이 같다고 equals가 true인 것은 X

+ 객체가 같은 객체라면 hashCode 같아야 합니다.

+ 그러나 이 반대는 `해시충돌이 발생할 수 있으므로` 성립하지 않아도 됩니다.
  + 같은 해시코드 객체라고 같은 객체 인게 X

---

## 2. hashCode를 구현해야 하는 경우

+ User Type 타입을 `컬렉션에 넣을 때` (Map이나 Set에)

+ equals, hashCode 구현여부에 따라 버그가 발생할 수 있습니다.

+ **Java는 `같은 객체`라는 것을 `hashCode를 통해 비교`합니다.**
  + 그러므로 hashCode를 제대로 정의해놓지 않으면 문제가 생길 수 있습니다.
