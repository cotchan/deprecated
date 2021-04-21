---
title: Java) char에 대한 아스키 코드 구하는 법
author: cotchan
date: 2021-04-22 08:20:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. JAVA 아스키 코드 연산 방법

+ 주어진 문자가 알파벳 대소문자인지는 아스키 코드를 비교해서 간단하게 구할 수 있습니다.
+ ****`정수 자료형에 char 자료형을 대입하면` char에 대한 아스키 코드 값을 구할 수 있습니다.**
  + + **`int i = char c`**

```java
//example 1
char alphabet = 'k';
int alphabetASCII = alphabet;
```

---

```java
//example 2
boolean isLowerAlphabet(char target) { 

  int targetASCII = target
  int lowerBound = 'a';
  int upperBound = 'z';
  
  return (lowerBound <= targetASCII && targetASCII <= upperBound);
}

```

---

+ 출처
  + [자바 String을 Char로, Char를 String으로 변환하기](https://kutar37.tistory.com/entry/%EC%9E%90%EB%B0%94-String%EC%9D%84-Char%EB%A1%9C-Char%EB%A5%BC-String%EC%9C%BC%EB%A1%9C-%EB%B3%80%ED%99%98%ED%95%98%EA%B8%B0) 
  + [Java - Char 배열을 String으로 변환하는 방법](https://codechacha.com/ko/java-convert-chararray-to-string/)
