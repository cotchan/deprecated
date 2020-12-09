---
title: Objective-C) Block Syntax
author: cotchan 
date: 2020-12-09 10:14:21 +0800 
categories: [Objective-C, Objective-C_Basic]
tags: [objective-c]
---

아래 출처 내용들을 참고하여 작성하였습니다.    

## 블록이란

`콜백 함수`의 Objective-C 버전입니다.    
블록(Block)은 C함수로 런타임에 생성되는 다이나믹 함수(Dynamic Function)입니다.    



---


## 블록 선언 방법 (Basic)

+ ^표시가 블록임을 선언합니다.
+ { } 안에 실행할 코드를 넣어줍니다.

```ruby
^{
	NSLog(@"This is a block");
}
```

+ objective-c 함수 선언방식

```ruby
void methodName: (void)name;
```

+ objective-c 블록함수 선언방식

```ruby
# 반환형 (^블록명) (파라미터 삽입);
void (^blockName)(void);

# 반환형을 바꿀 수도 있고, 파라미터를 여러 개 넣어줄 수도 있습니다.
double (^blockName)(double, double);
```


---


일반적으로 많이 쓰는 방식을 위주로 정리하였습니다.    


## Parameter Field로 정의하기

+ Parameter로 Block function을 사용하면 `callback` 방식으로 사용되며, 이 부분은 `비동기로 처리` 됩니다.

```ruby
fieldName:(RETURN_TYPE (^)(PARAMETERS...))parameterName
```

+ 정의 예제   

```ruby
- (nullable NSURLSessionDataTask *)POST:(NSString *)URLString
                             parameters:(nullable id)parameters
                               progress:(nullable void (^)(NSProgress *uploadProgress))uploadProgress
                                success:(nullable void (^)(NSURLSessionDataTask *task, id _Nullable responseObject))success
                                failure:(nullable void (^)(NSURLSessionDataTask * _Nullable task, NSError *error))failure;
```

+ 구현 예제

```ruby
 [manager POST:URL
          parameters:requestDictionary
          progress:nil
          success:^(NSURLSessionDataTask * _Nonnull task, 
				      id  _Nullable responseObject) 
	  {
		# success case
          }
	  failure:^(NSURLSessionDataTask * _Nullable task, 
				 NSError * _Nonnull error) 
	  {
		# failure case	
          }];
```

---


## Property로 정의하기

```ruby
RETURN_TYPE (^)(PARAMETERS ...)
```

+ 정의 예제

```ruby
@property (nonatomic, strong) void (^completionHandler)(NSData *data);
```


+ 구현 예제

```ruby
someClass.completionHandler = ^(NSData *data) {
	# doSomething
};

```


---


## typedef로 타입으로 만들기

추후 업데이트 예정

---


## 변수로 생성하기

추후 업데이트 예정


---

+ 출처
	+ [[Objective-C] Block Syntax 초간단정리](http://seorenn.blogspot.com/2016/07/objective-c-block-syntax.html)
	+ [[Objective-C] Block completion 구문 이해하기](https://medium.com/@twih1203/objective-c-block-completion-%EA%B5%AC%EB%AC%B8-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-17e08bbc9906)
	+ [[Objective-C] 블럭 문법 (Blocks Programming)](http://seorenn.blogspot.com/2014/04/objective-c-blocks-programming.html)


