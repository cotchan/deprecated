---
title: Flutter) locator (의존성 주입 라이브러리)
author: cotchan
date: 2021-04-24 15:33:00 +0800
categories: [Flutter]
tags: [flutter2]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. locator란

- **특징**
  - 의존성 주입 라이브러리로 `global locator를 사용`하여 어디에서든 type을 요청가능합니다.
  - **위젯을 포장하거나 context가 없어도 사용가능합니다.**
  - 인스턴트 추적(instance tracking)은 Factories or Singleton를 통해 자동으로 처리
  - 다방향 데이터 흐름인 전역 객체 사용

---

## 2. locator에 클래스 등록하는 방법

+ **locator.dart 안에 `GetIt의 instance를 저장`한다.**
+ `setupLocator라는 function을 생성`하여 액세스하고자 하는 모든 type을 등록합니다.

```dart
//locator.dart

import 'package:get_it/get_it.dart';

GetIt locator = GetIt.instance;

final GetIt locator = GetIt.instance;

void setupLocator() {

  //=== Data Source ===
  locator.registerSingleton<LocalDataSourceImpl>(LocalDataSourceImpl());
  locator.registerSingleton<RemoteDataSourceImpl>(RemoteDataSourceImpl());

  //=== Repository ===
  locator.registerSingleton<UserRepositoryImpl>(UserRepositoryImpl());
  locator.registerSingleton<StorageFileRepositoryImpl>(StorageFileRepositoryImpl());

  //=== AppManager ===
  locator.registerSingleton<AppManager>(AppManager());
}
```

+ 해당 파일(`locator.dart`)을 import한 어디에서든 전역적으로 접근가능하도록 합니다.
+ **setupLocator를 runApp() 전에 불러주면 service provider가 locator에 등록됩니다.**
  - 모든 타입들은 앱을 시작하기 전에 등록되어야 합니다.

---

## 3. 데이터 접근 방법

- **get it에서 `instance를 요청하기 위해서는` locator로부터 주입받고자 하는 type을 확인하면 됩니다.**
- 따라서 전체적인 앱의 UI 구조가 변경되더라도 올바른 서비스와 값을 주입할 수 있습니다.

+ 예시 코드

```dart
import 'service_locator.dart';


void func() {

  final AppManager _appManager;
  final UserRepositoryImpl _userRepository;
  final StorageFileRepositoryImpl _storageFileRepository;

  _appManager = locator<AppManager>();
  _userRepository = locator<UserRepositoryImpl>();
  _storageFileRepository = locator.get<StorageFileRepositoryImpl>();
}
```

---

+ 출처
  + [https://seizemymoment.tistory.com/48](https://seizemymoment.tistory.com/48)




