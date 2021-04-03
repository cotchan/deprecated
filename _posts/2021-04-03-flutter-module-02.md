---
title: Flutter) [Module] 폼 입력값 검증 및 화면 전환
author: cotchan
date: 2021-04-03 15:40:00 +0800
categories: [Flutter, Flutter_module]
tags: [flutter_module]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Form, TextFormField, TextEditingController

+ **입력값을 검증하는데는 `Form`과 `TextFormField 위젯`을 사용합니다.**
+ `Form`과 `TextFormField`를 사용하면 회원 가입처럼 `사용자 입력값을 검증해야 할 때 유용`합니다.
+ **`TextFormField` 위젯은 TextField 위젯이 제공하는 기능에 추가로 `validator` 프로퍼티를 활용한 검증 기능도 제공합니다.**

+ 폼에 값을 입력받고 결과 버튼을 눌렀을 때 `TextFormField` 위젯에 입력된 값을 가져와야 합니다.
  + **이 때 사용하는 것이 `TextEditingController` 클래스입니다.**



---

## 2. 코드 예시

+ **키와 몸무게 입력값을 검증하고 결과 화면으로 값을 전달하도록 만들기**

+ [1. 폼 필드값을 얻는 컨트롤러 준비](https://github.com/cotchan/obesity_calculator/tree/feature/3-1_validationAndScreenChange_setController)
+ [2. TextFormField 위젯과 컨트롤러 연결](https://github.com/cotchan/obesity_calculator/tree/feature/3-2_connect_WidgetAndController)
+ [3. 버튼 클릭 시 폼을 검증하고 다음 화면으로 값 전달](https://github.com/cotchan/obesity_calculator/tree/feature/3-3_formValidation_and_Routing)

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

