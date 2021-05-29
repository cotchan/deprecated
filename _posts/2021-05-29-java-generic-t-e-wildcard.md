---
title: Java) 자바 제네릭 ?, T, E, List의 차이
author: cotchan
date: 2021-05-29 19:07:00 +0800
categories: [Java]
tags: [java]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. ? (wildcard)

+ **모든 element를 `Object로 간주`합니다.**
+ **그런데 raw type과 차이점이 있다면 `add()가 막혀있습니다.`**
  + 그래서 `null`만 넣을 수 있습니다.
  + 그래서 이상한 타입의 element가 리스트에 추가되는걸 막아줍니다. 덕분에 안전합니다.
+ **또한, `List<? extends Number>` 이런 식으로 `범위를 한정지을 수 있습니다.`** 

---

## 2. T, E

+ **두 가지는 type parameter 이름이 다를 뿐 본질적으로 같습니다.**
  + Element의 `E는 List 내의 원소`를 나타내는데 적합합니다. 
  + 그러므로 배열 기반으로 되어 있는 구조에서 제네릭을 적용할 때는 E가 의미상 맞습니다.
  + 그 외의 경우는 T가 맞습니다.
+ **기능은 wildcard랑 거의 비슷합니다.**
+ **차이점이 있다면 `메소드나 클래스 내부에서` `T`, `E`같은 `type parameter를 참조할 수 있다는 것` 뿐입니다.**

---

## 3. List

+ **보통 `raw type`이라고 부릅니다.**
  + 이렇게 쓰면 안 됩니다.
+ 그 이유는 리스트에 `새로운 element 넣을 때 타입체크가 안 되서` 문제생길 여지가 아주 많습니다.
+ 참고로 primitive 타입을 넣을 수 없습니다. 
  + 그러므로 primitive 타입을 넣으려면 Integer와 같은 wrapper 클래스를 써야합니다. 
  + 만약 컴파일상 문제가 없더라도 실질적으로는 auto-boxing 되어서 들어갑니다.

---

+ 출처
  + [자바에서 List, List <?>, List <T>, List <E> , List <Object> 차이가 무엇인가요?](https://okky.kr/article/514701?note=1545259)
  + [java generic에서 E와 T 차이](https://lng1982.tistory.com/70)
