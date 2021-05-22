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

## 1-1. 이메일 정규 표현식

```
import static java.util.regex.Pattern.matches;

  private static boolean checkEmailAddress(String address) {
    return matches("[\\w~\\-.+]+@[\\w~\\-]+(\\.[\\w~\\-]+)+", address);
  }
```
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

## 3. 실제로 사용했던 표현식

---

## 3-1. 앵커 ^ 와 $

+ 앵커는 입력된 정규식이 어떤 특정 위치에서 동작할지를 제한하는 역할의 문자입니다. 
+ 즉, `위치만 제한할 뿐 검색 결과에는 포함되지 않습니다.`

+ **`패턴 시작 앵커(^)`는 해당 정규식이 줄의 시작 부분인지를 확인하는 역할을 합니다.** 
  + **보통 정규식의 가장 앞에 붙여서 사용합니다.**

+ **`패턴 종료 앵커($)`는 해당 정규식이 줄의 마지막 부분인지를 확인하는 역할을 합니다.**
  + **보통 정규식의 가장 마지막에 붙여서 사용합니다.**

```javascript
// 여러 `w` 중에서 문자열의 시작 부분과 일치하는지를 확인합니다
const str = "www";

// 가장 첫 번째 `w` 만 반환됩니다
str.match(/^w/);
// ["w", index: 0, input: "www", groups: undefined]

// 가장 마지막 `w` 만 반환됩니다
str.match(/w$/);
// ["w", index: 2, input: "www", groups: undefined]
```

---

## 3-2. ( )

+ **괄호는 `문자 그룹`을 정의하여, `괄호 내 쌍이 그룹을 형성`합니다.**
+ 즉, ( ) 괄호 안의 문자열은 하나의 의미로 처리됩니다.

+ 예시
  + /api/user/{userId}/** 이하 URL 패턴을 감지한다고 가정하겠습니다.
  + userId는 하나의 숫자로 처리되어야 합니다. 하지만 몇 글자의 숫자가 입력될지 모릅니다.
  + 이런 경우에 정규식은 아래와 같이 작성됩니다.
  + **`"^/api/user/(\\d+)/..."`** 
  + ( ) 괄호 안의 표현이 하나로 묶여서 다음 / 이전까지 {userId}를 하나의 의미로 처리합니다.

---

## 3-3. +

+ **`'문자가 1개 이상 나타남'`을 의미합니다.**

+ 예시
  + 몇 개의 숫자가 입력될지는 모르지만, 숫자만 입력되는 경우
  + 0123456789 범위의 모든 한 자리 숫자와 일치하는 '\d'와 '+' 의 의미가 합쳐지면 '한 글자 이상의 숫자'를 캐치할 수 있습니다.
  + 그래서 한 글자 이상의 숫자를 판별하는 정규식은 아래와 같이 작성됩니다.
  + **`"(\\d+)"`**

---

## 3-4. . 와 *

+ `.`는 모든 문자 하나와 일치합니다.
+ `*`는 문자 또는 숫자가 0개 이상 나타남을 의미합니다.

+ 그러므로 URL에서 '/**'처럼 뒤에 아무 URL이 와도 되는 경우를 나타낼 때 아래와 같이 정규식을 작성합니다.
+ **`^.*$`**

---

+ 출처
  + [regexr](https://regexr.com/)
  + [정규 표현식(regex) 기초](https://support.cognex.com/docs/vidi_341/web/KO/vidisuite/Content/ViDi_Topics/1_Overview/images_display_filters_regex_basics.htm)
  + [정규표현식 완전정복](https://wormwlrm.github.io/2020/07/19/Regular-Expressions-Tutorial.html)
  + [SQL 정규표현식 괄호의 차이](https://velog.io/@sunjoo/SQL-%EC%A0%95%EA%B7%9C%ED%91%9C%ED%98%84%EC%8B%9D-%EA%B4%84%ED%98%B8%EC%9D%98-%EC%B0%A8%EC%9D%B4)
  + [Javascript : 정규식 도메인 URL 추출 ( http , https )](https://m.blog.naver.com/PostView.nhn?blogId=psj9102&logNo=221203659771&proxyReferer=https:%2F%2Fwww.google.com%2F)
