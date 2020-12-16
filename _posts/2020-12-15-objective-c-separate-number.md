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

## 사용 예시

파일 사이즈가 주어졌을 때 3자리씩 쉼표(,)를 찍는 코드. 단, 소수점은 고려X

```ruby
# before: 123456789MB
# after: 123,456,789MB

- (NSString*)setThousandSeparator:(NSString*)string {

    NSString* digitPart = [[string componentsSeparatedByCharactersInSet: [[NSCharacterSet decimalDigitCharacterSet] invertedSet]] componentsJoinedByString:@""];
    
    //KMGTHB contains KB, MB, GB, TB, HB alphabet
    NSString* alphabetPart = [[string componentsSeparatedByCharactersInSet: [[NSCharacterSet characterSetWithCharactersInString:@"KMGTHB"] invertedSet]] componentsJoinedByString:@""];
        
    NSNumberFormatter *numberFormatter = [[NSNumberFormatter alloc] init];
    [numberFormatter setGroupingSeparator:@","];
    [numberFormatter setGroupingSize:3];
    [numberFormatter setUsesGroupingSeparator:YES];
    [numberFormatter setNumberStyle:NSNumberFormatterDecimalStyle];
    NSString *separateDigitPart = [numberFormatter stringFromNumber:[NSNumber numberWithDouble:digitPart.doubleValue]];

    NSString *result = [separateDigitPart stringByAppendingString:alphabetPart];
    return result;
}
```

---

+ 출처
	+ [NSString 천단위 , 붙여주기 (돈)](https://goldcocoe.tistory.com/18)

