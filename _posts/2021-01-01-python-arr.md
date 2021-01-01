---
title: Python) 배열 관련 연산/기법 정리
author: cotchan
date: 2021-01-01 00:26:21 +0800
categories: [Algorithm, Python]
tags: [python]     
---

아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
계속 업데이트할 예정입니다.

---

## 1. 배열 초기화 방법

## 1-1. 빈 리스트 생성하기

```python
arr = []
```

---

## 1-2. 리스트 길이를 정하고 0으로 초기화하는 방법

```python
arr = [0 for loop in range(n)]
```

---

## 1-3. 0으로 초기화된 2차원 배열 만들기

```python
matrix = [[0 for col in range(n)] for row in range(n)]
```

---

## 2. 배열 내 최소, 최대 구하기(max,min,sum)

+ 파이썬 내장함수 - max(), min(), sum()
+ **`max` - 반복가능한 객체의 가장 큰 요소 값을 리턴**
+ **`min` - 반복가능한 객체의 가장 작은 요소 값을 리턴**
+ **`sum` - 반복가능한 객체 요소값의 합. (Default값: 0)**


---

+ 출처
    + [[Python] 길이가 정해진 리스트 만들기](https://jobc.tistory.com/141)
    + [[Python] max, min, sum 내장함수](https://pydole.tistory.com/entry/Python-max-min-sum-%EB%82%B4%EC%9E%A5%ED%95%A8%EC%88%98)
