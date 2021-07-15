---
title: ios) View Controller Life Cycle 
author: cotchan 
date: 2021-07-15 15:21:21 +0800 
categories: [ios]
tags: [ios]
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. Overview

---

## 2. 라이프싸이클 설명

---

## 2-1. init

- **ViewController 객체가 생성될 때 초기화 작업을 하는 메소드**
- 이 메소드에서 초기화 작업을 할 때 ViewController는 필요한 자원을 할당받음
- 아직 이 시점에는 ViewController의 View가 생성된 것이 X

---

## 2-2. loadView

- 본격적으로 화면에 띄워질 View를 만드는 메소드

---

## 2-3. viewDidLoad

- **ViewController가 메모리에 로드되고 난 후에 호출됩니다.**
  - **이 메소드가 호출되는 시점에는 이미 outlet(변수)들이 메모리에 위치하고 있음**
- **그러므로 `사용자에게 화면이 보여지기 전에 데이터를 뿌려주는 행위에 대한 코드` 작성**
- **이 메소드는 `ViewController 라이프싸이클 중에` `오직 한 번만 호출`됩니다.**
    - 그러므로 오로지 한 번만 일어날 행위들에 대해 이 메소드 안에서 정의해야 함

---

## 2-4. viewWillAppear

- ViewController의 화면이 올라오고 난 이후에 호출되는 메소드
- 이 메소드에서는 UI내 애니메이션을 실행하거나, 뿌려지는 데이터 업데이트 수행 역할
- **`화면 전환을 통해 다시 현재 화면으로 돌아올 때` `viewWillAppear가 호출`됩니다.**

---

## 2-5. viewDidAppear

- **뷰가 화면에 나타난 직후에 실행됩니다.**
- view가 나타났다는 것을 컨트롤러에게 알리는 역할을 합니다.
- 또한 화면에 적용될 애니메이션을 그려줍니다.

---

## 2-6. viewWillDisappear

- 다음 ViewController로 화면이 전환되기 전, Original ViewController가 화면에서 사라질 때 이 메소드가 호출됩니다.
- **`뷰가 사라지기 직전에` 호출되는 함수입니다.**
  - 뷰가 삭제되려고하는 것을 뷰컨트롤러에게 통지해줍니다.

---

+ 출처
  + [[ios] UIViewController Lifecycle](https://baked-corn.tistory.com/32)
  + [iOS ) View Controller의 생명주기(Life-Cycle)](https://zeddios.tistory.com/43)
