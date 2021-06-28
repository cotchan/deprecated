---
title: Android) R.layout
author: cotchan 
date: 2021-06-28 11:00:21 +0800 
categories: [Android, Android_Basic] 
tags: [android] 
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. R.layout

+ **`res/layout` 폴더안에 aaa.xml 파일을 만들면 자바코드에서 `R.layout.aaa` 형태로 사용이 가능합니다.**
+ 마찬가지로 이미 안드로이드 sdk안에 만들어진 몇개의 xml 파일들이 있습니다. **이 xml 파일들을 사용할때 `android.R.xxx 형태`로 쓰게 됩니다.**

```android
ArrayAdapter<String> Adapter = new ArrayAdapter<String>  (this, android.R.layout.simple_list_item_1, dataArr);

```

+ 위 코드의 의미를 해석하자면, 
  + **" 안드로이드가 미리 만들어 놓은 레이아웃 중에 simple_list_item_1.xml 파일을 읽어와라"**라는 뜻이 됩니다.


---

+ 출처
  + [[안드로이드/Android] 안드로이드 리스트뷰 기초](https://jwandroid.tistory.com/230)
