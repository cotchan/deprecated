---
title: macOS) XPC Service Agent 만드는 방법 
author: cotchan
date: 2021-02-17 17:42:00 +0800
categories: [Macos, Macos_XPC]
tags: [macos]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. XPC Service Agent 만들기

+ 예시는 `rdConsoleSequencer`라는 프로젝트를 만드는 것으로 진행합니다. 
  + 자세한 사항은 `출처` 참고

```bash
$ mkdir rdConsoleSequencer
$ cd rdConsoleSequencer
$ swift package init --type executable
```
---

+ **XPC Service Agent의 로직은 아래와 같습니다.**
  1. create `a listener`
  2. set `delegate object` in the listener
  3. call resume method
    + resume method means that to start listening for connections
  4. RunLoop.main.run()

---

## 1-1. main.swift

```swift
//main.swift
import Foundation

let delegate = ServiceDelegate()
let listener = NSXPCListener(machServiceName: "com.rderik.ConsoleSequencerXPC" )
listener.delegate = delegate;
listener.resume()
RunLoop.main.run()
```

---

## 1-2. ServiceDelegate.swift

```swift
//ServiceDelegate.swift
import Foundation

class ServiceDelegate : NSObject, NSXPCListenerDelegate {
    func listener(_ listener: NSXPCListener, shouldAcceptNewConnection newConnection: NSXPCConnection) -> Bool {
        let exportedObject = ConsoleSequencerXPC()
        newConnection.exportedInterface = NSXPCInterface(with: ConsoleSequencerXPCProtocol.self)
        newConnection.exportedObject = exportedObject
        newConnection.resume()
        return true
    }
}
```

---

## 1-3. YourServiceProtocol.swift

+ XPC Service가 제공할 기능들을 의미합니다.
  + The interface of our service

```swift
//ConsoleSequencerXPCProtocol.swift
import Foundation

@objc(ConsoleSequencerXPCProtocol) protocol ConsoleSequencerXPCProtocol {
  func toRedString(_ text: String, withReply reply: @escaping (String) -> Void)
  func toGreenString(_ text: String, withReply reply: @escaping (String) -> Void)
}
```

---

## 1-4. YourService.swift

+ 1-3의 Protocol을 실제로 구현한 클래스입니다.
  + The Implement of our service 
+ Client가 Connection을 맺고 실제로 사용하는 기능을 구현하는 클래스입니다.

```swift
//ConsoleSequencerXPC.swift
import Foundation

@objc class ConsoleSequencerXPC: NSObject, ConsoleSequencerXPCProtocol{

  func toRedString(_ text: String, withReply reply: @escaping (String) -> Void) {
    reply("\u{1B}[31m\(text)\u{1B}[0m")
  }
  func toGreenString(_ text: String, withReply reply: @escaping (String) -> Void) {
    reply("\u{1B}[32m\(text)\u{1B}[0m")
  }
}
```

---

## 1-5. 프로젝트 빌드 

+ 위에서 작성한 XPC Service Agent 코드를 빌드합니다.

```bash
$ swift build
```

---

## 1-6. build 후 Agent로 등록

+ **가장 중요한 단계**

+ 프로젝트 빌드 후 생성되는 실행 파일 경로를 알고 있어야 합니다.
  + 기본 경로: `{YOUR_PROJECT_NAME}/.build/debug/{YOUR_PROJECT_NAME}.app`

+ **`~/Library/LaunchAgents/` 경로에 Agent에 대한 `.plist` 파일을 생성해야합니다.**
  + Agent는 우리가 작성한 swift code를 의미합니다.

---

+ `com.rderik.rdconsolesequencerxpc.plist` 파일 예시
  + Program key 값은 XPC Service swift 코드를 빌드한 후 실행 파일이 생성되는 위치입니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
  <string>com.rderik.ServiceProviderXPC</string>
  <key>Program</key>
  <string>PATH_TO_YOUR_PROJECT/rdConsoleSequencer/.build/debug/rdConsoleSequencer</string>
    <key>MachServices</key>
    <dict>
        <key>com.rderik.ConsoleSequencerXPC</key>
        <true/>
    </dict>
</dict>
</plist>
``` 

+ **중요사항**
  + **`MachServices` 필드**
    + Mach Service field on the Plist defines the Mach services **exposed by the agent.**
      + MachServices Key값은 외부에서 Agent로 보일 이름을 의미합니다.
    + The name of the service is the same name we registered when creating the listener in the `main.swift`
      + listener로 등록하고, 외부에서 connection을 생성하는 Agent명은 모두 MachServices 필드 이름로 식별합니다.
  + **.plist파일에 `interval` 필드, `run at launch` 필드가 필요 없습니다.** 
    + 그 이유는 MachService는 요청이 시작될 때 on-demand로 launchd가 로드합니다.

---

## 1-7. Agent 실행

```bash
//로드(실행)
$ launchctl load ~/Library/LaunchAgents/com.rderik.rdconsolesequencerxpc.plist
```

---

```bash
//실행 여부 확인
$ launchctl list | grep rderik
```

---

+ 아래와 같이 커맨드창이 나오면 XPC Service가 Agent 형태로 실행된 것입니다.

```bash
-       0       com.rderik.ServiceProviderXPC
```


---

## 2. XPC Service Client 만들기

+ rdConsoleSequencerClient 프로젝트를 만듭니다.
  + Consuming our XPC service

```bash
$ mkdir rdConsoleSequencerClient
$ cd rdConsoleSequencerClient
$ swift package init --type executable
```

---

+ **XPC Service Client의 로직은 아래와 같습니다.**
  1. create `a connection` with XPC Service
  2. get the real XPC Service through `the Protocol` with connection
  3. 이제 Client는 XPC Service를 사용하면 끝입니다. 

---

## 2-1. main.swift

```swift
import Foundation

print("Welcome to our simple REPL")
let connection = NSXPCConnection(machServiceName: "com.rderik.ConsoleSequencerXPC")
connection.remoteObjectInterface = NSXPCInterface(with: ConsoleSequencerXPCProtocol.self)
connection.resume()

let service = connection.remoteObjectProxyWithErrorHandler { error in
    print("Received error:", error)
} as? ConsoleSequencerXPCProtocol

while true {
  print("Insert text: ", terminator: "")
  let text = readLine(strippingNewline: true)!
  service!.toRedString(text) { (texto) in
    print(texto, terminator: " ")
  }
  service!.toGreenString(text) { (texto) in
    print(texto)
  }
  if text == "exit" {
    break
  }
}
```

---

## 2-2. YourServiceProtocol.swift

+ Client는 Agent의 Protocol만 알고 있으면 됩니다.
  + main 함수에서 볼 수 있듯이 remoteObjectProxy를 사용합니다.
    + 즉, Client는 Protocol을 통해서 실제 구현된 Service 클래스를 사용할 수 있습니다.

```swift
import Foundation

@objc(ConsoleSequencerXPCProtocol) protocol ConsoleSequencerXPCProtocol {
  func toRedString(_ text: String, withReply reply: @escaping (String) -> Void)
  func toGreenString(_ text: String, withReply reply: @escaping (String) -> Void)
}
```

---

## 2-3. XPC Agent 사용하기

```bash
$ swift run
```





---

+ 출처
  + [Creating a Launch Agent that provides an XPC service on macOS using Swift](https://rderik.com/blog/creating-a-launch-agent-that-provides-an-xpc-service-on-macos/)
