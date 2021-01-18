---
title: Swift) IBAction, IBOutlet 
author: cotchan
date: 2021-01-18 16:32:21 +0800
categories: [Swift, Swift_UI]
tags: [swift]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. IBAction, IBOutlet의 역할

+ 이 둘의 역할은 StoryBoard와의 연결고리를 담당합니다. 
+ 변수나 함수를 정의할 때 앞에 `@IBAction` 또는 `@IBOutlet` 키워드를 통해 StoryBoard에서 버튼이나 레이블같은 `컴포넌트와 연결이 가능`합니다.

---

## 1-1. IBAction

+ IBAction은 **Event가 일어난 경우 `호출되는 Action`을 정의**합니다.
+ **Action은 입력이 들어왔을 때 어떤 행동을 할 지를 나타냅니다.**

---

## 1-2. IBOutlet

+ IBOutlet은 **값에 접근하기 위한 `변수`**입니다.
+ **Outlet은 데이터를 가져오는 것입니다.**

---

## 2. 접두어 IB

+ IB는 `Interface Builder`의 약자입니다.
+ 예를 들어, IBAction은 Interface Builder를 통해 받아온 정보로 Action을 수행하겠다는 의미입니다.

---

## 3. 접두어 @

+ `@`는 `컴파일러`에게 어떤 속성을 가지고 있다고 전하는 역할을 하는 예약어입니다.
+ 컴파일러에게, @가 붙은 명령어에 대해 어떤 attribute가 부여되었음을 말합니다. 
  + @IBAction: Interface Builder와 연결된 Action. 
  + @UIApplicationMain: App의 Main이 여기에 있습니다.

+ 따라서 @IBAction의 속성이 func의 정의 앞에 붙어있다면, 이 함수는 Interface Builder에서 사용될 수 있고, UI로 연결이 가능하다는 의미를 가집니다.

---

## 4. 뒤에 나오는 UI

+ UI 중에서 어떤 항목을 가리키도록 설정할 것인가를 의미합니다.
  + `UIButton`이라면 버튼을 가리키기 위해 사용될 것입니다.
  + `UILabel`이라면 Label을 가리키기 위해 사용될 것입니다.
 

---

+ 출처
  + [[Swift 기초 문법] - IBAction과 IBOutlet이 뭘까](https://etst.tistory.com/74)
