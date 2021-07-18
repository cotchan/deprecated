---
title: TIL) VO vs DTO
author: cotchan
date: 2021-07-18 20:39:00 +0800
categories: #
tags: [til]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 문제

+ **팀원간의 `VO`와 `DTO`에 대한 단어 해석이 달라서 이에 대한 개념을 정리한다.**

---

## 2. VO

+ **VO(`Value Object`)는 `값 그 자체를 표현하는 객체`이다.**
  + **예시: Email, Address 등 기본 자료형 외의 나타내고자하는 자료형을 표현할 때 사용한다.**

+ 로직을 포함할 수 있으며, 객체의 불변성(객체의 정보가 변경하지 않음)을 보장한다.

+ 서로 다른 이름을 갖는 VO 인스턴스더라도 모든 속성 값이 같다면 두 인스턴스는 같은 객체라고 할 수 있다. 
  + **이를 위해 VO에는 Object 클래스의 `equals()`와 `hashCode()`를 오버라이딩해야 한다.**

---

## 3. DTO

+ **DTO(`Data Transfer Object`)로서 `계층(Layer)간 데이터 교환을 위해 사용하는 객체`이다.**

+ **`데이터 교환만을 위해 사용하`므로 로직을 갖지 않고, getter/setter 메소드만 갖는다.**

+ 일반적으로 Controller Layer와 가장 큰 연관성을 갖는다. 
  + **그 이유는 `Service Layer`로부터받은 `비즈니스 로직의 결과물을 클라이언트에게 결과로 내려주기 위해 사용`한다.**

```java
//example

class RGBColorDto {
   private int red;
   private int green;
   private int blue;
  
   public RGBColor(int red, int green, int blue) {
      this.red = red;
      this.green = green;
      this.blue = blue;
   }
  
   public int getRed() {
      return red;
   }
  
   public int setRed(int red) {
      this.red = red;
   }
   ...
}
```
---

+ 출처
  + [DTO와 VO 그리고 Entity의 차이](https://youngjinmo.github.io/2021/04/dto-vo-entity/)
