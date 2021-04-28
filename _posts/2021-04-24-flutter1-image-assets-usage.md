---
title: Flutter) 이미지 사용 방법 (assets)
author: cotchan
date: 2021-04-24 16:16:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 프로젝트에 이미지 추가 방법

+ **이미지를 추가할 때는 파일을 프로젝트의 `assets 폴더 하위에 위치`시키고 `pubspec.yaml에 등록`합니다.**
  + 그래야 소스코드에서 읽을 수 있습니다.

```
//pubspec.yaml
flutter:

  //...
  assets:
    - assets/london.jpg
    - assets/autumn-leaves.jpg
```

---

+ 이미지 추가 예시
  + assets/images/etc 폴더 내 이미지를 등록
  + assets/images/file 폴더 내 이미지를 등록

```yaml
//pubspec.yaml

# The following section is specific to Flutter.
flutter:

  # The following line ensures that the Material Icons font is
  # included with your application, so that you can use the icons in
  # the material Icons class.
  uses-material-design: true

  assets:
    - assets/images/etc/
    - assets/images/file/
```

---

## 2. 이미지 경로 등록 방법

+ 아래 같이 별도의 클래스를 둬서 이미지 path를 관리할 수도 있습니다. 

```dart
//resource_service.dart

class ResourceService {
  static const String SPLASH_IMAGE = 'assets/images/etc/splash.png';
  static const String DIRECTORY_ICON = 'assets/images/file/dir_icon.png';
  static const String WORD_FILE_ICON = 'assets/images/file/docx_icon.png';
  static const String EMPTY_DATA_ICON = 'assets/images/file/empty_data_icon.png';
  static const String ETC_FILE_ICON = 'assets/images/file/etc_icon.png';
  static const String IMAGE_FILE_ICON = 'assets/images/file/image_icon.png';
  static const String PDF_FILE_ICON = 'assets/images/file/pdf_icon.png';
  static const String PPT_FILE_ICON = 'assets/images/file/pptx_icon.png';
  static const String TXT_FILE_ICON = 'assets/images/file/txt_icon.png';
  static const String EXCEL_FILE_ICON = 'assets/images/file/xlsx_icon.png';
  static const String EXCEPTION_MARK_ICON = 'assets/images/etc/exception_mark.png';
}


```

---

## 3. 이미지 뿌릴 때는 Image.asset() 

+ **이미지를 화면에 뿌릴 때는 `Image.assets() 생성자를 사용`합니다.**

+ **이미지 화면에 뿌리는 사용 예시 코드**

```dart
// 사용 예시1
Column(
  children: <Widget>[
    Image.asset('assets/london.jpg'), 
      Image.asset('assets/autumn-leaves.jpg'),
  ],
)


// 사용 예시2
Container(
  child: Image.asset(viewModel.exceptionImage, height: 250),
)


// 사용 예시3
SizedBox(
  width: MediaQuery.of(context).size.width * 0.4,
  child: Image.asset(viewModel.splashImage)
)


// 사용 예시4
Container(
  child: Image.asset(viewModel.emptyImage, height: 250),
)
```

---
