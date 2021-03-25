---
title: Flutter) [위젯] 5.1 입력용 위젯
author: cotchan
date: 2021-03-25 19:30:00 +0800
categories: [Flutter, Flutter_widget]
tags: [flutter]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. TextField

+ 글자를 입력받는 위젯입니다.
+ `InputDecoration` 클래스와 함께 사용하면 힌트 메시지나 외곽선 등의 꾸밈 효과를 간단히 추가할 수 있습니다.

```dart
TextField(),
```

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-24.png)


---

+ `decoration` 프로퍼티를 활용하면 다양한 효과를 줄 수 있습니다.
+ 여기서는 `InputDecoration` 클래스의 `labelText` 프로퍼티를 활용하여 힌트를 표현하고 있습니다.

```dart
TextField(
  decoration: InputDecoration(
    labelText: '여기에 입력하세요',    // 힌트
  ),
),
```

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-25.png)

---

+ `InputDecoration` 클래스의 `border` 프로퍼티에 `OutlineInputBorder` 클래스의 인스턴스를 지정하면 외곽선과 힌트를 표현하는 머티리얼 디자인을 구현할 수 있습니다.

```dart
TextField(
  decoration: InputDecoration(
    border: OutlineInputBorder(),  // 외곽선
    labelText: '여기에 입력하세요',
  ),
),
```

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-26.png)


---

## 2. CheckBox와 Switch

+ 설정 화면 등에 많이 사용되는 체크박스, 라디오 버튼, 스위치를 표현하는 위젯입니다.
+ Checkbox와 Switch는 모양만 다를 뿐 사용 방법은 동일합니다.

+ **`상태를 나타낼 불리언 타입의 변수`가 필요하고, 이 변수를 `value 프로퍼티`에 설정합니다.**
+ **`onchanged 이벤트`는 체크값이 변할 때마다 발생하는데 여기서 변경된 값이 불리언 value 인수로 넘어옵니다.**
+ **그리고 `setState() 함수를 통해` value 프로퍼티에 지정한 변숫값을 변경하며 UI를 다시 그립니다.**
+ 또한 CheckBox와 Switch를 사용하면 상태를 나타내는 변수가 등장하므로 `StatefulWidget`이어야 합니다.

```dart
var isChecked = false;

Checkbox(
  value: _isChecked,
  onChanged: (value) {
    setState(() {
      _isChecked = value;
    });
  },
),

Switch(
  value: _isChecked,
  onChanged: (value) {
    setState(() {
      _isChecked = value;
    });
  },
),
```

---

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

//수정 요소
class MyHomePage extends StatefulWidget {

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {

  var _isChecked = false;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Checkbox / Radio / Switch'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(8.0),
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              Checkbox(
                value: _isChecked,
                onChanged: (value) {
                  setState(() {
                    _isChecked = value;
                  });
                },
              ),
              SizedBox(
                height: 40,
              ),
              Switch(
                value: _isChecked,
                onChanged: (value) {
                  setState(() {
                    _isChecked = value;
                  });
                },
              ),
            ],
          )
        ),
      ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-27.png)

---

## 3. Radio와 RadioListTile

- **선택 그룹 중 하나를 선택할 때 사용하는 위젯입니다.**
- 어디까지를 터치 영역으로 볼 것이냐에 따라서 `Radio`를 사용하거나 `RadioListTile`을 사용합니다.

- Radio는 그룹 내에서 하나만 선택할 때 사용합니다.
1. **그룹이 되는 항목을 `열거형(enum)으로 정의`합니다.**
2. **그리고 `groupValue 프로퍼티`에 열거형으로 정의한 타입의 변수를 지정합니다.**
3. **그리고 `onChanged 이벤트`에서 변경된 값을 반영합니다.**
- **ListTile 대신 RadioListTile을 사용하면 `가로 전체가 터치 영역`이 됩니다.**

```dart
enum Gender { MAN, WOMEN } //1

Gender _gender = Gender.MAN;  //3


//...생략...


//Radio는 라디오 영역만 터치 영역으로 인식
ListTile(
  title: Text('남자'),
  leading: Radio(
    value: Gender.MAN,
    groupValue: _gender,  //2
    onChanged: (value) {
      setState(() {
        _gender = value;
      });
    },
  ),
),
ListTile(
  title: Text('여자'),
  leading: Radio(
    value: Gender.WOMEN,
    groupValue: _gender,
    onChanged: (value) {
      setState(() {
        _gender = value;
      });
    },
  ),
),
```

---

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

//수정 요소
class MyHomePage extends StatefulWidget {

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

enum Gender { MAN, WOMEN }

class _MyHomePageState extends State<MyHomePage> {

  Gender _gender = Gender.MAN;

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Radio / RadioListTile'),
      ),
      body: Padding(
        padding: const EdgeInsets.all(8.0),
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            crossAxisAlignment: CrossAxisAlignment.center,
            children: <Widget>[
              ListTile(
                title: Text('남자'),
                leading: Radio(
                  value: Gender.MAN,
                  groupValue: _gender,
                  onChanged: (value) {
                    setState(() {
                      _gender = value;
                    });
                  },
                ),
              ),
              ListTile(
                title: Text('여자'),
                leading: Radio(
                  value: Gender.WOMEN,
                  groupValue: _gender,
                  onChanged: (value) {
                    setState(() {
                      _gender = value;
                    });
                  },
                ),
              ),
              SizedBox(
                height: 40,
              ),
              RadioListTile(
                title: Text('남자'),
                value: Gender.MAN,
                groupValue: _gender,
                onChanged: (value) {
                  setState(() {
                    _gender = value;
                  });
                },
              ),
              RadioListTile(
                title: Text('여자'),
                value: Gender.WOMEN,
                groupValue: _gender,
                onChanged: (value) {
                  setState(() {
                    _gender = value;
                  });
                },
              ),
            ],
          )
        ),
      ),
    );
  }
}
```

---

+ 결과 화면

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-28.png)

---

## 4. DropDownButton

- 여러 아이템 중 하나를 고를 수 있는 콤보박스 형태의 위젯입니다.
1. **`value 프로퍼티`에 표시할 값을 지정합니다.**
2. **`items 프로퍼티`에는 표시할 항목을 `DropdownMenuItem 클래스의 인스턴스들을 담은 리스트`로 지정해야 합니다.**
- 상태를 가지므로 `StatefulWidget으로 작성`합니다.

---

+ Sample Code

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(MyApp());
}

class MyApp extends StatelessWidget {
  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: MyHomePage(),
    );
  }
}

//수정 요소
class MyHomePage extends StatefulWidget {

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {

  final _valueList = ['첫 번째', '두 번째', '세 번째'];
  var _selectedValue = '첫 번째';

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Radio / RadioListTile'),
      ),
      body: DropdownButton(
        value: _selectedValue,
        items: _valueList.map(  // 11111
            (value) {
              return DropdownMenuItem(
                value: value,
                child: Text(value),
              );
            },
        ).toList(),  // 22222
        onChanged: (value) {
          setState(() {
            _selectedValue = value;
          });
        },
      ),
    );
  }
}
```

---

+ 결과 화면 

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-29.png)

---

![Desktop View](/assets/img/post/flutter/2021-03-25-widget-30.png)

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)
