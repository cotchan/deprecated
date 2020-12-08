---
title: Objective-C) File 정보 가져오기(NSFileManager)
author: cotchan 
date: 2020-12-08 11:12:21 +0800 
categories: [Objective-C, Objective-C_API] 
tags: [objective-c] 
---

계속 업데이트할 예정입니다. :)

## Get File Size

```ruby
-(unsigned long long)getFileSize:(NSString *)filePath
{
	unsigned long long fileSize;
	fileSize = [[[NSFileManager defaultManager] attributesOfItemAtPath:filePath error:nil] fileSize];
	return fileSize;
}
```


---


## Get File Modification Date

```ruby
-(NSDate *)getFileModificationDate:(NSString *)filePath
{
	NSDate *fileModificationDate;
	fileModificationDate = [[[NSFileManager defaultManager] attributesOfItemAtPath:self.filePath error:NULL] fileModificationDate];
	return fileModificationDate;
}
```
