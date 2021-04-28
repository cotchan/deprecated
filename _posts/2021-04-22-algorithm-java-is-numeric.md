---
title: Java) String 숫자 판별 방법(bool isNumeric(String s))
author: cotchan
date: 2021-04-22 08:50:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. Java 숫자 숫자 판별 방법(Int)

+ **Java의 `Integer.parseInt`의 경우 입력받은 `String이 Integer가 아닌 경우 NumberFormatException을 발생`시킵니다.**

```java
boolean isNumeric(String target) {
  try {
    Integer.parseInt(target);
    return true;
  } catch (NumberFormatException e) {
    return false;
  }
}
```

---

## 2. Long, Double의 경우

+ **다른 Wrapper 클래스들도 같은 기능을 제공하니 아래 메서드를 통해 판별할 수 있습니다.**

```java
Long.parseLong(String s);
Double.parseDouble(String s);
```

---

+ 출처
  + [[Java] 숫자 판별하기](https://keichee.tistory.com/311) 
