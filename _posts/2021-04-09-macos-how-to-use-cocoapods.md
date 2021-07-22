---
title: macOS) CocoaPods 사용 방법
author: cotchan
date: 2021-04-09 23:36:21 +0800
categories: [Macos, Macos_Develop]
tags: [macos]
---

## 1. pod init

+ `프로젝트 경로`에서 아래 명령어를 입력해줍니다.

```terminal
$ pod init
```

+ **그러면 프로젝트 경로에 `Podfile`이 생성됩니다.**

```
//$ vim Podfile

target 'MyApp' do
  use_frameworks!
  pod 'SnapKit'
end
```

---

## 2. Podfile 작성(편집)

+ **우리가 원하는 라이브러리를 `pod '라이브러리 이름'` 형식으로 작성합니다.**

```
//example

pod 'RealmSwift' 
pod 'AFNetworking', '~> 2.6'

//이렇게 뒤에 ~>가 붙은 코드를 보셨을겁니다. 저 숫자는 버전을 의미합니다. 
```

---

## 3. pod install

+ 작성한 Podfile을 저장하고 `pod install` 명령어를 입력해줍니다.

```
$ pod install
```

---

```
Please close any current Xcode sessions
and use `CocoaPodsTest.xcworkspace`
for this project from now on.
```

+ 설치가 완료되면 위와 같은 메시지가 나옵니다.
+ **이제부터는 기존 프로젝트가 아니라 `.xcworkspace`를 사용하라는 메시지입니다.**
  + 즉, .xcworkspace로 들어가야만 저희가 설치한 라이브러리를 사용할 수 있습니다.

---

## 4. import

+ 끝으로 `.xcworkspace`에서 우리가 설치한 라이브러리를 `import`해주면 해당 라이브러리를 사용할 수 있습니다.

```swift
import UIKit
import RealmSwift
```

---

+ 출처
  + [왕 초보를 위한 CocoaPods(코코아팟) 사용법 (Xcode와 연동)](https://zeddios.tistory.com/25)
  + [CocoaPods 사용법과 파일구조](https://medium.com/@hongseongho/cocoapods-%EC%82%AC%EC%9A%A9%EB%B2%95%EA%B3%BC-%ED%8C%8C%EC%9D%BC%EA%B5%AC%EC%A1%B0-c0ea2ef362d6)
  + [[Xcode] CocoaPods(코코아팟) 설치 및 사용 방법](https://developer-fury.tistory.com/6)
