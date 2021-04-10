---
title: Flutter) Flutter 설치하는 방법(macOS, 안드로이드 스튜디오)
author: cotchan
date: 2021-03-16 00:00:00 +0800
categories: [Flutter, Flutter_main]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

## 1. Android studio 설치

---

## 2. Flutter plugin과 SDK 설치

+ **Android studio에서 플러터 코드를 개발/빌드 하려면 `SDK가 필요`하고 Android studio에 `플러그인이 설치`되어 있어야 합니다.**
+ 다행히도 플러그인을 설치하면 SDK까지 자동으로 설치해줍니다.
+ **그리고 `Dart SDK`는 `Flutter SDK 내부에` 포함되어오니 따로 설치할 필요는 없습니다.**

---

## 2-1. Flutter Plug-in 설치하는 법

+ **아래와 같은 순서로 진행하시면 됩니다.**

1. 메뉴바에서 `Preferences`
2. `Plugins`
3. flutter 검색 후 install 
4. 안드로이드 스튜디오 재시작

---

## 2-2. Flutter SDK 직접 설치하는 법

+ **위 `2-1` 처럼 했는데도 Flutter SDK를 못찾는다고 하는 경우가 있습니다.**
+ 그래서 따로 Flutter SDK만 설치하는 법도 알아야 합니다.

+ **다운로드 경로: https://flutter.dev/docs/get-started/install/macos**
  + 자세한 설치 방법은 위 URL을 참고하면 됩니다.

---

+ **`환경변수로 Flutter 적용하는 방법`**

```dart
//터미널에서 아래 명령어 입력

$ export PATH="$PATH:`pwd`/flutter/bin"
```

+ 환경변수가 적용되면 이제 아래와 같은 명령어를 그냥 사용할 수 있습니다.

```dart
$ flutter --version
$ flutter doctor
```

---

+ **SDK 설치가 완료되면 아래와 같이 `Flutter SDK path를 설정`해줍니다.**

---

+ Flutter SDK 적용 전

![Desktop View](/assets/img/post/flutter/2021-03-16-install-1.png)

---

+ Flutter SDK 적용 후

![Desktop View](/assets/img/post/flutter/2021-03-16-install-2.png)

---

## 3. Android 개발환경 설정


---

## 4. iOS 개발환경 설정

---

+ 출처
  + [[Flutter] Mac에 Flutter 개발환경 설치하기](https://spiralmoon.tistory.com/entry/Flutter-Mac%EC%97%90-Flutter-%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD-%EC%84%A4%EC%B9%98%ED%95%98%EA%B8%B0)
