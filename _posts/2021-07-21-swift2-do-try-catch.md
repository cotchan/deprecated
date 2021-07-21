---
title: Swift2) throw, throws & do-try-catch & try, try?
author: cotchan 
date: 2021-07-21 15:10:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift2] 
---

+ **개인 공부 목적으로 작성한 글입니다.**
+ **아래 출처를 참고하여 작성하였습니다.**
+ 내용이 계속 추가 될 예정입니다. :)

---

## 1. throw, throws

+ throw
  + **`발생한 오류를 던질 때 사용합니다.` : throw와 오류명 집어넣기(return하는 것과 유사)**
+ throws
  + **`오류가 발생할 수 있다는 것을 표현`합니다. : `throws`를 "->"앞에 표시**

```swift
//sample

// 오류가 나는 조건을 throws와 함께 배치
func getNextYearAndThrows(paramYear: Int) throws -> Int {
    guard paramYear <= 2020 else {
        throw DataError.overSizeYear
    }
    
    guard paramYear >= 0 else {
        throw DataError.incorrectData(part: paramYear)
    }
    
    return paramYear+1
}
```

---

## 2. do & try

+ `try` 문을 작성할 코드를 `do` 스코프로 감쌉니다.
+ **그래서 로직은 `do-catch` 형태로 예외가 발생할 수 있는 코드를 감쌉니다.**

```swift
//sample

func getNextYear(paramYear: Int) -> Int {
    var year: Int = 0
    do {
        year = try getNextYearAndThrows(paramYear: paramYear)
    } catch DataError.overSizeYear {
        print("년도 초과해서 입력하였습니다")
    } catch DataError.incorrectData(let part){
        print("입력한 값이 \(part)이므로 오류입니다.")
    } catch {
        print("default error catch")
    }
    
    return year
}

let a = getNextYear(paramYear: -999) // 입력한 값이 -999이므로 오류입니다.
```

---

## 3. catch

+ **catch에 `<오류타입>을 작성하지 않으면` `default문으로 처리`됩니다.**

---

## 4. try

+ **`예외 발생시 catch로 예외 정보 던지며`, 성공하는 경우 `unwrapping된 값 반환`**

---

## 5. try?

+ **예외 발생시 `nil 리턴`, 성공하는 경우 `옵셔널 값 반환`**

---

+ 출처
  + [[swift] 12. 예외 처리 (throws, throw, do - try - catch)](https://ios-development.tistory.com/16)
