---
title: Flutter) 플러터 프로젝트 구성요소
author: cotchan
date: 2021-03-17 20:47:00 +0800
categories: [Flutter, Flutter_main]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 플러터 프로젝트 기본 폴더

---

## 1-1. lib 폴더

+ **플러터 소스코드(`.dart` 파일) 위치**

---

## 1-2. android 폴더

+ 플러터를 컴파일하여 생성된 안드로이드 앱 소스코드. 
+ **`자동 생성`되어 있기 때문에 임의로 수정하지 않습니다.**

---

## 1-3. ios 폴더

+ 플러터를 컴파일하여 생성된 iOS 앱 소스코드. 
+ **`자동 생성`됩니다.**


---

## 1-4. test 폴더

+ 테스트 코드 위치

---

## 2. 최상위 주요 폴더

---

## 2-1. .gitignore 파일

---

## 2-2. .metadata 파일

+ 플러터 프로젝트를 위한 내부 파일.
+ **`개발자가 편집하지 않습니다.`** 

---

## 2-3. .packages 파일

+ pubspec.yaml 파일과 관련된 내부 파일로 자동 생성

---

## 2-4. hello_flutter.iml 파일

+ .iml 파일은 안드로이드 스튜디오에서 생성한 프로젝트 내부 파일

---

## 2-5. pubspec.yaml 파일

+ **`이 파일은 중요`**
+ YAML 문서는 사람에게 읽기 쉽게 만들어진 마크업 언어 파일
+ **`플러터 프로젝트에 필요한 라이브러리와 리소스 등을 지정합니다.`**

---

## 3. 프로젝트 기본 구조1

+ **`기본 프로젝트 앱 실행 흐름`**

![Desktop View](/assets/img/post/flutter/2021-03-17-flutter-project-component.png)


---

## 4. 프로젝트 기본 구조2

+ **`머티리얼 앱 기본 구조`**
  + home과 appBar 속성은 거의 변하지 않으며 body 부분을 나만의 위젯으로 교체하면 간단하게 앱 화면을 만들 수 있습니다.

![Desktop View](/assets/img/post/flutter/2021-03-17-flutter-project-component2.png)

```dart
import 'package:flutter/material.dart';

void main() =>
    runApp(MaterialApp(
      title: 'Hello Flutter',
      home: Scaffold(
        appBar: AppBar(title: Text('Hello Flutter')),
        body: Text('Hello Flutter'),
      ),
    ));
```

---

![Desktop View](/assets/img/post/flutter/2021-03-17-flutter-project-component3.png)

---

+ 출처
  + 유동환, 『처음 배우는 플러터』, 한빛미디어(2020) 
