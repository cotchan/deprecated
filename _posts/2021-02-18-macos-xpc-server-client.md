---
title: macOS) XPC Service & Service Client 구조 
author: cotchan
date: 2021-02-18 17:42:00 +0800
categories: [Macos, Macos_XPC]
tags: [macos]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

![Desktop View](/assets/img/post/macos/2021-02-18-xpc-server-client.jpg)

---

## 1. XPC Service (Server)

+ XPC 통신을 통해 Service를 제공하는 쪽을 의미합니다.
+ **서비스를 제공하는 쪽에서 Listener를 사용합니다.**

1. Create a `Listener`
2. Set `delegate object` in the listener
3. call resume method
  + start listening for connections  
  + resume 메서드는 listener에게 커넥션을 리스닝하라고 지시합니다.
4. call `RunLoop.main.run()`

+ **컴포넌트**

```c++
main.swift
ServiceDelegate.swift
YourServiceXPCProtocol.swift
YourServiceXPC.swift
```

---

## 2. Service Client (Client)

+ XPC 통신을 통해 Service를 사용하는 쪽을 의미합니다.

1. Create a `connection` with XPc Service
2. Get the real XPC Service `through the Protocol`
3. 이제 자유롭게 XPC Service를 사용하면 됩니다.

+ **컴포넌트**

```c++
main.swift
//Server와 동일한 코드입니다.
YourServiceXPCProtocol.swift
```


---

+ 출처
  + [Creating a Launch Agent that provides an XPC service on macOS using Swift](https://rderik.com/blog/creating-a-launch-agent-that-provides-an-xpc-service-on-macos/)
