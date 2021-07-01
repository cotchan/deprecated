---
title: Java) 문자열 뒤집기(String Reverse)
author: cotchan
date: 2021-06-30 09:07:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. StringBuilder 사용

```java
for (int i = 0; i < words.length; ++i)
{
	String word = words[i];
	String reverseWord = new StringBuilder(word).reverse().toString();
}
```

---

+ 출처
  + []()
