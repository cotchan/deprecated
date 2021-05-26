---
title: Flutter) RenderBox was not laid out
author: cotchan
date: 2021-05-26 20:44:00 +0800
categories: [Flutter]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 에러 발생 원인

+ **`ListView를` `Column / Row 안에 배치`할 때 발생하는 에러 메시지입니다.** 

---

## 2. 해결 방법

+ **오류를 방지하려면 `내부 ListView에 크기를 제공해야합니다.`**

---

## 3. SampleCode

```dart
    new Row(
      children: <Widget>[
        Expanded(
          child: SizedBox(
            height: 200.0,
            child: new ListView.builder(
              scrollDirection: Axis.horizontal,
              itemCount: products.length,
              itemBuilder: (BuildContext ctxt, int index) {
                return new Text(products[index]);
              },
            ),
          ),
        ),
        new IconButton(
          icon: Icon(Icons.remove_circle),
          onPressed: () {},
        ),
      ],
      mainAxisAlignment: MainAxisAlignment.spaceBetween,
    )
```

---

+ 출처
  + [Flutter: RenderBox was not laid out](https://stackoverflow.com/questions/52801201/flutter-renderbox-was-not-laid-out)
