---
title: Objective-C) [Log] txt 파일로 저장하기(한 파일에 NSString 여러번 저장)
author: cotchan
date: 2021-03-22 13:10:21 +0800
categories: [Objective-C, Objective-C_Basic]
tags: [objective-c]
---

내용이 계속 추가 될 예정입니다. :)    
여러 포스팅을 참고하여 본인 공부목적으로 작성한 글입니다.    

---

## 1. SampleCode_1

+ **함수 설명**
  + trace.txt 파일에 logMessage(로그 메시지) 저장하는 코드

```ruby
+ (void)addTraceLog:(NSString*)logMessage {
    
    NSString *dirPath = [NSString stringWithFormat:@"/Users/%@/Library/Group Containers", NSUserName()];
    NSString *logPath = [dirPath stringByAppendingPathComponent:@"trace.txt"];
    
    NSFileManager *fileManager = [NSFileManager defaultManager];
    
    if ([fileManager fileExistsAtPath:logPath])
    {
        NSFileHandle *handle = [NSFileHandle fileHandleForWritingAtPath:logPath];
        [handle truncateFileAtOffset:[handle seekToEndOfFile]];
        [handle writeData:[logMessage dataUsingEncoding:NSUTF8StringEncoding]];
        [handle closeFile];
    }
    //파일이 존재하지 않으면 파일을 생성하고 해당 값을 저장
    else
    {
        [fileManager createFileAtPath:logPath contents:[logMessage dataUsingEncoding:NSUTF8StringEncoding] attributes:nil];
    }
}
```

---

## 2. SampleCode_2

```ruby
NSString *timeString = @"메렁";

NSString *path = [NSTemporaryDirectory()

stringByAppendingPathComponent:[NSString stringWithFormat:@"result.txt"]];

NSFileManager* fileMgr = [NSFileManager defaultManager];

if ([fileMgr fileExistsAtPath:path]) //파일이 존재하면 파일의 맨 끝을 찾아 그 곳에 해당 값을 저장
{

    NSFileHandle *handle = [NSFileHandle fileHandleForWritingAtPath:path];

    [handle truncateFileAtOffset:[handle seekToEndOfFile]];

    [handle writeData:[timeString dataUsingEncoding:NSUTF8StringEncoding]];

    [handle closeFile];

}
else // 파일이 존재하지 않으면 파일을 생성하고 해당 값을 저장
{
    [fileMgr createFileAtPath:path contents:[timeString dataUsingEncoding:NSUTF8StringEncoding] attributes:nil];
}
```

---

+ 출처
  + [[ios] 한 파일에 NSString 여러번 저장하기](https://life-shelter.tistory.com/86)
  + [파일로 쓰기, 파일 읽기](https://nightohl.tistory.com/entry/%ED%8C%8C%EC%9D%BC%EB%A1%9C-%EC%93%B0%EA%B8%B0)
  + [File Handling in Objective C](https://stackoverflow.com/questions/3692556/file-handling-in-objective-c)
