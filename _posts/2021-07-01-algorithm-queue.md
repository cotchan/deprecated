---
title: Java) Queue 관련 내용(우선순위큐 포함)
author: cotchan
date: 2021-07-01 17:53:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. PriorityQueue

+ 자바의 PQ는 기본적으로 `최소힙`입니다.

---

## 1-1. Collections.reverseOrder()

+ **PQ내 기본 정렬 순서를 바꿔주고 싶다면 `Collections.reverseOrder()` 메서드를 인수로 넣어주면 됩니다.**

```java
static PriorityQueue<Integer> pq = new PriorityQueue<>(Collections.reverseOrder());
```
 



---

+ 출처
  + []()
