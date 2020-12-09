---
title: Objective-C) atomic / nonatomic 속성
author: cotchan
date: 2020-12-09 15:23:21 +0800 
categories: [Objective-C, Objective-C_Basic] 
tags: [objective-c] 
---

아래 출처를 참고하여 작성한 글입니다. :)        
더 자세한 설명은 출처를 참고하시면 됩니다.    

## atomic

atomic으로 선언된 멤버의 경우` 다른 쓰레드에 의해 interrupt 되지 않고` 수행이 완료될 때까지 그 무결성을 유지합니다.    


+ atomic으로 설정된 프로퍼티의 getter/setter 메소드는 `lock`을 사용하여 멀티쓰레드에 대해 안전하게 처리됩니다.
+ 이러한 점 때문에 nonatomic 멤버에 비해 성능 저하가 발생합니다.

---


## nonatomic

nonatomic의 경우 데이터의 `무결성을 보장받지 않아도 되는 속성`을 의미합니다.

---

+ 출처	
	+ [[Objective-C] atomic / nonatomic 속성](https://dangercloz.tistory.com/84?category=671332)
