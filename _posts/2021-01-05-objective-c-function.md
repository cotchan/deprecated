---
title: Objective-C) 함수 사용방법
author: cotchan 
date: 2021-01-05 14:39:21 +0800 
categories: [Objective-C, Objective-C_Basic] 
tags: [objective-c] 
---

내용이 계속 추가 될 예정입니다. :)    
저도 다른 분 포스팅을 보고 공부했던 내용인데 출처가 기억이 나지 않아서 적지 못하는 점 양해부탁드립니다.    
 
---

## 1. 함수 생성

+ **`-`**
    + `인스턴스 메소드`를 의미합니다.
+ **`+`**
    + `클래스 메소드`를 의미합니다.

+ 차이점1. `함수 생성 방법`에서의 차이가 납니다.

```ruby
- (void)helloObjc { 
	NSLog(@"hello");
}

+ (void)helloObjc2 {
	NSLog(@"hello");
}
```

+ 차이점2. `함수 호출방식`에서의 차이가 납니다.

```ruby
[self helloObjc];
[Test helloObjc2];
```

+ 인스턴스 메소드
    + `self` 키워드를 이용해서 호출합니다. 
+ 클래스 메소드
    + `클래스명`을 이용해서 호출합니다.


---


## 2. 함수 선언

+ 함수 선언을 하지 않으면 만든 함수가 보이지 않습니다.

```ruby
@interface Test : UIViewController

- (void)helloObjc;
+ (void)helloObjc2;

@end
```

---


## 3. 함수 호출(실행)

1. **인스턴스 메소드 실행**

```ruby
[self helloObjc];
```


2. **클래스 메소드 실행**

```ruby
Test *vc = [[Test alloc] init];
[vc helloObjc];
```

+ Test 클래스의 객체 vc를 생성하고 vc의 함수 helloObjc를 실행시켰습니다. 
+ `변수 선언 시 *가 들어가는 것`들은 대부분 클래스형이기 때문에 메모리를 할당해줘야 합니다.
+ 따라서 `alloc`을 이용해서 생성합니다.
    + 그래서 대부분 클래스를 생성할 때 `alloc init`을 사용하고 있습니다.
+ **`alloc`**
    + alloc은 `NSObject`에 있습니다.
    + 최상위 클래스이기 때문에 모든 클래스가 기본적으로 다 가지고 있습니다.
    + `init`을 통해서 객체를 생성합니다.
    
```ruby
NSString *str = [[NSString alloc] init];
NSInteger a;

NSArray *arr = [[NSArray alloc] init];
Test *vc = [[Test alloc] init];
```

---

## 4. 매개변수 함수

```ruby
- (반환형)함수이름: (자료형)내부변수명 {}

# e.g.
- (void)helloWithName: (NSString *)str {}
```

```ruby
- (void)helloObjc {
	NSLog(@"hello");	
}

+ (void)helloObjc2 {
	NSLog(@"hello");
}

# 매개변수 1개
- (void)helloWithName: (NSString *)str {
	NSLog(@"%@씨 안녕하세요", str);
}


# 매개변수 2개 외부 변수 생략
- (void)helloWithName2: (NSString *)str1 :(NSString *)str2 {
	NSLog(@"%@와 %@ 안녕", str1, str2);
}


# 매개변수 2개 외부 변수를 표시한 케이스
- (void)helloWithName3: (NSString *)str1 name2:(NSString *) str2 {
	NSLog(@"%@와 %@ 안녕", str1, str2);
}
```

---

## 4-1. 매개변수 함수 실행

```ruby
[self helloObjc];
[Test helloObjc2];

[self helloWithName: @"a"];
[self helloWithName2: @"a"  :@"b"];;
[self helloWithName3: @"a" name2: @"b"];
```


---

## 4-2. 인수 한 개 짜리 함수 호출방법

```ruby
[circle setFillColor: kRedColor];
```

+ **매개변수가 1개일 때**
    + 매개변수가 1개 일때는 `외부변수명`이 따로 없습니다. 그래서 함수호출 코드를 보면 함수이름 다음에 바로 입력을 받습니다.
    + 함수명을 지을 때 관례적으로 funcWithName과 같이 `With`을 붙여 '어떤 변수를 필요로 한다'고 표시합니다.

---


## 4-3. 인수 두 개 짜리 함수 호출방법

```ruby
[textThing setStringValue: @"hello there" color: kBlueColor];
```

+ setStringValue와 color는 `인수 이름`을 의미합니다.
+ @"hello there"와 kBlueColor는 `실제로 전달되는 인수`를 의미합니다.

+ **매개변수가 2개일 때**
    + 2개일때는 내부변수명과 외부변수명을 두 개 다 사용할 수 있습니다.
    + 또한 `외부변수명`도 `생략 가능`합니다.
    + **외부 변수명이 없어도 `:`는 꼭 써줘야 합니다.**
        + **따라서 `:`로 매개변수의 갯수를 알 수 있습니다.**

---


## 4-4. 메소드와 인수의 관계 (미세먼지 팁)

+ 메소드가 인수를 받아들인다면 콜론을 갖고, 인수를 받지 않는다면 콜론도 없습니다.

```ruby
- (void) scratchTheCat; # (X)
- (void) scratchTheCat  # (O)

- (void) scratchTheCat: (CatType) critter; # (O)
```


---
