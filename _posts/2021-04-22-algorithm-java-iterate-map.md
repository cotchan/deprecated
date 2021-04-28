---
title: Java) Map, HashMap 순회하는 방법
author: cotchan
date: 2021-04-22 08:36:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. Map을 순회하는 3가지 방법

---

## 1-1. entrySet() 사용

+ **entrySet()은 `key 와 value 두 개 모두가 필요할 경우 사용`합니다.**

```java
Map <String, Object> map = new HashMap();

//...

for (Map.Entry<String, Object> entry : map.entrySet()) {
  String key = entry.getKey();
  Object value = entry.getValue();
  // 어떤 작업...
  }
```

---

## 1-2. keySet() 사용 

+ **keySet() 은 `key 값만 필요할 경우 사용`합니다.**

```java
Map <Object, Object> map = new HashMap();

//...
for (Object key : map.keySet()) {
  // 어떤 작업...
}

```

---

## 1-3. values() 사용

+ **values() 는 `value 값만 필요할 경우에 사용`합니다.**

```java
Map <String, Object> map = new HashMap();

//...
for (Object value: map.values()) {
  // 어떤 작업
}
```

---

+ 출처
  + [Map, HashMap 순회하기](https://starblood.tistory.com/entry/Map-HashMap-%EC%88%9C%ED%9A%8C%ED%95%98%EA%B8%B0) 
