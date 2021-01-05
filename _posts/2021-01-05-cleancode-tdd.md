---
title: CleanCode) 단위 테스트
author: cotchan 
date: 2021-01-05 09:17:21 +0800
categories: [CleanCode, CleanCode_Remark] 
tags: [cleancode]
---

CleanCode 책을 바탕으로 정리한 내용입니다.            
개인 공부 목적으로 작성한 글이며 지속적으로 업데이트할 예정입니다.        

---

## 1. TDD 법칙 세 가지

1. **실패하는 단위 테스트를 작성할 때까지 실제 코드를 작성하지 않는다.**
2. **컴파일은 실패하지 않으면서 실행이 실패하는 정도로만 단위 테스트를 작성한다.** 
3. **현재 실패하는 테스트를 통과할 정도로만 실제 코드를 작성한다.**

---

## 2. 깨끗한 테스트 코드를 유지해야하는 이유

+ **`테스트 코드`는 실제 코드 못지 않게 중요합니다.**
    + 테스트 코드는 사고와 설계와 주의가 필요합니다.
    + 실제 코드 못지 않게 깨끗하게 짜야 합니다.

+ **테스트 코드는 `유연성`, `유지보수성`, `재사용성`을 제공합니다.**
    + 그 이유는 테스트 케이스가 있으면 변경이 쉬워지기 때문입니다.
    + 테스트 케이스가 없다면 `모든 변경이` `잠정적인 버그`입니다.
    + 아키텍쳐가 아무리 유연하더라도, 설계를 아무리 잘 나눴더라도, 테스트 케이스가 없으면 개발자는 변경을 주저합니다. 버그가 숨어들까 두렵기 때문입니다.
    + 따라서 테스트 코드가 지저분하면 코드를 변경하는 능력이 떨어지며 코드 구조를 개선하는 능력도 떨어집니다.

+ **그러므로 실제 코드를 점검하는 `자동화된 단위 테스트 슈트`는 설계와 아키텍쳐를 최대한 깨끗하게 보존하는 열쇠입니다.** 

---

## 3. 깨끗한 테스트 코드를 만들려면

+ **깨끗한 테스트 코드를 만들려면 `세 가지`가 필요합니다.**
    1. **`가독성`** 
    2. **`가독성`**
    3. **`가독성`**

+ **테스트 코드의 가독성을 높이려면 여느 코드와 마찬가지로 `명료성`, `단순성`, `풍부한 표현력`이 필요합니다.**
+ 테스트 코드는 최소의 표현으로 많은 것을 나타내야 합니다.

---

## 3-1. Sample Code

+ 각 테스트는 명확히 `세 부분`으로 나눠집니다.
    1. **첫 부분은 테스트 `자료를 만듭니다.`**
    2. **두 번째 부분은 테스트 `자료를 조작`합니다.**
    3. **세 번째 부분은 조작한 `결과가 올바른지 확인`합니다.**

+ 잡다하고 세세한 코드를 거의 다 없애고 테스트 코드는 본론에 돌입해 진짜 필요한 자료 유형과 함수만을 사용합니다.
+ 그러므로 코드를 읽는 사람은 코드가 수행하는 기능을 재빨리 이해할 수 있습니다.

```java
//리팩토링 코드 
//SerializedPageResponderTest.java
public void testGetPageHierarchyAsXml() throws Exception { 
	makePages("pageOne", "PageOne.ChildOne", "PageTwo");

	submitRequest("root", "type:pages");

	assertResponseIsXML();
	assertResponseContains(
		"<name>PageOne</name>", 
		"<name>PageTwo</name>", 
		"<name>ChildOne</name>"
	);
}

public void testSymbolicLinksAreNotInXmlPageHierarchy() throws Exception {
	WikiPage page = makePage("PageOne");
	makePages("PageOne.ChildOne", "PageTwo");

	addLinkTo(page, "PageTwo", "SymPage");

	submitRequest("root", "type:pages");

	assertResponseIsXML();
	assertResponseContains(
		"<name>PageOne</name>", 
		"<name>PageTwo</name>", 
		"<name>ChildOne</name>"
	);
	assertResponseDoesNotContains("SymPage");
}

public void testGetDataAsXml() throws Exception {
	makePageWithContent("TestPageOne", "test page");

	submitRequest("TestPageOne", "type:data");

	assertResponseIsXML();
	assertResponseContains("test page", "<Test");
}
```


---

+ 출처	
	+ 로버트 C. 마틴, 『Clean Code』, 인사이트(2013), p153-p169
