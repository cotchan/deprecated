---
title: Python) Call by Value? Call by Reference? 
author: cotchan
date: 2021-01-14 08:30:21 +0800
categories: [Algorithm, Python]
tags: [python]     
---

아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
계속 업데이트할 예정입니다.

---

## 1. 결론

+ 어떤 값을 전달하느냐에 따라 달라집니다.
  + **`Immutable`(불변)한 객체를 넘긴다면 Call by Value 입니다.**
    + ex. `int`, `str` 등
  + **`Mutable`(가변)한 객체를 넘긴다면 Call by Refernece 입니다.**
    + ex. `list`, `dictionary` 등
  + 이러한 파이썬의 방식을 `passed by assignment`라고 합니다.

---

+ **이와 같은 방식이 가능한 이유는 파이썬에서는 `모든 것이 객체`이기 때문입니다.**


---

+ 출처
  + [Python 은 call-by-value 일까 call-by-reference 일까](https://www.pymoon.com/entry/Python-%EC%9D%80-callbyvalue-%EC%9D%BC%EA%B9%8C-callbyreference-%EC%9D%BC%EA%B9%8C)
