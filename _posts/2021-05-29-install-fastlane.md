---
title: Install) macOS에서 fastlane 설치 방법(feat. Could not locate Gemfile)
author: cotchan
date: 2021-05-29 15:02:00 +0800
categories: # 
tags: [install]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **이 포스팅에서 설명하는 설치 방법은 `Ruby와 bundler를 사용한 방식`입니다.**

---

## 1. Xcode가 있어야 함

+ Xcode가 없다면 아래 명령어를 입력해줍니다.

```bash
$ xcode-select --install
$ sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```

---

## 2. Installing fastlane

---

## 2-1. Ruby version

+ fastlane은 `Ruby versions 2.5 이상 버전`을 지원합니다. 아래 명령어를 통해 버전을 확인합니다.

```bash
$ ruby --version
```

---

## 2-2. Bundler install

+ Bundler가 없다면 아래의 오류 메시지가 나올 수 있습니다.
  + `You have to install development tools first.`

+ **Bundler 설치 방법**

```bash
$ gem install bundler

//위 명령어가 실행이 안된다면
$ sudo gem install bundler
```

---

## 2-3. Gemfile을 만듭니다.

+ **`vim Gemfile`**
+ **`vim Gemfile`**
+ **`vim Gemfile`**
+ **`vim Gemfile`**
+ **`vim Gemfile`**


```
//Gemfile 내부
source "https://rubygems.org"

gem "fastlane"
```

---

## 3. Run fastlane

+ **fastlane을 실행할 때마다 아래의 명령어를 사용합니다.**

```
$ bundle exec fastlane [lane]
```

---

## 4. 삽질 내용

+ **`지금부터 나오는 내용은 본문 내용과 무관합니다.`**

---

+ **1) gem을 사용하는 fastlane 설치가 계속 안 됨**

+ 아래 명령어를 치면  

```bash
$ sudo gem install fastlane -NV
```

+ 아래의 오류 메시지 발생

```bash
You have to install development tools first.
```

---

+ **2) 그래서 아래 명령어로 fastlane을 깔았음**

```bash
brew install fastlane
```

---

+ **3) Xcode 지워버려서 다시 깔고 아래 명령어 입력**

```bash
$ sudo xcode-select --switch /Applications/Xcode.app/Contents/Developer
```


---

+ **4) 통곡의 오류 메시지**

+ 아래 명령어를 치면

```bash
$ bundle exec fastlane release
```

+ 아래의 오류 메시지 발생

```bash
Could not locate Gemfile or .bundle/ directory
```

---

+ **5) Gemfile이 문제인지 bundler가 없는 게 문제인지 모르니 bundler 설치**

```bash
$ sudo gem install bundler
```

---

+ **6) bundler를 설치해도 나오는 통곡의 오류 메시지**

```
Could not locate Gemfile
```

---

+ **7) 이쯤되니 Gemfile이 없는 게 문제인 걸 눈치챔**
  + **`vim Gemfile`**
  + **`vim Gemfile`**
  + **`vim Gemfile`**
  + **`vim Gemfile`**
  + **`vim Gemfile`**

+ `Gemfile`의 위치는 `fastlane이 설치되어 있는 곳`과 동일한 위치

```bash
chanyoungui-iMac:ios chanyoung$ ls -l
total 32
drwxr-xr-x   8 chanyoung  staff   256  5 26 14:41 Flutter
-rw-r--r--   1 chanyoung  staff    46  5 28 19:21 Gemfile
-rw-r--r--   1 chanyoung  staff  2909  5 27 16:08 Podfile
-rw-r--r--   1 chanyoung  staff  4853  5 27 16:17 Podfile.lock
drwxr-xr-x  18 chanyoung  staff   576  5 26 14:41 Pods
drwxr-xr-x  16 chanyoung  staff   512  5 28 11:08 Runner
drwxr-xr-x@  5 chanyoung  staff   160  5 28 11:08 Runner.xcodeproj
drwxr-xr-x@  5 chanyoung  staff   160  5 24 17:50 Runner.xcworkspace
drwxr-xr-x   6 chanyoung  staff   192  5 28 19:20 fastlane
drwxr-xr-x   3 chanyoung  staff    96  5 11 14:17 keys
drwxr-xr-x   3 chanyoung  staff    96  5 11 14:17 profiles
```

---

+ **8) 이제는 fastlane을 실행하는 아래 명령어가 동작**

```bash
$ bundle exec fastlane release
```

---

+ 출처
  + [Getting started with fastlane for iOS](http://docs.fastlane.tools/getting-started/ios/setup/)
  + [Gemfile, Gem = 루비 라이브러리 설치 및 사용](https://record22.tistory.com/40)
