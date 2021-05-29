---
title: dart) ? 연산자(조건적 멤버 접근)
author: cotchan
date: 2021-04-08 22:38:00 +0800
categories: [Dart]
tags: [dart]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **계속 업데이트 할 예정입니다.**
+ **dart에서는 다양한 의미로 `?` 연산자를 사용합니다.**
+ **다만, 여기서는 `조건적 멤버 접근`용도로 사용하는 `?` 연산자에 대해서만 다룹니다.**

---

## 1. 조건적 멤버 접근연산자

+ **이 연산자는 `좌항이 null이면 null을 리턴`하고, `좌항이 null이 아니면 우항의 값을 리턴`합니다.**

+ 아래와 같은 형태로 되어 있습니다.

```
좌항?.우항
```

---

## 2. 예시 코드

+ **`student의 name이 null이면 null을 리턴`하고 아니면 `student.name의 length에 접근`합니다.**
  + 잘못된 메모리에 접근하는 것을 막기 위해 쓰입니다.

```dart
class Student {
    String name;
}
main(){
    Student student = Student();
    print(student.name?.length);
}
```

---

+ 출처
  + [다트(3) - 특이 연산자](https://sneakstarberry.github.io/posts/dart03-operand/)

