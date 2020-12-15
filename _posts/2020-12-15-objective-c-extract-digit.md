---
title: Objective-C) NSString에서 숫자, 문자만 추출하기
author: cotchan 
date: 2020-12-15 20:39:21 +0800 
categories: [Objective-C, Objective-C_NSString]
tags: [objective-c]    
---

아래 출처를 참고하여 작성한 포스팅입니다.    

---

## 1. 숫자만 추출하는 방법


```ruby
# case.1
- (NSString*)getDigitFromString:(NSString*)string {

    NSString* digitPart = [[string componentsSeparatedByCharactersInSet: [[NSCharacterSet decimalDigitCharacterSet] invertedSet]] componentsJoinedByString:@""];
    
    return digitPart;
}
```

```ruby
# case.2
- (NSString*)getDigitFromString:(NSString*)string {
    
    NSString* digitPart = [[string componentsSeparatedByCharactersInSet: [[NSCharacterSet characterSetWithCharactersInString:@"0123456789"] invertedSet]] componentsJoinedByString:@""];
    
    return digitPart;
}

```

---

## 2. 특정 문자만 추출하는 방법

```ruby
- (NSString*)getStringFromString:(NSString*)string {
    
    # KMGTHB contains KB, MB, GB, TB, HB alphabet
    NSString* alphabetPart = [[string componentsSeparatedByCharactersInSet: [[NSCharacterSet characterSetWithCharactersInString:@"KMGTHB"] invertedSet]] componentsJoinedByString:@""];
    
    return alphabetPart;
}
```


---

+ 출처
	+ [iPhone - NSString 숫자만 , 숫자빼고 제거](http://jmkook77.blogspot.com/2012/02/iphone-nsstring.html)
	+ [문자열에서 숫자만 추출하기](https://kwonsaw.tistory.com/91)

