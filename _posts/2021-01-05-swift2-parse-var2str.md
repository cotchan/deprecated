---
title: Swift2) 변수 <=> 문자열 파싱 방법
author: cotchan
date: 2021-01-05 14:24:21 +0800
categories: [Swift, Swift_ETC]
tags: [swift2]
---

## \() 사용하기

+ **`\()`을 사용하면 임의의 변수를 String으로 파싱할 수 있습니다.**

```swift
let num:Int = 2020
var str = \(num)
print(str)
//2020
```

---
