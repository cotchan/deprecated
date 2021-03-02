---
title: sb2) Lombok 설치(with Intellij & Gradle)
author: cotchan 
date: 2021-03-03 00:35:21 +0800 
categories: [Spring-Boot2, Spring-Boot2_Setup]
tags: [] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. build.gradle 설정 추가

+ `compile('org.projectlombok:lombok')` 을 추가합니다. 

```
dependencies {
    compile('org.projectlombok:lombok')
}
```

---

## 2. 플러그인 설치

+ 단축키 플러그인으로 `Action`을 검색합니다.
  + mac기준: `cmd` + `shift` + `A`
  + 윈도우 기준: `Ctrl` + `Shift` + `A`
+ **롬복 플러그인을 설치한 후 인텔리제이를 `재시작`합니다.**

---

## 3. Annotation Processors 설정

+ **`Enable annotation processing`을 체크합니다.**

+ Annotation Processors 찾는 법
  1. Preferences에서 'Annotation' 검색
  2. 또는 Settings > Build > Compiler > Annotation Processors

---

+ 위 과정을 모두 마치면 이제 롬복을 사용할 수 있습니다. 


---

+ 출처
  + 이동욱, 『스프링 부트와 AWS로 혼자 구현하는 웹 서비스』, 프리렉(2019) 
