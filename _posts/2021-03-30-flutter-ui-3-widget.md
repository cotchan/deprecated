---
title: Flutter) [ui] 7.5 화면이 3개인 UI 작성
author: cotchan
date: 2021-03-30 19:37:00 +0800
categories: [Flutter, Flutter_ui]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Step 1

- 현재까지는 별도의 페이지를 작성하지 않고 `_index` 값만 변경하여 표시하고 있습니다.
- 이제 3개의 페이지를 작성하고 실제로 각 페이지가 표시되도록 코드 수정

- main.dart에서 StatelessWidget 클래스를 상속받은 Page1, Page2, Page3 클래스를 정의합니다.
    - 모든 페이지를 똑같은 형태로 만들고 Text의 글자만 '홈 페이지', '이용서비스', '내 정보'로 작성

```dart
class Page1 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        '홈 페이지',
        style: TextStyle(fontSize: 40),
      ),
    );
  }
}

class Page2 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        '이용서비',
        style: TextStyle(fontSize: 40),
      ),
    );
  }
}

class Page3 extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Center(
      child: Text(
        '내 정보',
        style: TextStyle(fontSize: 40),
      ),
    );
  }
}
```

---

## 2. Step 2

+ **다음으로 Scaffold의 body 프로퍼티는 방금 작성한 페이지가 표시되도록 아래와 같이 코드를 수정합니다.**

```dart
//하단 탭 작성
class _MyHomePageState extends State<MyHomePage> {
  var _index = 0;
  var _pages = [  //11111
    Page1(),
    Page2(),
    Page3(),
  ];

  @override
  Widget build(BuildContext context) {
    // TODO: implement build
    return Scaffold(
      appBar: AppBar(...생략...),
      body: _pages[_index],  //22222
      bottomNavigationBar: BottomNavigationBar(...생략...),
    );
  }
}
```

---

+ 11111
  + 페이지를 `_pages` 리스트 변수의 값으로 정의했습니다.
+ 22222
  + body 프로퍼티에는 화면이 갱신될 때마다 현재 선택된 인덱스 번호인 `_index`를 활용하여 해당 페이지를 찾아내도록 처리했습니다.


---

## 3. 결과 화면


![Desktop View](/assets/img/post/flutter/2021-03-29-widget-45.png)

---

![Desktop View](/assets/img/post/flutter/2021-03-29-widget-46.png)

---


![Desktop View](/assets/img/post/flutter/2021-03-29-widget-47.png)

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

