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

## 1. 

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
- 보통 로드 밸런서가 라운드 로빈방식으로 부하를 뿌려줍니다.
 
1. **각 Layer가 모듈화가 잘 되어있으면 다른 Layer에 영향을 안 주고 변화를 줄 수 있습니다.**
2. **`Scale-out`이 쉽습니다.**

---

## 6. 3-tier 아키텍쳐의 단점

- 특정 Load(서버)에 문제가 생겼을 때 대응을 해야합니다.
  - 해결방법
    1. **`세션 클러스터`**
    2. **`JWT`**


---
+ 출처
