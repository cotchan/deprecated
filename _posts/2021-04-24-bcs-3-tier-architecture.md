---
title: bcs) 3-Tier 아키텍쳐 패턴
author: cotchan 
date: 2021-04-24 23:34:21 +0800 
categories: [ComputerScience, BCS]
tags: [bcs]
---

+ 아래 출처의 내용들을 바탕으로 작성한 내용입니다.    
+ **계속 업데이트 할 예정입니다.**

---

## 1. 3-tier-아키텍쳐란

+ **`REST API`를 만드는 대표적인 아키텍쳐 패턴입니다.**

![3-tier-architecture](https://user-images.githubusercontent.com/75410527/115984724-991e6f80-a5e3-11eb-958b-4ae26af9611f.PNG)

---

## 2. Presentation Layer

+ 실제 사용자와 접점이 있는 곳
  + html page
  + android, ios 등

---

## 3. Application Layer

+ **실제 `Backend application 로직이 동작`하는 Layer**

- `주의`: mvc는 application layer 안에서 백엔드 로직을 개발할 때 사용하는 패턴
- 아키텍쳐가 더 포괄적인 개념

---

## 4. Data Layer

+ 데이터를 불러오는 계층

---

## 5. 3-tier 아키텍쳐의 장점

- 서버에 트래픽이 몰려서 서버가 죽을 때 해결할 수 있는 가장 쉬운 방법은?
    - **`Scale out` (Application Layer를 늘립니다.)**

![scale-out-01](https://user-images.githubusercontent.com/75410527/115984799-e00c6500-a5e3-11eb-8c55-c6052120428c.PNG)

+ 왜 3-tier 아키텍쳐를 사용할까요?
    + **Scale out(수평 확장)이 쉽기 때문입니다.**
    + **즉, 각 Layer가 모듈화가 잘 되어있으면 다른 Layer에 영향을 안 주고 변화를 줄 수 있습니다.**

+ 그런데 Scale out 아키텍쳐를 만들 때 신경써야 할 것은 **특정 노드에 장애가 발생했을 때 사용자 로그인이 풀리거나 하는 문제에 대응하는 것입니다.**
+ 대응하는 대표적인 방법 
    + `세션 클러스터` 구성
    + `JWT` 사용

---
+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
