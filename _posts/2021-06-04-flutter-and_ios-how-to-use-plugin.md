---
title: Flutter) [android/ios] Native Plugin 함수 사용방법
author: cotchan
date: 2021-06-04 10:37:00 +0800
categories: [Flutter]
tags: [flutter-and_ios]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

## 1. Flutter Boundary

---

## 1-1. MethodChannel 오픈

+ flutter에서 Native call을 할 `MethodChannel`을 선언합니다.

```dart
  static const MethodChannel _migrationMethodChannel = MethodChannel
    ('app/migration');

  static Future<String?> getPreviousUrl() async {
    return _migrationMethodChannel.invokeMethod<String>('getPreviousUrl');
  }
```

---

## 1-2. Native Call 함수 만들기

+ **flutter에서 Native로 call을 할 `method 이름을 선언합니다.`**

+ parameter를 사용하지 않는 경우

```dart
  static Future<String?> getPreviousUrl() async {
    return _migrationMethodChannel.invokeMethod<String>('getPreviousUrl');
  }
```

---

+ parameter를 사용하는 경우

```dart
  static Future<String?> encodeObfuscatorMessage(String message) async {
    return _cryptoMethodChannel.invokeMethod<String>(
        'encodeRc4Message', message.trim());
  }
```

---

## 2. Native Boundary

+ ios 기준으로 설명합니다.

---

## 2-1. Native's Entrypoint 

+ **ios의 경우 plugin 함수가 호출되는 Entrypoint는 `AppDelegate.swift` 입니다.**

---

## 2-2. Call FlutterChannel

+ **`FlutterMethodChannel` 함수를 사용하여 Flutter에서 선언한 아래의 메소드 채널과 Native 함수를 연결합니다.**
  + MethodChannel(`'app/crypto'`) 
  + MethodChannel(`'app/migration'`)

```swift
//AppDelegate.swift

import Flutter
import UIKit

enum ChannelName {
    static let crypto = "app/crypto"
    static let migration = "app/migration"
}

//enum MethodName

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
    override func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {

        //...

        let cryptoChannel = FlutterMethodChannel(name: ChannelName.crypto,
                                                 binaryMessenger: controller.binaryMessenger)
                
        //...

        let serverChannel = FlutterMethodChannel(name: ChannelName.migration,
                                                binaryMessenger: controller.binaryMessenger)
        
        //...
    }
}

```

---

## 2-3. 사용할 메서드 분기

+ **MethodChannel을 얻은 후 `setMethodCallHandler`를 사용하여 flutter에서 호출하려는 함수를 분기합니다.**

```swift
//함수가 분기되는 부분

cryptoChannel.setMethodCallHandler {
    (call: FlutterMethodCall, result: FlutterResult) -> Void in
    switch (call.method) {
      case MethodName.encodeRc4Message:
      case MethodName.decodeRc4Message:
      default: 
```

---

```swift

//enum ChannelName

enum MethodName {
    static let encodeRc4Message = "encodeRc4Message"
    static let decodeRc4Message = "decodeRc4Message"
    static let getPreviousUrl = "getPreviousUrl";
}

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
    override func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        //...
        
        let cryptoChannel = FlutterMethodChannel(name: ChannelName.crypto,
                                                 binaryMessenger: controller.binaryMessenger)
        cryptoChannel.setMethodCallHandler {
            (call: FlutterMethodCall, result: FlutterResult) -> Void in
            switch (call.method) {
            case MethodName.encodeRc4Message:
                if  let message = call.arguments as? String,
                    let messageData = message.data(using: .utf8),
                    let encryptedData = Obfuscator.encryptAndEncoding(messageData),
                    let encryptString = String(data: encryptedData, encoding: .utf8) {
                    result(encryptString)
                } else {
                    result(FlutterMethodNotImplemented)
                }
                break
            default:
                result(FlutterMethodNotImplemented)
                break
            }   
        }       
                
        let serverChannel = FlutterMethodChannel(name: ChannelName.migration,
                                                binaryMessenger: controller.binaryMessenger)
        
        serverChannel.setMethodCallHandler {
            (call: FlutterMethodCall, result: FlutterResult) -> Void in
            switch (call.method) {
            case MethodName.getPreviousUrl:
                let defaults = UserDefaults.standard
                let serverUrl = defaults.object(forKey: "serverAddr") as? String ?? String()
                let portNumber = defaults.object(forKey: "serverPort") as? String ?? String()
                if serverUrl.isEmpty || portNumber.isEmpty {
                    result(nil)
                } else {
                    let fullUrl = serverUrl + ":" + portNumber
                    result(fullUrl)
                }
                break
            default:
                result(FlutterMethodNotImplemented)
                break
            }
        } 
          
        //...
    }
}
```

---

## 2-4. 메서드 결과값 return 받는 법

+ **`result()`에 메서드의 결과값을 담아줍니다.**
  + **그러면 이제 flutter에서 해당 plugin의 return value를 사용할 수 있습니다.**

```swift
//Throw Exception
result(FlutterMethodNotImplemented)


//result value
result(nil)

result(fullUrl)

result(encryptString)
```

---

## 2-5. Native Part Full Code

+ **`AppDelegate.swift` Full Code**

```swift
//AppDelegate.swift

@UIApplicationMain
@objc class AppDelegate: FlutterAppDelegate {
    override func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?) -> Bool {
        
        //...
        
        let cryptoChannel = FlutterMethodChannel(name: ChannelName.crypto,
                                                 binaryMessenger: controller.binaryMessenger)
        cryptoChannel.setMethodCallHandler {
            (call: FlutterMethodCall, result: FlutterResult) -> Void in
            switch (call.method) {
            case MethodName.encodeRc4Message:
                if  let message = call.arguments as? String,
                    let messageData = message.data(using: .utf8),
                    let encryptedData = Obfuscator.encryptAndEncoding(messageData),
                    let encryptString = String(data: encryptedData, encoding: .utf8) {
                    result(encryptString)
                } else {
                    result(FlutterMethodNotImplemented)
                }
                break
            case MethodName.decodeRc4Message:
                if  let message = call.arguments as? String,
                    let messageData = message.data(using: .utf8),
                    let decryptedData = Obfuscator.decodingAndDecrypt(messageData),
                    let decryptString = String(data: decryptedData, encoding: .utf8) {
                    result(decryptString)
                } else {
                    result(FlutterMethodNotImplemented)
                }
                break
            default:
                result(FlutterMethodNotImplemented)
                break
            }   
        }       
                
        let serverChannel = FlutterMethodChannel(name: ChannelName.migration,
                                                binaryMessenger: controller.binaryMessenger)
        
        serverChannel.setMethodCallHandler {
            (call: FlutterMethodCall, result: FlutterResult) -> Void in
            switch (call.method) {
            case MethodName.getPreviousUrl:
                let defaults = UserDefaults.standard
                let serverUrl = defaults.object(forKey: "serverAddr") as? String ?? String()
                let portNumber = defaults.object(forKey: "serverPort") as? String ?? String()
                if serverUrl.isEmpty || portNumber.isEmpty {
                    result(nil)
                } else {
                    let fullUrl = serverUrl + ":" + portNumber
                    result(fullUrl)
                }
                break
            default:
                result(FlutterMethodNotImplemented)
                break
            }
        } 
          
        GeneratedPluginRegistrant.register(with: self)
        return super.application(application, didFinishLaunchingWithOptions: launchOptions)
    }
}
```

---

+ 출처
