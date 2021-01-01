---
title: Python) 문자열 사용법(Reverse, find, lower/upper) 
author: cotchan
date: 2021-01-01 00:26:21 +0800
categories: [Algorithm, Python]
tags: [python]     
---

아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
계속 업데이트할 예정입니다.

---

## 1. 문자열 뒤집기([::-1])

+ **`[::-1]`**

```python
print 'Reverse this strings'[::-1]

# sgnirts siht esreveR
```

```python
input = ['123','456']

num1 = input[0][::-1]
num2 = input[1][::-1]

print(num1)
print(num2)

# 321
# 654
```

---

## 2. 대소문자 변환하기

## 2-1. 대문자 변환(upper)

+ 대문자 변환 메소드는 `3가지`가 있습니다. 

```python
string.upper()
string.capitalize()
string.title()
```

+ **`upper`**
    + 주어진 문자열에서 모든 알파벳들을 대문자로 변환시킵니다.
+ **capitalize**
    + 주어진 문자열에서 맨 첫 글자를 대문자로 변환시킵니다.
+ **title**
    + 주어진 문자열에서 알파벳 외의 문자(숫자, 특수기호, 띄어쓰기 등)로 나누어져 있는 영단어들의 첫 글자를 모두 대문자로 변환시킵니다.

---

## 2-2. 소문자 변환(lower)

+ **`lower`**

```python
string.lower()
``` 

---

## 3. 특정문자 찾기

## 3-1. find

+ **`find(찾을 문자, 찾기 시작할 위치)`** 
    + find는 문자열 중에 특정문자를 찾고 위치를 반환해준다. **없을 경우 `-1`을 리턴**

```python
>>> s = '가나다라 마바사아 자차카타 파하'
>>> s.find('마')
5
>>> s.find('가')
0
>>> s.find('가',5)
-1
```

---

## 3-2. startwith

+ **`startswith(시작하는 문자, 시작지점)`**
    + startswith은 문자열이 특정문자로 시작하는지 여부를 알려줍니다. `true/false` 반환
    + 두 번째 인자를 넣음으로써 찾기 시작할 지점을 정할 수 있습니다.

```python
>>> s = '가나다라 마바사아 자차카타 파하'
>>> s.startswith('가')
True
>>> s.startswith('마')
False

>>> s.startswith('마',s.find('마')) #find는 '마' 의 시작지점을 알려줌 : 5
True
>>> s.startswith('마',1)
False
```


---

## 3-3. endswith

+ **`endswith(끝나는 문자, 문자열의 시작, 문자열의 끝`**
    + endswith은 문자열이 특정문자로 끝나는지 여부를 알려줍니다. `true/false` 반환
    + 두번째인자로 문자열의 시작과 세번째인자로 문자열의 끝을 지정할 수 있습니다.

```python
>>> s = '가나다라 마바사아 자차카타 파하'
>>> s.endswith('마')
False
>>> s.endswith('하')
True

>>> s.endswith('마',0,10)
False
>>> s.endswith('마',0,6)
True
```


---

+ 출처
    + [Python 문자열 뒤집기(Reverse String)](https://dongyeopblog.wordpress.com/2016/11/21/python-%EB%AC%B8%EC%9E%90%EC%97%B4-%EB%92%A4%EC%A7%91%EA%B8%B0reverse-string/)
    + [Python 알파벳 대문자로 변환하기](https://m.blog.naver.com/PostView.nhn?blogId=cjh226&logNo=220992745309&proxyReferer=https:%2F%2Fwww.google.com%2F)
    + [[Python] 파이썬 특정문자 찾기(find,startswith,endswith)](https://dpdpwl.tistory.com/119?category=977964)
