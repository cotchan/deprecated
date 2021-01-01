---
title: Python) 실수 연산 (round, format 등) 
author: cotchan
date: 2021-01-01 00:26:21 +0800
categories: [Algorithm, Python]
tags: [python]     
---

아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
계속 업데이트할 예정입니다.

---

## 1. 소숫점 자릿수 조절(소수점 올림, 반올림)

## 1-1. round(실수, n)

+ **반올림**
    + 소수점을 `n번째까지만 표현`하고 반올림을 하고 싶을 때 `round `함수를 사용합니다.

```python

>>> n = 7/15
>>> n
0.4666666666666667

>>> round(n,2)
0.47

>>> round(n,4)
0.4667

>>> round(n)
0

```

---

### 1-2. ceil, floor, trunc (올림, 내림, 버림)

+ 올림, 내림, 버림을 하기 위해서는 `math` 클래스안의 `ceil`, `floor`, `trunc` 함수를 사용합니다.

```python
import math

>>> math.ceil(12.2)
13
>>> math.ceil(12.6)
13
>>> math.floor(12.2)
12
>>> math.floor(12.6)
12
>>> math.trunc(12.2)
12
>>> math.trunc(12.6)
12
```
---

## 2. 부동 소수점 자릿수 제한

## 2-1. format() 함수

+ **소수점 이하를 n번째 자리까지만 표현하고 싶을 때 format 함수를 사용합니다.**
    + `.2f` 이면 소수점이하 `둘째 자리`까지 출력
    + `.3f` 이면 소수점이하 `셋째 자리`까지 출력


```python
a = 13.9499999999999
a = format(a, '.2f')
print(a)

# 13.95
```




---

+ 출처
    + [[Python]파이썬 자리수 조절(소수점,올림,반올림)](https://dpdpwl.tistory.com/94)
    + [부동 소수점을 두 개의 소수점으로 제한](https://c10106.tistory.com/2015)
