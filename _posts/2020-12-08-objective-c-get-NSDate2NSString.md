---
title: Objective-C) NSDate <=> NSString 
author: cotchan 
date: 2020-12-08 10:19:21 +0800 
categories: [Objective-C, Objective-C_API]
tags: [objective-c] 
---

아래 사이트를 참고하시면 NSDate를 어떻게 파싱하면 좋을지 확인할 수 있습니다.         
+ [NSDateFormatter](https://nsdateformatter.com/)


## NSDate => NSString

```ruby
//result: 2020-12-08 21:15:29
-  (NSString *)getStringFromDate:(NSDate *)date {
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    [dateFormatter setDateFormat:@"yyyy-MM-dd, HH:mm:ss"];
    [dateFormatter setLocale:[NSLocale localeWithLocaleIdentifier:[[NSLocale preferredLanguages] objectAtIndex:0]]];
    NSString *time = [dateFormatter stringFromDate:date];
    return time;
}
```

```ruby
//result: 2020-12-08
-  (NSString *)getYearMonthDayStringFromDate:(NSDate *)date {
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init];
    [dateFormatter setDateFormat:@"yyyy-MM-dd"];
    [dateFormatter setLocale:[NSLocale localeWithLocaleIdentifier:[[NSLocale preferredLanguages] objectAtIndex:0]]];
    NSString *time = [dateFormatter stringFromDate:date];
    return time;
}
```

```ruby
//result: 21:15:29
-  (NSString *)getHourMinuteSecondStringFromDate:(NSDate *)date {
    NSDateFormatter *dateFormatter = [[NSDateFormatter alloc] init;
    [dateFormatter setDateFormat:@"HH:mm:ss"];
    [dateFormatter setLocale:[NSLocale localeWithLocaleIdentifier:[[NSLocale preferredLanguages] objectAtIndex:0]]];
    NSString *time = [dateFormatter stringFromDate:date];
    return time;
}
```


---


## NSString => NSDate

추후 업데이트 예정입니다.    


---

+ 출처
	+ [NSDate를 NSString으로, NSString을 NSDate로 변환하는 방법.](https://entusapps.com/entry/NSDate%EB%A5%BC-NSString%EC%9C%BC%EB%A1%9C-NSString%EC%9D%84-NSDate%EB%A1%9C-%EB%B3%80%ED%99%98%ED%95%98%EB%8A%94-%EB%B0%A9%EB%B2%95)
	
