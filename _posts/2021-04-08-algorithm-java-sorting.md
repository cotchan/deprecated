---
title: Java) Sorting
author: cotchan
date: 2021-04-08 08:21:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. 배열 정렬

---

## 1-1. Arrays.sort()

+ **`배열` 오름차순(기본) 정렬**

```java
import java.util.Arrays;

public class Foo {
  public static void main(String args[]) {


    // 문자열 배열 정렬 (가나다 순으로 소팅)
    String s[] = {  "맹구",
                  "배용준",
                  "땡칠이",
                  "장동건",
                  "강수정",
                  "송창식",
                  "황당해",
                  "고은아"};

    Arrays.sort(s);
    System.out.println(Arrays.toString(s));
    // 결과: [강수정, 고은아, 땡칠이, 맹구, 배용준, 송창식, 장동건, 황당해]






    // 숫자 배열 정렬
    double num[] = {               -1000,
                     0.07890264912715708,
                                     0.2,
                    -0.18441624291164838,
                                       0,
                                     123,
                                    -0.1,
                                    -0.1,
                                    1000,
                                  0.4999};

    Arrays.sort(num);
    System.out.println(Arrays.toString(num));
    // 결과: [-1000.0, -0.18441624291164838, -0.1, -0.1, 0.0, 0.07890264912715708, 0.2, 0.4999, 123.0, 1000.0]
  }
}
```

---

## 2. 리스트 정렬

---

## 2-1. Collections.reverse

+ **`리스트 역순 정렬`**

```java
Collections.reverse(List<?> list);
```

---

+ 출처
  + [java - 문자열, 숫자, 영문 배열 및 list 정렬(sort)](https://linuxism.ustd.ip.or.kr/964) 
  + [Sort List in reverse in order](https://stackoverflow.com/questions/18073590/sort-list-in-reverse-in-order)
