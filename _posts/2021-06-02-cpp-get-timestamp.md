---
title: C++) Timestamp 특정 Format으로 변환(feat.strftime, time2string)
author: cotchan
date: 2021-06-02 00:00:00 +0800
categories: [C++, C++_Tip]
tags: [c++]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 

```cpp
const std::string getCurrentDateTime() {
    time_t     now = time(0); //현재 시간을 time_t 타입으로 저장
    struct tm  timerStruct;
    char       buf[80];
    tstruct = *localtime(&now);
    strftime(buf, sizeof(buf), "%Y-%m-%d.%X", &timerStruct); // YYYY-MM-DD.HH:mm:ss 형태의 스트링

    return buf;
}
```

---

+ 출처
  + [strftime](https://modoocode.com/122)
  + [struct tm](https://modoocode.com/109)
  + [localtime](https://modoocode.com/120)
