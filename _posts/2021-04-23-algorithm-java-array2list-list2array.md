---
title: Java) Array2List, List2Array 변환 방법
author: cotchan
date: 2021-04-23 08:37:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. 변환 시 유의사항

+ **int와 같은 `Primitive 타입의 배열`과 Integer와 같은 `Wrapper 타입의 List`를 서로 바꿔줄 수는 없습니다.**

---

## 1. Array 2 List

+ **`Arrays.asList(Object[])`**

```java
ArrayList 리스트명 = new ArrayList<>(Arrays.asList(배열명));
```

---

```java
Integer arr[] = {1,2,3,4}; // 배열 생성
		
// 배열을 List로 변환
ArrayList<Integer> list = new ArrayList<>(Arrays.asList(arr));

String arr2[] = {"ab","ac","ad","ae"}; // 배열 생성

// 배열을 List로 변환
ArrayList<String> arrayList = new ArrayList<String>(Arrays.asList(arr2));
```

---

## 1. List 2 Array

+ **`리스트명.toArray(new 데이터타입[size])`**

```java
배열명 = 리스트명.toArray(new 데이터타입[리스트명.size()]);
```

---

```java
Integer arr[] = {1,2,3,4}; // 배열 생성
		
// 배열을 List로 변환
ArrayList<Integer> list = new ArrayList<>(Arrays.asList(arr));
		
// List를 배열로 변환
arr = list.toArray(new Integer[list.size()]);
```


---

+ 출처
  + [자바 배열을 리스트로, 리스트를 배열로 변환방법](https://wakestand.tistory.com/183) 
