---
title: Objective-C) Objective-C 기본 기능 코드
author: cotchan 
date: 2020-12-08 20:54:21 +0800 
categories: [Objective-C, Objective-C_Basic] 
tags: [objective-c] 
---

내용이 계속 추가 될 예정입니다. :)    
저도 다른 분 포스팅을 보고 공부했던 내용인데 출처가 기억이 나지 않아서 적지 못하는 점 양해부탁드립니다.    
 
---


## 인수 한 개 짜리 함수 호출방법

```ruby
[circle setFillColor: kRedColor];
```


---


## 인수 두 개 짜리 함수 호출방법

```ruby
[textThing setStringValue: @"hello there" color: kBlueColor];
```

+ setStringValue와 color는 `인수 이름`을 의미합니다.
+ @"hello there"와 kBlueColor는 `실제로 전달되는 인수`를 의미합니다.


---


## 메소드와 인수의 관계 (미세먼지 팁)

+ 메소드가 인수를 받아들인다면 콜론을 갖고, 인수를 받지 않는다면 콜론도 없습니다.

```ruby
- (void) scratchTheCat; # (X)
- (void) scratchTheCat  # (O)

- (void) scratchTheCat: (CatType) critter; # (O)
```


---
