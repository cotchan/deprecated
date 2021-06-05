---
title: JWT) JWT(Json Web Token) 이란?
author: cotchan 
date: 2021-06-05 17:23:21 +0800 
categories: #
tags: [jwt] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처를 참고하여 작성하였습니다.**

---

## 1. JWT 소개

+ **JWT는 클라이언트와 서버, 서비스와 서비스 사이에 `인증을 위해 사용하는 토큰`입니다.**
+ JSON Web Token (JWT) 은 웹표준 (RFC 7519) 으로서 두 개체에서 JSON 객체를 사용하여 가볍고 자가수용적인(`self-contained`) 방식으로 정보를 안전성 있게 전달해줍니다.
+ URL에대해 안전한 문자열로 구성되어 있기 때문에 HTTP 어디든(`URL`, `Header` 등) 위치할 수 있습니다.

---

## 2. JWT 구조

+ JWT의 구조는 **`HEADER.PAYLOAD.SIGNATURE`** 입니다.

![스크린샷 2021-06-05 오후 6 27 52](https://user-images.githubusercontent.com/75410527/120887060-d5ee7500-c62b-11eb-87e6-ba63ddf24e61.png)

---

## 2-1. Header

- **헤더는 `JWT를 어떻게 검증(verify)하는가에 대한 내용`을 담고 있습니다.**
- 두 가지 정보를 가지고 있습니다.
    - **`토큰의 타입` (JWT)**
    - **`해싱 알고리즘`(서명 시 사용하는 알고리즘)을 지정합니다.**
      - 이 알고리즘은 토큰을 검증할 때 사용되는 `signature 부분에서 사용`합니다.

```json
{
  "typ": "JWT",
  "alg": "HS256"
}
```

- **위와 같은 `JSON 객체를 문자열로 직렬화`하고 `UTF-8`과 `Base64 URL-Safe로 인코딩`하면 `Header 파트가 완성`됩니다.**



---

## 2-2. Payload

- **JWT의 내용입니다. 토큰에 담을 정보가 들어있습니다.**
- **Payload에 있는 속성들을 `클레임 셋`이라고 부릅니다. (`Claim set`, 여러개의 클레임을 의미)**
    - **Payload에 담는 `정보의 한 조각`을 `클레임(Claim)`이라고 부릅니다.**
    - **`클레임은 name / value 한 쌍`으로 이뤄져 있습니다.**
    - 토큰에는 여러개의 클레임들을 넣을 수 있습니다.
- 클레임셋은 클라이언트와 서버가 주고 받기로한 값들로 구성됩니다.

```json
{
    "iss": "jinho.shin",
    "iat": "1586364327"
}
```

+ **위와 같은 `JSON 객체를 문자열로 직렬화`하고 `Base64 URL-Safe로 인코딩`하면 `Payload 파트가 완성`됩니다.**



---

## 2-3. Signature

- **점(.)을 구분자로 해서 `Header 파트`와 `Payload 파트`를 `합친 문자열`을, 주어진 `비밀키로 해싱`하고 `Base64 URL-Safe로 인코딩`한 값입니다.**
    - 문자열을 인코딩 하는게 아닌 hex → base64 인코딩을 해야합니다.

---

## 2-4. 완성된 JWT

- **점을 구분자로 해서 `Header`, `Payload`, `Signature`를 합치면 JWT가 완성됩니다.**
- **이렇게 완성된 JWT는 `헤더의 알고리즘`과 `공개 키를 이용해 검증할 수 있습니다.`**
- 서명 검증이 성공하면 JWT의 모든 내용을 신뢰할 수 있게 되고, 페이로드의 값으로 접근 제어나 원하는 처리를 할 수 있습니다.


---

+ 출처
  + [[JWT] JSON Web Token 소개 및 구조](https://velopert.com/2389)
  + [JWT를 소개합니다.](https://meetup.toast.com/posts/239)
  + [jwt.io](https://jwt.io/)
