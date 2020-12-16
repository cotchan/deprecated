---
title: CleanCode) 오류 처리
author: cotchan 
date: 2020-12-16 08:52:21 +0800
categories: [CleanCode] 
tags: [cleancode]
---

CleanCode 책을 바탕으로 정리한 내용입니다.            
개인 공부 목적으로 작성한 글이며 지속적으로 업데이트할 예정입니다.        

---

## 오류 코드보다 예외를 사용하라

오류가 발생하면 예외를 던지는 편이 낫습니다. 그래야 호출자의 코드가 더 깔끔해집니다. 그래야 `논리가 오류 처리 코드와 뒤섞이지 않습니다.`

## 1. 오류를 사용하는 경우

```java
public class DeviceController {
    ...
    public void sendShutDown() {
        DeviceHandle handle = getHandle(DEV1);

        //디바이스 상태를 점검한다.
        if (handle != DeviceHandle.INVALID) { 
            //레코드 필드에 디바이스 상태를 저장한다.
            retrieveDeviceRecord(handle);

            //디바이스가 일시정지 상태가 아니라면 종료한다.
            if (record.getStatus() != DEVICE_SUSPENDED) {
                pauseDevice(handle);
                clearDeviceWorkQueue(handle);
                closeDevice(handle);
            }
            else {
                logger.log("Device suspended. Unable to shut down");
            }
        }
        else {
            logger.log("Invalid handle for: " + DEV1.toString());
        }
    }
    ...
}
```

+ 위와 같은 방법을 사용하면 호출자 코드가 복잡해집니다. 
+ 함수를 `호출한 즉시 오류를 확인해야 하기 때문`입니다.
	+ 불행히도 이 단계는 잊어버리기가 쉽습니다. 
+ **그래서 오류가 발생하면 예외를 던지는 편이 낫습니다.** 
+ 그러면 호출자 코드가 더 깔끔해집니다. 논리가 오류 처리 코드와 뒤섞이지 않기 때문입니다.


---

## 2. 예외를 사용하는 경우

```java
public class DeviceController {
    ...
    public void sendShutDown() {
        try { 
            tryToShutDown();
        } 
        catch (DeviceShutDownError e) {
            logger.log(e); 
        }
    }

    private void tryToShutDown() throws DeviceShutDownError {
        DeviceHandle handle = getHandle(DEV1);
        DeviceRecord record = retrieveDeviceRecord(handle);
        pauseDevice(handle);
        clearDeviceWorkQueue(handle);
        closeDevice(handle);	 
    }

    private DeviceHandle gethandle(DeviceID id) {
        ...
        throw new DeviceShutDownError("Invalid handle for: " + id.toString()); 
        ...
    }
	
    ...
}
```

---

## 1st. Try-Catch-Finally 문부터 작성하라

+ **예외가 발생할 코드를 짤 때는 try-catch-finally문으로 시작하는 편이 낫습니다.** 
+ 그러면 try 블록에서 무슨 일이 생기든지 호출자가 기대하는 상태를 정의하기가 쉬워집니다.
+ try 블록에서 무슨 일이 생기든지 catch 블록은 프로그램 상태를 일관성 있게 유지해야 합니다.
 

+ 다음은 파일이 없으면 예외를 던지는지 알아보는 단위 테스트입니다.

```java
//part.1
@Test(expected = StorageException.class)
public void retrieveSectionShouldThrowOnInvalidFileName() { 
    sectionStore.retrieveSection("invalid - file");
}
```

+ 단위 테스트에 맞춰서 다음 코드를 구현합니다. 
+ 하지만 다음 코드는 예외를 던지지 않으므로 단위 테스트는 실패합니다. 

```java
//part.2
public List<RecordedGrip> retrieveSection(String sectionName) { 
    //실제로 구현할 때까지 비어 있는 더미를 반환합니다.
    return new ArrayList<RecordedGrip>();
}
```

+ 잘못된 파일에 접근을 시도하도록 아래와 같이 구현을 변경해봅니다.
+ 아래 코드는 예외를 던지므로 이제는 테스트가 성공합니다. 

```java
//part.3
public List<RecordedGrip> retrieveSection(String sectionName) { 
    try { 
        FileInputStream stream = new FileInputStream(sectionName);
    }
    catch (Exception e) {
        throw new StorageException("retrieval error", e); 
    }
    
    return new ArrayList<RecordedGrip>();
}
```

+ 코드가 예외를 던지므로 이제는 테스트가 성공합니다.
+ **이 시점에서 리팩터링이 가능합니다.**
+ `catch 블록에서 예외 유형을 좁혀` 실제로 FileInputStream 생성자가 던지는 FileInputStream 생성자가 던지는 FileNotFoundException을 잡아냅니다.

```java
//part.4
public List<RecordedGrip> retrieveSection(String sectionName) {
    try {
        FileInputStream stream = new FileInputStream(sectionName);
	stream.close();
    }
    catch (FileNotFoundException e) {
        throw new StorageException("retrieval error", e);
    }
    
    return new ArrayList<RecordedGrip>();
}
```

try-catch 구조로 범위를 정의했으므로 TDD를 사용해 필요한 나머지 논리를 추가합니다.     
나머지 논리는 FileInputStream을 생성하는 코드와 close 호출문 사이에 넣으며 오류나 예외가 전혀 발생하지 않는다고 가정합니다.    
    
+ **`먼저 강제로 예외를 일으키는 테스트 케이스를 작성`한 후 테스트를 통과하게 코드를 작성하는 방법을 권장합니다.**
+ 그러면 자연스럽게 try 블록의 트랜잭션 범위부터 구현하게 되므로 범위 내에서 트랜잭션의 본질을 유지하기 쉬워집니다.


---

## 2nd. 미확인(Unchecked) 예외를 사용하라



---

+ 출처	
	+ 로버트 C. 마틴, 『Clean Code』, 인사이트(2013), p68-p94
