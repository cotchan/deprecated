---
title: Flutter) [ios] permission handler 사용 시 Missing Purpose String in Info.plist ERROR
author: cotchan
date: 2021-06-04 10:25:00 +0800
categories: [Flutter]
tags: [flutter-and_ios]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 문제

+ ios에서 permission handler 라이브러리 사용시, 사용하지 않는 권한에 대해서도 Info.plist 에 목적을 명시하지 않아서 `앱스토어에 업로드 거부당하는 문제`입니다.

---

## 2. 원인

+ permission handler 라이브러리 안에서 모든 권한에 대해 처리하는 코드가 각 Native로 구현되어있습니다.
+ 그래서 iOS에서는 해당 권한을 사용한다고 인지하여 오류가 납니다.

---

## 3. 해결방법

+ **Cocoapods에 빌드옵션 Define 스크립트를 넣어 해결합니다.**

+ iOS 는 Flutter 라이브러리를 빌드할때 Cocoapods 을 이용하는데, Cocoapods 빌드시 빌드옵션으로 안쓰는 권한들에 대해서 Define 해주는 스크립트를 넣어 해결합니다.


```
post_install do |installer|
  installer.pods_project.targets.each do |target|
    flutter_additional_ios_build_settings(target)
    target.build_configurations.each do |config|
         # You can remove unused permissions here
         # for more infomation: https://github.com/BaseflowIT/flutter-permission-handler/blob/develop/permission_handler/ios/Classes/PermissionHandlerEnums.h
         # e.g. when you don't need camera permission, just add 'PERMISSION_CAMERA=0'
         config.build_settings['GCC_PREPROCESSOR_DEFINITIONS'] ||= [
           '$(inherited)',

           ## dart: PermissionGroup.calendar
           'PERMISSION_EVENTS=0',

           ## dart: PermissionGroup.reminders
           'PERMISSION_REMINDERS=0',

           ## dart: PermissionGroup.contacts
           'PERMISSION_CONTACTS=0',

           ## dart: PermissionGroup.camera
           ## 'PERMISSION_CAMERA=0',

           ## dart: PermissionGroup.microphone
           'PERMISSION_MICROPHONE=0',

           ## dart: PermissionGroup.speech
           'PERMISSION_SPEECH_RECOGNIZER=0',

           ## dart: PermissionGroup.photos
           # 'PERMISSION_PHOTOS=0',
           
           ## dart: PermissionGroup.sensors
           'PERMISSION_BLUETOOTH=0',
           
           ## dart: [PermissionGroup.location, PermissionGroup.locationAlways, PermissionGroup.locationWhenInUse]
           'PERMISSION_LOCATION=0',

           ## dart: PermissionGroup.notification
           'PERMISSION_NOTIFICATIONS=0',

           ## dart: PermissionGroup.mediaLibrary
           'PERMISSION_MEDIA_LIBRARY=0',

           ## dart: PermissionGroup.sensors
           'PERMISSION_SENSORS=0'
         ]
    end
  end
end
```


---

+ 출처
  + [iOS Not Working after upgrading to flutter 1.20.1](https://github.com/Baseflow/flutter-permission-handler/issues/365)
