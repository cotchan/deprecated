---
title: Design-pattern) Delegation Pattern
author: cotchan
date: 2021-02-18 11:16:21 +0800
categories: [Design-Pattern]
tags: [design-pattern]
---

+ **아래 출처글을 참고하여 작성한 글입니다.**
+ 개인 공부 목적으로 작성하였습니다.
+ 계속 업데이트할 예정입니다.

---

## 1. Delegation Pattern이란

+ 위임 패턴이란 Service Protocol을 매개로 Client와 Service Provider가 통신하는 방식입니다.
  + Client는 Service Protocol을 Call
  + Service Provider는 Service Protocol을 Implement

---

## 2. 주요 컴포넌트 

+ Delegate Pattern을 구성하는 3가지 요소입니다.

![Desktop View](/assets/img/post/jpa/design-pattern/2021-02-18-delegate-01.png)

1. **`Object Needing a Delegate` (위임자)**
  + **Delegate가 필요한 객체이므로 Delegate Protocol을 소유합니다.**
  + Client에 해당하는 코드로 해당 서비스를 사용하려는 쪽입니다.
2. **`Delegate Protocol`**
  + **Object Needing a Delegate가 필요한 부분을 정의합니다.**
3. **`Object Acting as a Delegate` (수신자)**
  + Delegate Protocol을 구현한 클래스입니다.

---

## 3. 컴포넌트들간의 관계

+ `Object Needing a Delegate` 와 `Delegate Protocol`
  + Dependency 관계
    + Object Needing a Delegate는 `weak property`로 delegate protocol을 소유함으로써 참조하지 않습니다.
    + 그러므로 Association 관계가 아니라 Dependency 관계를 가집니다.

+ `Delegate Protocol` 와 `Object Acting as a Delegate` 
  + Implement 관계

---

## 4. 동작 순서

1. Object Acting as a Delegate에서 Protocol에 해당하는 내용을 정의합니다.
2. Object Acting as a Delegate는 Object Needing a Delegate에게 `"나를 이용해"`라고 알려줍니다.
3. Object Needing a Delegate가 Object Acting as a Delegate의 메소드를 '대신' 실행해줍니다.
 
---

+ 출처
    + [Delegation Pattern](https://brunch.co.kr/@yoonms/20)
