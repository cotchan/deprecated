---
title: Swift 기본 기능 코드
author: cotchan 
date: 2020-12-08 20:38:21 +0800 
categories: [Swift, Swift_Basic] 
tags: [swift] 
---

계속 추가 될 예정입니다. :)

## 배열 초기화 방법

```swift
private var numbers:[Bool] = [Bool].init(repeating: false, count: numberRange)
var used:[Bool] = [Bool](repeating: false, count: 10)
```


---


## Array 임의의 인덱스에 접근하는 방법

```swift
//number[mynumberIdx] 표현 방법
//swift에서 Array의 임의의 인덱스에 접근하는 방법

let myNumber:Character = number[number.index(number.startIndex, offsetBy: mynumberIdx)

answer[answer.index(answer.startIndex, offsetBy: idx)]
numbers[numbers.index(numbers.startIndex, offsetBy: targetNumber)
```


---


## for문 순회 방법

1. 숫자 갯수만큼 for문 순회
2. 컨테이너 내부 값으로 for문 순회

```swift
//숫자로 for문 순회
for number in 0 ..< number.count {
	//process
}

//컨테이너 내부 값으로 for문 순회
for num in answer {
	//abcde
}
```


---


## do-while 문

```swift
//case.1
repeat {
	answerNumber = numberGenerator.generate()
} while ValidationChecker.isValidateNumber(number: answerNumber) == false


//case.2
repeat {
  
	//userInput
	Logger.getUserNumber()
	let userNumber:String = UserInterface.getUserNumber()
	if ValidationChecker.isValidateNumber(number: userNumber) == false {
		continue
	}
    
	game.setCandidateNumber(targetNumber: userNumber)

	if game.isAnswer() == true {
		break
	}
	else {
		game.increaseWrongCount()
		Logger.getCurrentStateOfGame(game: game)
	}
            
} while game.isOpportunityLeft() == true
```


---


## String => Number 파싱 유효성 검사

```swift
static func isNumericType(param candidate:String) -> Bool {
        
	for value in candidate {
            
		guard value.isNumber else {
			return false
		}
	}
        
	return true
}
```
