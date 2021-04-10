---
title: macOS) [Xcode] Unable to boot device Error Message
author: cotchan
date: 2021-04-10 17:00:21 +0800
categories: [Macos, Macos_Develop]
tags: [macos]
---

+ 아래 출처를 참고하여 작성한 글입니다.

---

## 1. iOS 시뮬레이터 실행 오류

+ `Simulator`를 실행 시 아래와 같은 오류가 발생하였습니다. 
  + Unable to boot device because it cannot be located on disk

---

+ **`문제 해결 방법`**

+ **Xcode와 시뮬레이터를 끈 뒤 아래의 두 명령어를 입력하면 됩니다.**

```bash
$ sudo killall -9 com.apple.CoreSimulator.CoreSimulatorService
$ rm -rf ~/Library/*/CoreSimulator
```

---

+ 출처
  + [[Xcode] Unable to boot device because it cannot be located on disk](https://doorganizedcoding.tistory.com/35)
