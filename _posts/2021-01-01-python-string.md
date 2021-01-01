---
title: Python) 문자열 기법 정리 
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

+ 출처
    + [Python 문자열 뒤집기(Reverse String)](https://dongyeopblog.wordpress.com/2016/11/21/python-%EB%AC%B8%EC%9E%90%EC%97%B4-%EB%92%A4%EC%A7%91%EA%B8%B0reverse-string/)
    + [Python 알파벳 대문자로 변환하기](https://m.blog.naver.com/PostView.nhn?blogId=cjh226&logNo=220992745309&proxyReferer=https:%2F%2Fwww.google.com%2F)
