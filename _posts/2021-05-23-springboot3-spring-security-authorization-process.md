---
title: sb3) 인가(Authorization) 과정 정리 
author: cotchan 
date: 2021-05-23 17:03:21 +0800 
categories: [Spring, Spring_Security]
tags: [spring-security] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처를 참고하여 작성하였습니다.**

---

## 인가란?

+ **인가란 `현재 사용자가 보호된 리소스에 권한이 있는지를 검사하는 것`입니다.**
+ **인가 과정에서 사용하는 주요 컴포넌트는 `AccessDecisionManager`, `AccessDecisionVoter` 입니다.**

---

## 인가 처리(과정) 요약

+ **인가(Authorization)처리를 위한 핵심 컴포넌트**

+ **`AccessDecisionManager`**
  + **인증된 사용자의 보호 리소스 접근 여부를 판단합니다. 3개의 기본 구현을 제공**
    + AffirmativeBased: 접근을 승인하는 voter가 1개 이상
    + ConsensusBased: 과반수
    + UnanimouseBased: 모든 voter가 승인해야 함

+ **`AccessDecisionVoter`**
  + **AccessDecisionManager는 다수의 AccessDecisionVoter로 구성됩니다.**
  + **각각의 Voter는 주어진 정보를 기반으로 아래 정보를 반환합니다.**
    + 승인(`ACCESS_GRANTED`) 
    + 거절(`ACCESS_DENIED`)
    + 보류(`ACCESS_ABSTAIN`)
  + RoleVoter는 보호 리소스에 접근하기 위한 권한을 사용자가 지니고 있는지 확인합니다.
  + WebExpressionVoter는 SpEL 표현식에 따른 웹 접근제어를 처리합니다.

---

<img width="1695" alt="스크린샷 2021-05-23 오후 5 28 46" src="https://user-images.githubusercontent.com/75410527/119253492-6f5d6600-bbec-11eb-942b-1c5883b3c213.png">


---

+ 출처
  + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694)
