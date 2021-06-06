---
title: Java) 자료형 변환(형변환) 모음
author: cotchan
date: 2021-04-08 08:20:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. String

+ **임의의 다른 자료형을 String으로 만들 때는 `String.valueOf(${targetValue})` 연산을 적용합니다.**

---

## 1-1. char[] to String

+ **`String.valueOf()`**
+ String.valueOf() 메소드에 매개변수로 `char[]`를 넣으면, `String`으로 바로 변환해줍니다.

```java
char[] ary = {'a','b','c','d','e'};
String arrayString = String.valueOf(ary);
System.out.println(arrayString);
```

---

+ **`String 생성자`**
+ char 배열을 `String 생성자`의 인자로 넣고 String을 생성하면 됩니다.

```java
public void charArrayToString1() {
    char[] charArray = { 'H', 'e', 'l', 'l', 'o', 'W', 'o', 'r', 'l', 'd' };
    String str = new String(charArray);
    System.out.println(str);
}
```

---

## 1-2. char to String

+ **`한 글자 char`에 대해서 `String 연산을 적용`해야 할 때**
+ **`String s = char c + "";`**

```java
//example1
char c = 'a';
String oneLetterString = c + "";
```

```java
//example2
for (int idx = 0; idx < source.length(); ++idx)
{
    String target = source.charAt(idx) + "";
    
    if (target.compareTo("-") == 0 || target.compareTo("_") == 0 || target.compareTo(".") == 0)
    {
        //...
    }
}
```

---

## 1. Long & Integer

---

## 1-1. Long to int

+ **`int Long.valueOf(${long value}).intValue();`연산을 통해서 형변환이 가능합니다.**

```java
Long oper = Long.parseLong(oper[2]);

int intValue = Long.valueOf(oper).intValue();
```

---

+ 출처
  + [자바 String을 Char로, Char를 String으로 변환하기](https://kutar37.tistory.com/entry/%EC%9E%90%EB%B0%94-String%EC%9D%84-Char%EB%A1%9C-Char%EB%A5%BC-String%EC%9C%BC%EB%A1%9C-%EB%B3%80%ED%99%98%ED%95%98%EA%B8%B0) 
  + [Java - Char 배열을 String으로 변환하는 방법](https://codechacha.com/ko/java-convert-chararray-to-string/)
