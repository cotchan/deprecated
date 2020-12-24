---
title: Swift) Wrapper 클래스 사용방법
author: cotchan
date: 2020-12-24 16:30:21 +0800
categories: [Swift, Swift_Basic]
tags: [swift]
---

## 1. wrapper 클래스 사용이유

+ **`cpp 라이브러리`를 `swift 환경`에서 사용하기 위해 Wrapper 클래스를 사용합니다.**


---

## 2. wrapper 클래스 적용 방법

## 2-1. 프로젝트 폴더 내 wrapper 폴더에 래퍼 클래스(obj-c.mm & .h) 파일 생성

![Desktop View](/assets/img/post/swift/2020-12-24-swift-wrapper-class-1.png)

```ruby
# FileMonWrap.h

#ifndef FileMonWrap_h
#define FileMonWrap_h

#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface FileMonWrap : NSObject

- (void) Start:(NSArray*) Paths;
- (void) Stop;

@end

NS_ASSUME_NONNULL_END


#endif /* FileMonWrap_h */
```

```ruby
# FileMonWrap.mm

#import "FileMonWrap.h"
#import "FileMonitorLib.hpp"

@implementation FileMonWrap

CFileMonitorLib fm;

- (void) Start:(NSArray*) Paths
{
    std::vector<std::string> vPaths;
    
    for(NSString *path in Paths)
    {
        vPaths.push_back([path UTF8String]);
    }
    
    # FileMonitorLib.cpp의 CFileMonitorLib::Start(const vector<string>& vPaths) 메서드 수행
    fm.Start(vPaths);
}

- (void) Stop
{
    # FileMonitorLib.cpp의 CFileMonitorLib::Stop() 메서드 수행 
    fm.Stop();
}

@end
```

## 2-2. (기존에 있는) 브릿지 헤더에 등록하기

```c++
//sample
#import "FileMonWrap.h"
```

![Desktop View](/assets/img/post/swift/2020-12-24-swift-wrapper-class-2.png)

```c++
//FDR-Bridging.h

#ifndef FDR_Bridging_h
#define FDR_Bridging_h

#import "FileMonWrap.h"

#endif /* FDR_Bridging_h */

```
---

## 2-3. Swift 코드에서 라이브러리(C++ 코드) 사용

+ 이제 `Swift 코드`를 통해 라이브러리`(C++ 코드)를 제어`할 수 있습니다.


+ 프로젝트 내 FileMonitor.swift 소스 파일 생성(테스트 용) 

```swift
//FileMonitor.swift

import Foundation

class FileMonitor
{
    var m_Paths: Array<String> = Array<String>()
    
    static private let _self = FileMonitor()
    static func getInstance() -> FileMonitor
    {
        return self._self
    }
    
    private init()
    {
    }
    
    func addPath(_ Path: String)
    {
        m_Paths.append(Path)
    }
    
    func removePath(_ Path: String)
    {
        for i in 0...m_Paths.count
        {
            if m_Paths[i] == Path
            {
                m_Paths.remove(at: i)
                break;
            }
        }
    }
    
    func removeAll()
    {
        m_Paths.removeAll()
    }
    
    func start()
    {
        FileMonWrap().start(m_Paths)
    }
    
    func stop()
    {
        FileMonWrap().stop()
    }
}
```


--- 
