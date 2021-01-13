---
title: Python) 정렬 방법
author: cotchan
date: 2021-01-14 08:45:21 +0800
categories: [Algorithm, Python]
tags: [python]     
---

아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
계속 업데이트할 예정입니다.

---

## 1. 내장함수 sorted()

+ 새로운 정렬된 리스트를 반환하는 `sorted()` 메서드

```python
>>> sorted([5, 2, 3, 1, 4])
[1, 2, 3, 4, 5]
```


---

## 2. list.sort()

+ 리스트를 제자리에서 수정합니다. (단, `None`을 리턴합니다.)
+ **list.sort() 메서드는 `리스트에서만 정의`됩니다.**

```python
>>> a = [5, 2, 3, 1, 4]
>>> a.sort()
>>> a
[1, 2, 3, 4, 5]
```

---

## 3. 정렬 KEY 정하기

+ `list.sort()`와 `sorted()`는 모두 비교하기 전에 각 리스트 요소에 대해 호출할 함수(또는 다른 콜러블)를 지정하는 `key` `매개 변수`를 가지고 있습니다. 

```python
def solution(n, costs):
    answer = 0
    edges = sorted([(x[2], x[0], x[1]) for x in costs]) # 정렬 KEY 지정
    parents = [i for i in range(n)] # [0,1,2,3,4,5,...,n-1]

    # ...
```

```python
>>> student_tuples = [
...    ('john', 'A', 15),
...    ('jane', 'B', 12),
...    ('dave', 'B', 10),
...]

>>> sorted(student_tuples, key=lambda student: student[2])   # sort by age
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

---

## 4. 오름차순/내림차순 방법

+ **`기본 정렬`은 `오름차순`입니다.**

+ **list.sort()와 sorted()는 모두 불리언 값을 갖는 `reverse` `매개 변수`를 받습니다.**
  + **내림차순 정렬을 지정하는 데 사용됩니다.**
  + 예를 들어, 학생 데이터를 역 age 순서로 얻으려면, 아래와 같이 합니다:


```python
>>> sorted(student_tuples, key=itemgetter(2), reverse=True)
[('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]
```

---

```python
>>> sorted(student_objects, key=attrgetter('age'), reverse=True)
[('john', 'A', 15), ('jane', 'B', 12), ('dave', 'B', 10)]
```

---

+ 출처
  + [정렬 HOW TO](https://docs.python.org/ko/3/howto/sorting.html)
