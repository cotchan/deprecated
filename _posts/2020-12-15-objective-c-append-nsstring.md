---
title: Objective-C) NSString 이어붙이기
author: cotchan 
date: 2020-12-15 20:00:21 +0800 
categories: [Objective-C, Objective-C_NSString]
tags: [objective-c]    
---

아래 출처를 참고하여 작성한 포스팅입니다.    

---

## 첫 번째 방법. stringByAppendingString

```ruby
# case.1
NSString *errorTag = @"Error: ";
NSString *errorString = @"premature end of file.";
NSString *errorMessage = [errorTag stringByAppendingString:errorString];

# case.2
NSMutableString *string = [NSMutableString stringWithString:@"String 1"];
[string appendString:@"String 2"];
```

---

## 두 번째 방법. stringWithFormat

```ruby
NSString *sample1 = @"Hello";
NSString *sample2 = @"Being appended";

NSString *complete = [NSString stringWithFormat:@"%@ %@", sample1, sample2];
```

---

+ 출처
	+ [How to append a NSString onto another NSString](https://stackoverflow.com/questions/11308352/how-to-append-a-nsstring-onto-another-nsstring)

