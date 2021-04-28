---
title: dart) getter, setter
author: cotchan
date: 2021-04-24 16:37:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**

---

## 1. Getter & Setter

+ **`게터, 세터 이름은 마음대로` 지어줘도 됩니다.**

```dart
//person.dart
class Person {

  String _name;

  //Getter 이름은 마음대로 지어줘도 됩니다.
  String get name => _name;

  //Setter 이름은 마음대로 지어줘도 됩니다.
  String get hello => _name;


  //Setter 이름은 마음대로 지어줘도 됩니다.  
  set name(String name) => _name = name;

  //Setter 이름은 마음대로 지어줘도 됩니다.
  set setName(String name) => _name = name;

}

//
import 'person.dart'

Person person = Person();

//use setter
person.name = 'Kim';
person.setName = 'Kim';

//use getter
print(person.name);  //Kim
print(person.hello);  //Kim
```

---

+ 출처
  + [다트 Getter & Setter](https://brunch.co.kr/@mystoryg/127)
