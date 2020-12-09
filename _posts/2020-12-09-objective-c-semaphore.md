---
title: Objective-C) Semaphore 사용 방법 
author: cotchan 
date: 2020-12-09 11:21:21 +0800 
categories: [Objective-C, Objective-C_ETC]
tags: [objective-c]
---

아래 출처를 바탕으로 작성한 글입니다.     

## 세마포어란

(세마포어에 대한 자세한 설명은 아래 포스팅을 참고해주세요.)

`Counting semaphore`는 공유자원에 대한 접근을 제어하는 일종의 FLAG입니다.    
자원이 몇 개로 한정되었을 때, 이 자원에 접근하는 걸 컨트롤하는데 사용됩니다.               
semaphore에 접근하는 함수는 2가지입니다.    

자원을 사용하고, 사용한 자원을 반납하는데 `wait()`과 `signal()` 함수를 사용합니다.    
**wait()함수를 통해 자원을 사용하면, 카운팅 세마포어의 갯수가 -1이 됩니다.**       
**signal()함수를 통해 사용한 자원을 반납하면, 카운팅 세마포어의 갯수가 +1이 됩니다.**    


+ 자원을 사용하는 함수의 이름이 `wait`인 이유는 공유 자원을 사용하려고 했는데, `사용가능한 공유자원이 없으면 대기한다`는 의미입니다. 
+ 자원을 반납하는 `signal` 함수의 의미는 `wait 상태인 세마포어를 깨운다` 정도의 의미로 생각하면 될 것 같습니다. 
    


---


## Objective-C에서의 semaphore 사용

Objective-C에서는 아래와 같은 문법으로 sempahore를 사용할 수 있습니다.

```ruby
# 세마포어 생성
dispatch_semaphore_create(long count);

# 자원을 사용합니다.
# 자원을 사용할 수 없는 경우, 지정된 시간만큼 세마포어에 blocking을 겁니다.
long dispatch_semaphore_wait(dispatch_semaphore_t semaphore, dispatch_time_t timeout);

# blocking을 해제합니다.
long dispatch_semaphore_signal(dispatch_semaphore_t semaphore);
```



---


## Objective-C에서의 세마포어 사용 예시

아래와 같은 순서로 세마포어를 사용할 수 있습니다.    

1. 세마포어 생성    
2. business logic 함수 실행    
3. semaphore_wait() 함수 실행을 통해 공유자원 사용    
4. business logic 함수의 callback에서 semaphore_signal() 호출을 통해 사용한 공유자원 반납    


```ruby
# 1st
dispatch_semaphore_t sema = dispatch_semaphore_create(0);
    
# 2nd
[manager POST:URL
   parameters:requestDictionary
     progress:nil
      success:^(NSURLSessionDataTask * _Nonnull task, id  _Nullable responseObject) 
      {
      	# 4th: 성공 시 callback
        dispatch_semaphore_signal(sema);
      } 
      failure:^(NSURLSessionDataTask * _Nullable task, NSError * _Nonnull error) 
      {
	# 4th: 실패 시 callback
	dispatch_semaphore_signal(sema);
      }];
    
# 3rd
dispatch_semaphore_wait(sema, DISPATCH_TIME_FOREVER);

```



---

+ 출처
	+ [[iOS] [기본] GCD(Grand Central Dispatch) 사용하기.](https://padgom.tistory.com/entry/iOS-%EA%B8%B0%EB%B3%B8-GCDGrand-Central-Dispatch-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0)
	+ [[운영체제]세마포어(semaphore) 완전 쉬운 이해! wait(), signal(), 이진, 계수형](https://jhnyang.tistory.com/101)
