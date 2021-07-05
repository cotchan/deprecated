---
title: Java) DateFormat ("yyyy-MM-dd HH:mm" 표기 방법)
author: cotchan
date: 2021-07-05 11:21:00 +0800
categories: [Java]
tags: [java]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Java 8+

+ **Java 8 이상에서는 `DateTimeFormatter`를 사용합니다.**

```java
//example

LocalDateTime ldt = LocalDateTime.now();
System.out.println(DateTimeFormatter.ofPattern("MM-dd-yyyy", Locale.ENGLISH).format(ldt));
System.out.println(DateTimeFormatter.ofPattern("yyyy-MM-dd", Locale.ENGLISH).format(ldt));
System.out.println(ldt);
```

---

```java
//output

05-11-2018
2018-05-11
2018-05-11T17:24:42.980
```

---

## 2. Java 7-

+ **Java 7이하에서는 `SimpleDateFormat`을 사용합니다.**

```java
//example

public static String getCurrentTime() {
    SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm");
    return simpleDateFormat.format(System.currentTimeMillis());
}
```

---

```java
//output

2021-07-05 12:35
```

---

```java
//example2

Date myDate = new Date();
System.out.println(myDate);
System.out.println(new SimpleDateFormat("MM-dd-yyyy").format(myDate));
System.out.println(new SimpleDateFormat("yyyy-MM-dd").format(myDate));
System.out.println(myDate);
```

---

```java
//output2

Wed Aug 28 16:20:39 EST 2013
08-28-2013
2013-08-28
Wed Aug 28 16:20:39 EST 2013
```

---

+ 출처
  + [Java SimpleDateFormat 으로 날짜/시간/요일 표현하기](https://moonong.tistory.com/16)
  + [java.util.Date format conversion yyyy-mm-dd to mm-dd-yyyy](https://stackoverflow.com/questions/18480633/java-util-date-format-conversion-yyyy-mm-dd-to-mm-dd-yyyy/18480709)
