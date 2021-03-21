---
title: Flutter) [레이아웃과 위젯] Text, Image 위젯
author: cotchan
date: 2021-03-21 16:33:00 +0800
categories: [Flutter, Flutter_widget]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Text 위젯

+ 필요한 구글에서 `flutter text <원하는 내용>`으로 검색

---

## 1-1. Text 위젯 생성자

+ Text 위젯의 생성자는 다음과 같습니다.

```dart
Text (this.data, {
  Key key,
  this.style,
  this.structStyle,
  this.textAlign,
  this.textDirection,
  this.locale,
  this.softWrap,
  this.overflow,
  this.textScaleFactor,
  this.maxLines,
  this.semanticsLabel,
  this.textWidthBasis,
})
```

---

## 1-2. Text 위젯 샘플코드

```dart

import 'package:flutter/cupertino.dart';
import 'package:flutter/material.dart';

void main() => runApp(TextDemo());

class TextDemo extends StatelessWidget {
  static const String _title = "Text 위젯 데모";
  static const String _name = 'Tony Startk';
  static const String _longText = """
  플러터(Flutter)는 구글이 개발한 오픈소스 모바일 애플리케이션 개발 프레임워크다.
  안드로이드, iOS용 애플리케이션 개발을 위해,
  또 구글 퓨시아용 애플리케이션 개발의 주된 방식으로 사용된다.(위키백과)
  """;

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: _title,
      home: Scaffold(
        appBar: AppBar(title: Text(_title)),
        body: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: <Widget>[
            Text("단순 텍스트 표시"), //simple text  11111
            Text(
              'Styled Text with $_name', //styled text  22222
              style: TextStyle(
                color: Colors.black,
                fontSize: 20.0,
                background: Paint()
                    ..color = Color(0xFFDCEDC8)
                    ..style = PaintingStyle.fill,
                  fontWeight: FontWeight.bold),
              ),
              Text(
                _longText,
                overflow: TextOverflow.ellipsis,  //333333
            ),
          ],
        ),
      ));
  }
}
```

---

## 1-3. 샘플 코드 해석

+ **11111**
  + 단순 텍스트입니다.
  + 인자 이름을 입력하지 않고 본문 내용을 넣으면 됩니다.

---

+ **22222**
  + 텍스트 스타일을 적용합니다.
  + 단순 텍스트를 입력한 것뿐만 아니라 다트 언어에서 제공하는 interpolation을 활용할 수 있습니다.
  + 또한 텍스트 스타일은 `style` 인자를 지정합니다.
  + 위의 소스 코드에서는 텍스트의 크기(fontSize)를 지정하고 글자 색상(color)과 배경색(background)을 지정합니다.
  + 마지막으로 폰트 중량(fontWeight)을 지정합니다. 폰트 중량에는 이탤릭체, 볼드체 등을 지정할 수 있습니다.

---

+ **33333**
  + overflow 속성을 지정하면 텍스트가 정해진 공간을 넘었을 때 줄임표(...)로 표시합니다.

---

## 1-4. 샘플 코드 실행 결과

![Desktop View](/assets/img/post/flutter/2021-03-21-text-widget-example.png)

---

## 2. Image 위젯

+ 모바일 앱에서는 작은 화면에 다양한 형태의 이미지나 아이콘을 표시합니다.
+ 가장 단순하게 이미지를 불러오는 방법은 `Image.asset()` 메서드를 호출

---

## 2-1. 이미지 추가 방법

+ 이미지를 추가할 때는 **파일을 프로젝트의 `assets` 폴더 하위에 위치시키고 `pubspec.yaml`에 등록해야 합니다.**
  + 그래야 소스코드에서 읽을 수 있습니다. 

+ `pubspec.yaml`에 등록

```
//pubspec.yaml
flutter:

  //...
  assets:
    - assets/london.jpg
    - assets/autumn-leaves.jpg
```

---

## 2-2. Image.aseet() 생성자

+ 빈번하게 사용하는 속성으로는 필수 속성인 `이름`과 `너비(width)`, `높이(height)`와 `정렬(alignment)` 등입니다. 

```dart
Image.asset(
    String name, {
      Key key,
      AssetBundle bundle,
      this.frameBuilder,
      this.semanticLabel,
      this.excludeFromSemantics = false,
      double scale,
      this.width,
      this.height,
      this.color,
      this.colorBlenMode,
      this.fit,
      this.alignment = Alignment.center,
      this.repeat = ImageRepeat.noRepeat,
      this.centerSlice,
      this.matchTextDirection = false,
      this.gaplessPlayback = false,
      String package,
      this.filterQuality = FilterQuality.low,
})
```

---

## 2-3. Image 위젯 샘플 코드

```dart
import 'package:flutter/material.dart';

void main() => runApp(ImageDemo());

class ImageDemo extends StatelessWidget {
  static const String _title = "Image 위젯 데모";

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: _title,
      debugShowCheckedModeBanner: false,
      home: Scaffold(
        appBar: AppBar(title: Text(_title)),
        body: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          children: <Widget>[
            Image.asset('assets/london.jpg'),  //11111
            Image.asset('assets/autumn-leaves.jpg'),
          ],
        ),
      )
    );
  }
}
```

---

## 2-4. 샘플 코드 해석

+ **11111**
  + `pubspec.yaml`에 등록한 이미지를 Image 위젯으로 표시합니다.

---

## 2-5. 샘플 코드 실행 결과

![Desktop View](/assets/img/post/flutter/2021-03-21-text-widget-example-2.png)

---

+ 출처
  + 유동환, 『처음 배우는 플러터』, 한빛미디어(2020) 
