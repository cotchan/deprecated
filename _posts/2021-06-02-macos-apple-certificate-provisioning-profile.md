---
title: macOS) 코드사이닝, 인증서, 프로비저닝 프로파일이란?
author: cotchan
date: 2021-06-02 10:00:00 +0800
categories: [Macos, Macos_Develop]
tags: [macos]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 애플 인증서

+ **요약**
  + **`개발자`가 `apple 인증서를 받으면` (애플 대신에) `앱을 실행할 수 있는 권한을 부여받습니다.`**

---

## 2. 프로비저닝 프로파일

+ **요약**
  + **`애플인증서와 iOS 기기를 연결시켜주는 역할`을 합니다.**
  + 3가지 정보로 이루어져 있습니다.
    1. `appID`: 앱 스토어에 등록될 Bundle ID가 등록됩니다.
    2. `인증서`: 위에서 언급한 애플 인증서
    3. `Device`: 디바이스 UDID

---

+ 출처
  + [코드사이닝, 인증서, 프로비저닝 프로파일이란?](https://medium.com/jinshine-%EA%B8%B0%EC%88%A0-%EB%B8%94%EB%A1%9C%EA%B7%B8/%EC%BD%94%EB%93%9C%EC%82%AC%EC%9D%B4%EB%8B%9D-%EC%9D%B8%EC%A6%9D%EC%84%9C-%ED%94%84%EB%A1%9C%EB%B9%84%EC%A0%80%EB%8B%9D-%ED%94%84%EB%A1%9C%ED%8C%8C%EC%9D%BC%EC%9D%B4%EB%9E%80-2bd2c652d00f)
