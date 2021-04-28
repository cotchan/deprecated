---
title: Wiki) 정규식 (본인이 적용했던 것들)
author: cotchan 
date: 2021-04-23 23:11:21 +0800 
categories: [Wiki]
tags: [wiki] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. Java에서 사용한 예제

---


## 2. Dart(Flutter)에서 사용한 예제

---

## 2-1. Summary

+ **`2021 OJT Flutter 프로젝트에서 사용했던 정규식 모음`**

```dart
//프로젝트 내에서 URL 유효성 판단 로직. 영어대소문자, 숫자, URL 표현을 위한 -, ., /만 허용
_unvalidUrlExpression = new RegExp(r'[^a-zA-Z0-9-\\./]');

//is alpabet 의미. 주어진 문자가 최소 하나라도 영어대소문자를 포함하고 있는지 식별하기 위함
_isAlphabet = new RegExp(r'[a-zA-Z]');

//is digit 의미. 주어진 문자가 최소 하나라도 숫자를 포함하고 있는지 식별하기 위함
_isDigit = new RegExp(r'\d');

//not digit 의미. hasMatch 함수를 적용해서 숫자가 아닌 문자를 식별하는 게 목적
_isNotDigit = new RegExp(r'\D');

//not word 의미. hasMatch 함수를 적용해서 숫자, 영어 대소문자를 제외한 특수문자를 전부 잡아내는 게 목적
_isNonCharacter = new RegExp(r'\W');
```

---

## 2-2. 숫자, 영문자 조합 여부 판단(ID, PW)

+ **`isDigit`, `isAlphabet`, `isNonCharacter` 사용**
+ **요구 사항: `최소 1개의 숫자`와 `최소 1개의 영어 대소문자를 포함`해야 합니다.**

```dart
RegExp _isAlphabet = new RegExp(r'[a-zA-Z]');
RegExp _isDigit = new RegExp(r'\d');
RegExp _isNonCharacter = new RegExp(r'\W');

// 최소 1개 숫자 포함하고 && 최소 1개 알파벳(대소문자 모두 포함) 포함하고 && 특수 문자가 포함 안된 경우
bool _isValidId(String userId) {

  if (_isDigit.hasMatch(userId) && _isAlphabet.hasMatch(userId) && !_isNonCharacter.hasMatch(userId))
  {
    int userIdLength = userId.length;
    if (userIdLength < 8 || userIdLength > 20)
    {
      return false;
    }
    else
    {
      return true;
    }
  }
  else
  {
    return false;
  }
}


RegExp _isAlphabet = new RegExp(r'[a-zA-Z]');
RegExp _isDigit = new RegExp(r'\d');
RegExp _isNonCharacter = new RegExp(r'\W');
```

---

## 2-3. 네 자리 숫자 여부 판단(포트 번호)

+ **`isNotDigit` 사용**
+ **포트 번호는 `4자리 숫자`인 경우만 유효**

```dart
//숫자가 아닌 애를 식별하는 정규식에 매칭되었다? -> false
// 그게 아니라면 숫자로만 이루어져있다고 판단 
bool _isValidPort(String portNumber) {

  if (_isNotDigit.hasMatch(portNumber))
  {
    return false;
  }

  return (portNumber.length == 4);
}

RegExp _isNotDigit = new RegExp(r'\D');
```

---

## 2-4. 숫자, 영문자 조합한 URL 유효성 판단

+ **진짜 URL 식별을 위한 유명한 정규식을 적용한게 X**
  + 숫자, 영문자 조합이어야 함
  + 그리고 URL 특성상 쓰일 수 밖에 없는 `.`, `/`, `-`는 포함


```dart
bool _isValidUrl(String requestUrl) {
  if (_unvalidUrlExpression.hasMatch(requestUrl))
    return false;
  else
    return true;
}

//URL 중에 매칭되는 애가 있다면 -> 유효한 URL 표현이 X
//해석: [] 범위로 묶어서, 영어대소문자, 숫자, 그리고 -, . 표현을 위함 \\ 2개 사용 그리고 /
RegExp _unvalidUrlExpression = new RegExp(r'[^a-zA-Z0-9-\\./]');
```

---

+ 출처
  + [regexr](https://regexr.com/)
