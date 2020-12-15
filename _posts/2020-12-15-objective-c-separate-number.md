---
title: Objective-C) NSString 천단위 숫자구분 (, 붙여주기)
author: cotchan 
date: 2020-12-15 20:39:21 +0800 
categories: [Objective-C, Objective-C_NSString]
tags: [objective-c]    
---

아래 출처를 참고하여 작성한 포스팅입니다.    

---

## Separater 적용방법

```ruby
- (NSString*) moneyLine:(NSString*)string
{
	NSNumberFormatter *numberFormatter = [[NSNumberFormatter alloc] init];

	[numberFormatter setGroupingSeparator:@","];
	[numberFormatter setGroupingSize:3];
	[numberFormatter setUsesGroupingSeparator:YES];
	[numberFormatter setNumberStyle:NSNumberFormatterDecimalStyle];

	NSString *theString = [numberFormatter stringFromNumber:[NSNumber numberWithDouble:string.doubleValue]];
	return theString;
}
```


---

+ 출처
	+ [NSString 천단위 , 붙여주기 (돈)](https://goldcocoe.tistory.com/18)

