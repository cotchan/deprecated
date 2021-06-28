---
title: Android) LayoutInflater
author: cotchan 
date: 2021-06-25 11:23:21 +0800 
categories: [Android, Android_Basic] 
tags: [android] 
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. LayoutInflater?

+ LayoutInflater는 안드로이드에서 `View`를 만드는 가장 기본적인 방법입니다.
+ **`XML레이아웃 파일에서` 뷰를 생성할때는 `LayoutInflater`를 이용해야 합니다.**

---

## 2. getLayoutInflater

+ Activity에서는 LayoutInflater를 쉽게 얻어올 수 있도록 getLayoutInflater()를 제공합니다.

```java
void func(Activity screen) {
  LayoutInflater inflater = screen.getLayoutInflater()
}
```

---

## 3. inflate()

+ **`LayoutInflater` 객체의 `inflate 메서드`를 이용하여 `새로운 뷰를 생성` 할 수 있습니다.**

```java
inflate(resource: Int, root: ViewGroup?, attachToRoot: Boolean)
```

+ **`resource`**
  + View를 만들고 싶은 레이아웃 파일의 id입니다.

+ **`root`**
  + 생성될 View의 parent를 명시해줍니다.
  + null일 경우에는 `LayoutParams` 값을 설정할 수 없기 때문에 XML 내의 최상위 `android:layout_xxxxx` 값들이 무시되고 merge tag를 사용할 수 없습니다.

+ **`attachToRoot`**
  + `true` 로 설정해 줄 경우 `root`의 자식 `View`로 자동으로 추가됩니다. 이때 `root`는 `null` 일 수 없습니다.

---

+ 출처
  + [안드로이드 LayoutInflater 사용하기](https://medium.com/vingle-tech-blog/android-layoutinflater-b6e44c265408)
