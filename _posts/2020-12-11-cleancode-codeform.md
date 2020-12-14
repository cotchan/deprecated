---
title: 코드 형식 맞추기
author: cotchan 
date: 2020-12-11 14:46:21 +0800
categories: [CleanCode, CleanCode_CodeForm] 
tags: [cleancode]
---

CleanCode 책을 바탕으로 정리한 내용입니다.            
개인 공부 목적으로 작성한 글이며 지속적으로 업데이트할 예정입니다.        

---

## INTRO

프로그래머라면 형식을 깔끔하게 맞춰 코드를 짜야합니다.    
코드 형식을 맞추기 위한 간단한 규칙을 정하고 그 규칙을 착실히 따라야 합니다.    
팀으로 일한다면 팀이 합의해 규칙을 정하고 모두가 그 규칙을 따라야 합니다.    
필요하다면 규칙을 자동으로 적용하는 도구를 활용하는 게 낫습니다.


---

## 형식을 맞추는 목적

**무엇보다 분명히 짚고 넘어가야할 한 가지는, `코드 형식`은 정말 중요합니다.**    
너무 중요해서 무시하기 어렵고, 너무나도 중요하므로 융통성 없이 맹목적으로 따르면 안됩니다.        

`돌아가는 코드`가 전문 개발자의 일차적인 의무라 여길지도 모르겠습니다.     
다만 오늘 구현한 기능이 다음 버전에서 바뀔 확률은 아주 높습니다.     
**그런데 오늘 구현한 `코드의 가독성`은 앞으로 바뀔 `코드 품질에 지대한 영향`을 미칩니다.**    
코드가 아무리 바뀌어도 맨 처음 잡아놓은 구현스타일과 가독성 수준은 유지보수 용이성과 확장성에 계속 영향을 미칩니다.    
원래 코드는 사라질지라도 개발자의 스타일과 규율은 사라지지 않습니다.    

---

## 좋은 코드 형식이란

---

## 1. 적절한 행 길이를 유지하라

JUnit, FitNesse, Time and Money는 500줄을 넘어가는 파일이 없으며 대다수가 200줄 미만입니다.    
이게 의미하는 것은 500줄을 넘지 않고 대부분 200줄 정도인 파일로도 커다란 시스템을 구축할 수 있다는 사실입니다.    
반드시 지킬 엄격한 규칙은 아니지만 바람직한 규칙으로 삼으면 좋겠습니다.    
일반적으로 큰 파일보다 작은 파일이 이해하기 쉽습니다. 


---

## 2. 신문 기사처럼 작성하라

좋은 신문 기사를 떠올려봅니다. 독자는 위에서 아래로 기사를 읽습니다.    
최상단에 기사를 몇 마디로 요약하는 표제가 나옵니다. 독자는 표제를 보고서 기사를 읽을지 말지 결정합니다.    
첫 문단은 전체 기사 내용을 요약합니다. 세세한 사실은 숨기고 커다란 그림을 보여줍니다.    
쭉 읽으며 내려가면 세세한 사실이 조금씩 드러납니다. 날짜, 이름, 발언, 주장, 기타 세부사항이 나옵니다.    

+ **소스 파일도 신문 기사와 비슷하게 작성해야 합니다.**    

1. 이름은 간단하면서도 설명이 가능하게 짓습니다.
2. **`이름만 보고도` 올바른 모듈을 살펴보고 있는지 아닌지를 판단할 정도로 신경써서 짓습니다.**
3. 소스 파일 첫 부분은 `고차원 개념`과 `알고리즘`을 설명합니다.
4. 아래로 내려갈수록 의도를 세세하게 묘사합니다.
5. 마지막에는 가장 저차원 함수와 세부 내역이 나옵니다.


---

## 3. 개념은 빈 행으로 분리하라

거의 모든 코드는 왼쪽에서 오른쪽으로 그리고 위에서 아래로 읽힙니다. 각 행은 수식이나 절을 나타내고, 일련의 행 묶음은 완결된 생각 하나를 표현합니다. `생각 사이에는 빈 행을 넣어 분리`해야 마땅합니다.    

그러므로 패키지 선언부, import 문, 각 함수 사이에는 빈행을 넣어 분리해야 합니다. 너무도 간단한 규칙이지만 코드의 세로 레이아웃에 큰 영향을 미칩니다.


---

## 4. 세로 밀집도

줄바꿈이 개념을 분리한다면 `세로 밀집도`는 `연관성을 의미`합니다. 즉, 서로 밀접한 코드 행은 세로로 가까이 놓아야 한다는 뜻입니다.


+ 아래 코드의 경우 `의미없는 주석`으로 두 인스턴스 변수를 떨어뜨려 놓았습니다.    

```java
public class ReporterConfig {
	/**
	* 리포터 리스너의 클래스 이름
	*/
	private String m_className;

	/**
	* 리포터 리스너의 속성
	*/
	private List<Property> m_properties = new ArrayList<Property>();
	
	public void addProperty(Property property) {
		m_properties.add(property);
	}
}
```

---

+ 아래 코드의 경우 위의 경우보다 `코드가 한 눈에` 들어옵니다.

```java
public class ReporterConfig {
        
	private String m_className;
        private List<Property> m_properties = new ArrayList<Property>();
        
	public void addProperty(Property property) {
                m_properties.add(property);
        }
}
```

---

## 5. 변수 간의 수직거리

## 5-1. 변수는 사용하는 위치에 최대한 가까이 선언하기

---

## 5-2. 인스턴스 변수는 클래스 맨 처음에 선언

+ 클래스 마지막이든, 클래스 최상단이든 `잘 알려진 위치에 인스턴스 변수를 모은다`는 사실이 중요합니다. 변수 선언을 어디서 찾을지 모두가 알고 있어야 합니다. 
+ **변수 간에 세로로 거리를 두지 않습니다.**


---

## 6. caller를 callee보다 상단에 선언하기 (종속함수 관계)

+ 한 함수가 다른 함수를 호출한다면 두 함수는 세로로 가까이 배치해야 합니다.
+ **또한 가능하다면 `caller`(호출하는 함수)`를` `callee`(호출되는 함수)`보다` `먼저 배치합니다.`**
	+ 위와 같이 하면 프로그램이 자연스럽게 읽힙니다. 규칙을 일관적으로 적용한다면 독자는 방금 호출한 함수가 잠시 후에 정의되리라는 사실을 예측할 수 있습니다.

```java
public class WikiPageResponder implements SecureResponder {
	public Response makeResponse(FitNesseContext context, Request request) 
	{
		//do something
		String pageName = getPageNameOrDefault(request, "FrontPage");
		loadPage(pageName, context);
		//do something
	}	
	private String getPageNameOrDefault(Request request, String defaultPageName)
	{
		//do something	
	}
	protected void loadPage(String resource, FitNesseContext context)
	{
		//do something
	}
	//...
}
```

---

## 7. 개념적 유사성이 비슷한 코드는 가까이 둔다.

친화도가 높을수록 코드를 가까이에 배치합니다.





---

+ 출처	
	+ 로버트 C. 마틴, 『Clean Code』, 인사이트(2013), p95-p116
