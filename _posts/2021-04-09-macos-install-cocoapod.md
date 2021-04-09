---
title: macOS) CocoaPods 설치 방법
author: cotchan
date: 2021-04-09 23:20:21 +0800
categories: [Macos, Macos_Develop]
tags: [macos]
---

## 1. HomeBrew 설치

+ HomeBrew를 통해 `Ruby`를 설치하려고 HomeBrew를 설치합니다.
  + [https://brew.sh/index_ko](https://brew.sh/index_ko) 

```
//아래 코드를 터미널에서 실행

$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

---

## 2. Gem을 위한 Ruby 설치

```
//아래 코드를 터미널에서 실행

$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

---

```
$ brew update
```

---

## 3. Gem을 통해 CocoaPods 설치

```
$ sudo gem install cocoapods
```

---

## 4. CocoaPods 설치 안되는 문제 헤결방법

+ 2번 과정을 진행하면 아래와 같은 오류 메시지가 나옵니다.

```
//Error Message

Building native extensions. This could take a while... ERROR: Error installing cocoapods: ERROR: Failed to build gem native extension.
```

---

+ **`아래 코드를 복사 후 실행`하면 문제를 해결할 수 있습니다.**

```
$ brew install cocoapods --build-from-source
```

---

+ 출처
  + [ERROR: Failed to build gem native extension. For macbook 카타리나](https://effectivecode.tistory.com/1435)
  + [Ruby 처음 배우기 : 맥에 Ruby 설치하기](https://smartbase.tistory.com/43)
