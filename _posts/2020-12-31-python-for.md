---
title: Python for문
author: cotchan
date: 2020-12-31 00:26:21 +0800
categories: # [Algorithm, Python]
tags: [python]     
---

아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
계속 업데이트할 예정입니다.

---

## 1. for in 반복문

+ **문법**

```python
for item in iterable:
    # ...반복할 구문...
```

+ **사용예시**

```python
# example
for i in var_list:
    # ...

for i in var_dict:
    # ...

for i in var_set:
    # ...
```


---

## 2. range

+ range는 `range(시작숫자, 종료숫자, step)` 형태로 사용합니다.
+ range 범위는 c/c++의 for문과 동일하며 `[시작숫자, 종료숫자)` 범위를 가집니다.
+ **사용예시**

```python
# example
for i in range(0,9):
    print(i)

# 0
# 1
# 2
# 3
# 4
# 5
# 6
# 7
# 8
```

---

+ 출처
    + [19. for in 반복문, Range, enumerate](https://wikidocs.net/16045)
    + [2.3 for를 사용하는 반복문](https://wikidocs.net/58)
