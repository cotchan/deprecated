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

## 1. @property란

+ 해당 프로퍼티에 접근자 메서드가 필요함을 컴파일러에게 알려줍니다.
+ Objective-C의 기본 필드는 기본적으로 `protected` 타입입니다.
    + @private이나 @public 섹션을 이용해서 접근 특성을 바꿀 수 있습니다.
+ getter와 setter를 사용하여 변수를 관리합니다.

+ `@property`라는 속성을 사용하는데, 자동으로 getter, setter를 만들어줍니다.
+ 또한 `.`을 통한 접근과 `[]`을 이용한 접근이 둘 다 가능합니다.

---

## 2. @property의 특성

```ruby
#import <UIKit/UIKit.h>

@interface ViewController : UIViewController {
	UITextField *notesFiled_;
}

@property (weak, nonatomic) IBOutlet UITextView *tweetTextView;

- (IBAction)postItButtonPressed: (id)sender;

@end
``` 

```java
@interface
클래스 인터페이스 선언이 시작되는 것을 알리는 키워드

ViewController
선언하는 클래스 이름

: UIViewController
선언하는 클래스가  상속받는 클래스 이름. Objective-C는 다중 상속을 지원하지 않습니다.

UITextField *notesFiled_
클래스 인터페이스에 정의되는 인스턴스 변수입니다.
기본적으로 모든 필드는 protected 접근 특성을 가집니다.
@private나 @public 섹션을 이용해서 접근 특성을 바꿀 수 있습니다.
```

+ 프로퍼티 (weak, nonatomic)
    + 프로퍼티는 효율성과 관련이 있습니다.
    + 프로퍼티를 이용하면 다른 객체가 우리 클래스 데이터와 상호작용하는 방법을 제한할 수 있습니다.
    + 프로퍼티 특성은 세 종류로 구분됩니다.
        + `쓰기 가능 여부` / `setter 동작 방법` / `원자성`
    + `weak` 자리에 오는 특성들
        + `read/write`
            + default 값으로 누구나 프로퍼티를 고칠 수 있습니다.
        + `read-only`
            + 본인 이외에 프로퍼티를 고칠 수 없습니다.
        + `strong`
            + 객체 값을 직접 사용할 때 필요합니다.
            + 객체를 참조하는 강한 포인터가 있으면 활성 상태가 유지됩니다.
        + `weak`
            + 참조된 객체를 alive 상태로 유지하지 않습니다.
        + `copy`
            + 단순히 값이 아니라 어떤 값의 사본이 필요할 때 사용합니다.
    + `nonatomic`
        + nonatomic 특성을 추가하면 뮤텍스 관련 기능을 컴파일러가 추가하지 않습니다.
        + 멀티스레드 환경에서 사용될 가능성이 없다면 원자성을 유지하는 것이 낭비이기 때문에 상황을 고려하여 nonatomic특성을 추가해주면 됩니다.

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
        # 변수 선언
	NSString *str;
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
