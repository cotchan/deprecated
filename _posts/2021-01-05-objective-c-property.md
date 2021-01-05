---
title: Objective-C) @property
author: cotchan
date: 2021-01-05 15:45:21 +0800
categories: [Objective-C, Objective-C_Basic]
tags: [objective-c]
---

내용이 계속 추가 될 예정입니다. :)    
여러 포스팅을 참고하여 본인 공부목적으로 작성한 글입니다.    

---

## 1. @property

+ Objective-C의 변수들은 `protected` 타입입니다.
    + public은 잘 사용하지 않습니다.
+ getter와 setter를 사용하여 변수를 관리합니다.

+ `@property`라는 속성을 사용하는데, 자동으로 getter, setter를 만들어줍니다.
+ 또한 `.`을 통한 접근과 `[]`을 이용한 접근이 둘 다 가능합니다.

---

## 2. 사용 예시

+ 선언은 간단합니다. 헤더파일에 아래와 같이 변수를 추가해주면 끝입니다.

```ruby
# Test.h
@interface Test : UIViewController

- (void)helloObjc;
+ (void)helloObjc2;

# property 사용
@property int count;

@end
```

+ **`implementation`안에 변수를 선언할 수는 있지만 초기화는 할 수 없습니다.**
+ **그렇기 때문에 따로 초기화를 해줘야 합니다.**

```ruby
@implementation Test2 {
	NSString *str; //변수 선언
}

- (void)viewDidLoad {

	[super viewDidLoad];
	# Do any additional setup after loading the view.

	str = @"abc"; 

	Test *vc = [[Test alloc] init];
	[vc helloObjc];

	# 방법1 .으로 접근
	NSInteger c = vc.count
	vc.count = 3;

	# 방법2 getter setter
	NSInteger c = [vs count];
	[vc setCount: 3];

}
@end
```

---

+ 출처
    + [[Objective-C] 기초문법 - 변수, 함수, 객체 사용해보기!!](https://nsios.tistory.com/4)
