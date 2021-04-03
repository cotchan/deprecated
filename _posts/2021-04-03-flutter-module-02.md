---
title: Flutter) [Module] 폼 입력값 검증하기
author: cotchan
date: 2021-04-03 15:40:00 +0800
categories: [Flutter, Flutter_module]
tags: [flutter_module]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. Form, TextFormField

+ **입력값을 검증하는데는 `Form`과 `TextFormField 위젯`을 사용합니다.**
- `Form`과 `TextFormField`를 사용하면 회원 가입처럼 `사용자 입력값을 검증해야 할 때 유용`합니다.
+ **`TextFormField` 위젯은 TextField 위젯이 제공하는 기능에 추가로 `validator` 프로퍼티를 활용한 검증 기능도 제공합니다.**

---

## 2. 코드 예시

- 다음 코드는 버튼을 클릭하면 사용자가 입력한 값이 정해진 규칙에 맞는지 검사하는 예시입니다.  

```dart
import 'package:flutter/material.dart';

void main() => runApp(MyApp());


class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: '폼 검증 데모',
      home: Scaffold(
        appBar: AppBar(
          title: Text('폼 검증 데모'),
        ),
        body: MyCustomForm(),
      ),
    );
  }
}

class MyCustomForm extends StatefulWidget {
  @override
  _MyCustomFormState createState() => _MyCustomFormState();
}

class _MyCustomFormState extends State<MyCustomForm> {

  //Form 위젯에 유니크한 키 값을 부여하고 검증시 사용
  final _formkey = GlobalKey<FormState>();    //11111

  @override
  Widget build(BuildContext context) {
    return Form(    //22222
      key: _formkey,    //33333
      child: Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: <Widget>[
          TextFormField(    //44444
            validator: (value) {
              if (value.isEmpty) {
                return '글자를 입력하세요';
              }
              return null;
            },
          ),
          Padding(
              padding: const EdgeInsets.symmetric(vertical: 16.0),
              child: RaisedButton(
                onPressed: () {

                  //폼을 검증하여 통과하면 true, 실패하면 false 리턴
                  if (_formkey.currentState.validate()) {   //55555
                    Scaffold.of(context)
                        .showSnackBar(SnackBar(content: Text('검증 완료')));
                  }
                },
                child: Text('검증'),
              ),
          ),
        ],
      ),
    );
  }
}
```

---

## 3. 코드 해석

+ `11111`
  +

+ `22222`
  + **검증에는 `TextFormField 위젯을 사용`하며, `검증할 내용 전체를 Form 위젯으로 감쌉니다.`**

+ `33333`
  + **`Form` 위젯에는 유니크한 키를 지정해야 하며 GlobalKey<FormState> 인스턴스를 키로 사용합니다.**
 
+ `44444`
  + **검증에는 `TextFormField 위젯을 사용`하며, `검증할 내용 전체를 Form 위젯으로 감쌉니다.`**
  + TextFormField 위젯에는 validator 프로퍼티가 있으며 여기에는 입력된 값을 인수(value)로 받는 함수를 작성합니다.
  + **또한 검증 로직을 작성하는데, 에러 메시지를 문자열로 반환하거나 null을 반환하여 검증이 통과되었음을 정의할 수 있습니다.**

+ `55555`
  + **폼의 검증은 `_formKey.currentState.validate()로 수행`하며 true 또는 false 값을 반환합니다.**
  + 폼 안에 TextFormField 위젯이 여러 개 있어도 이 한 줄로 검증을 체크할 수 있습니다.

+ `SnackBar` 클래스는 하단에 메시지를 표시하는 클래스입니다.
  + 검증이 완료되면 '검증 완료'라는 메시지를 표시합니다.

---

## 4. 결과화면

+ 검증 미통과 시

![Desktop View](/assets/img/post/flutter/2021-04-02-module-03.png)

---

+ 검증 통과 시

![Desktop View](/assets/img/post/flutter/2021-04-02-module-04.png)

---

+ 출처
  + 오준석, 『오준석의 플러터 생존코딩』, 한빛미디어(2020)

