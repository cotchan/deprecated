---
title: Flutter) 가로 방향으로 회전 막기
author: cotchan
date: 2021-05-26 20:27:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 가로 방향 회전 막기

+ main 파일에 아래와 같이 딱 두 줄만 추가하면 됩니다. 
  + **단, `setPreferredOrientations` 함수는 아래 Usage 코드와 같이 `최상위 build 함수에서 실행하면 됩니다.`**

```dart
//Sample

// 서비스 라이브러리
import 'package:flutter/services.dart'; 

// 세로로만 UI 표시 설정
SystemChrome.setPreferredOrientations([DeviceOrientation.portraitUp]); 
```

---

```dart
//Usage
import 'package:flutter/material.dart';
import 'package:flutter/services.dart'; // 서비스 라이브러리 가져오기

void main() => runApp(MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    SystemChrome.setPreferredOrientations([DeviceOrientation.portraitUp]); //세로 고정
    return MaterialApp(    
    ...
    ...
```

---

+ 출처
  + [플러터(Flutter) - 가로 방향 막기(Prevent landscape orientation)](https://m.blog.naver.com/PostView.naver?blogId=chandong83&logNo=222005189894&proxyReferer=https:%2F%2Fwww.google.com%2F)

