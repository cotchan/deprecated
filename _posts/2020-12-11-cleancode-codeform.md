---
title: CleanCode) 코드 형식 맞추기
author: cotchan 
date: 2020-12-11 14:46:21 +0800
categories: [CleanCode] 
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

일반적으로 함수 호출 종속성은 아래 방향으로 유지합니다. **다시 말해 `callee` 함수를 caller보다 `나중에 배치`합니다.** 그러면 소스 코드 모듈이 `고차원에서` `저차원으로` 자연스럽게 내려갑니다.     


---

## 7. 개념적 유사성이 비슷한 코드는 가까이 둔다.

친화도가 높을수록 코드를 가까이에 배치합니다.    
친화도가 높은 요인은 여러 가지입니다. 앞서 언급했던 `caller-callee 관계`가 한 예입니다. 또한 비슷한 동작을 수행하는 일군의 함수가 좋은 예시입니다.    

```java
public class Assert {
	
    static public void assertTrue(String message, boolean condition) {
        if (!condition)
	    fail(message);
    }

    static public void assertTrue(boolean condition) { 
	assertTrue(null, condition);
    }

    static public void assertFalse(String message, boolean condition) {
        assertTrue(message, !condition);
    }
			
    static public void assertFalse(boolean condition) {
        assertFalse(null, condition);
    }

    //...
}
```

위 함수들은 개념적인 친화도가 매우 높습니다.    
+ 명명법이 똑같고
+ 기본 기능이 유사하고 간단합니다.

서로가 서로를 호출하는 관계는 부차적인 요인입니다. 종속적인 관계가 없더라도 가까이 배치할 함수들입니다.


---

## 8. 코드의 세로 배치 순서

+ 신문 기사와 마찬가지로 `가장 중요한 개념을` `가장 먼저` 표현합니다.    
+ 가장 중요한 개념을 표현할 때는 세세한 사항을 최대한 배제합니다. 
+ `세세한 사항은` `가장 마지막에` 표현합니다.
+ **그러면 독자가 소스 파일에서 첫 함수 몇 개만 읽어도 개념을 파악하기 쉬워집니다.** 

---

## 9. 들여쓰기

범위(Scope)로 이뤄진 계층을 표현하기 위해 우리는 코드를 들여씁니다.    
들여쓰는 정도는 계층에서 코드가 자리잡은 수준에 비례합니다.    

+ 클래스 정의처럼 파일 수준인 문장은 들여쓰지 않습니다.
+ 클래스 내 메서드는 클래스보다 한 수준 들여씁니다.
+ 메서드 코드는 메서드 선언보다 한 수준 들여씁니다.
+ 블록 코드는 블록을 포함하는 코드보다 한 수준 들여씁니다.

---

## 9-1. 들여쓰기 무시하지 않기

때로는 간단한 if문, 짧은 while문, 짧은 함수의 경우 들여쓰기 규칙을 무시하려는 경향이 있습니다. 그러나 이러한 경우에도 들여쓰기를 하는 것이 코드 가독성을 높일 수 있습니다.

```java
//들여쓰기 사용 X
public class CommentWidget extends TextWidget
{
    public static final String REGEXP = "^#[^\r\n]*(?:(?:\r\n)|\n|\r)?";
    
    public CommentWidget(ParentWidget parent, String text) { super(parent,text); }
    public String render() throws Exception { return ""; }
}
```

```java
//들여쓰기 사용 O
public class CommentWidget extends TextWidget
{
    public static final String REGEXP = "^#[^\r\n]*(?:(?:\r\n)|\n|\r)?";
    
    public CommentWidget(ParentWidget parent, String text) { 
        super(parent,text); 
    }

    public String render() throws Exception { 
        return ""; 
    }
}
```

---

## 9-2. 빈 while문이나 for문 처리하는 방법

이런 구조는 가능한 피하는 것이 좋지만, 피하지 못할 때는 빈 블록을 올바로 들여쓰고 괄호로 감쌉니다. 세미콜론(`;`)은 새 행에다 제대로 들여써서 넣어줍니다. 그렇지 않으면 눈에 띄지 않습니다.

```java
while (dis.read(buf, 0, readBufferSize) != -1)
;
```

---

## 10. 팀 규칙을 따르기

프로그래머라면 각자 선호하는 규칙이 있습니다.    
**하지만 팀에 속한다면 자신이 선호해야 할 규칙은 바로 `팀 규칙`입니다.**        
팀은 한 가지 규칙에 합의해야 합니다. 그리고 모든 팀원은 그 규칙을 따라야 합니다. 그래야 소프트웨어가 일관적인 스타일을 보입니다.    

+ 팀 규칙 예시 
    + 어디에 괄호를 넣을지
    + 들여쓰기는 몇 자로 할지
    + 클래스와 변수와 메서드 일음은 어떻게 지을지

+ 좋은 소프트웨어 시스템은 읽기 쉬운 문서로 이뤄집니다.
    + 스타일은 일관적이고 매끄러워야 합니다. 
    + **한 소스 파일에서 봤던 형식이 다른 소스 파일에도 쓰이리라는 신뢰감을 독자에게 줘야 합니다.**

---

## 11. 밥 아저씨(저자)의 형식 규칙 샘플 코드

```java
public class CodeAnalyzer implements JavaFileAnalysis {
	private int lineCount;
	private int maxLineWidth;
	private int widestLineNumber;
	private LineWidthHistogram lineWidthHistogram;
	private int totalChars;

	public CodeAnalyzer() {
		lineWidthHistogram = new LineWidthHistogram();
	}

	public static List<File> findJavaFiles(File parentDirectory) {
		List<File> files = new ArrayList<File>();
		findJavaFiles(parentDirectory, files);
		return files;
	}

	private static void findJavaFiles(File parentDirectory, List<File> files) {
		for (File file : parentDirectory.listFiles()) {
			if (file.getName().endsWith(".java"))
				files.add(file);
			else if (file.isDirectory())
				findJavaFiles(file, files);
		}
	}

	public void analyzeFile(File javaFile) throws Exception { 
		BufferedReader br = new BufferedReader(new FileReader(javaFile));
		String line;
		while ((line = br.readLine()) != null)
			measureLine(line);
	}

	private void measureLine(String line) {
		lineCount++;
		int lineSize = line.length();
		totalChars += lineSize;
		lineWidthHistogram.addLine(lineSize, lineCount);
		recordWidestLine(lineSize);
	}
	
	private void recordWidestLine(int lineSize) {
		if (lineSize > maxLineWidth) {
			maxLineWidth = lineSize;
			widestLineNumber = lineCount;
		}
	}

	public int getLineCount() {
		return lineCount;
	}

	public int getMaxLineWidth() {
		return maxLineWidth;
	}

	public int getWidestLineNumber() {
		return widestLineNumber;
	}

	public LineWidthHistogram getLineWidthHistogram() {
		return lineWidthHistogram;
	}

	public double getMeanLineWidth() {
		return (double)totalChars/lineCount;
	}

	public int getMedianLineWidth() {
		Integer[] sortedWidths = getSortedWidths();
		int cumulativeLineCount = 0;
		for (int width : sortedWidths) {
			cumulativeLineCount += lineCountForWidth(width);
			if (cumulativeLineCount > lineCount/2)
				return width;
		}
		throw new Error("Cannot get here");
	}

	private int lineCountForWidth(int width) {
		return lineWidthHistogram.getLinesforWidth(width).size();
	}

	private Integer[] getSortedWidths() {
		Set<Integer> widths = lineWidthHistogram.getWidths();
		Integer[] sortedWidths = (widths.toArray(new Integer[0]));
		Arrays.sort(sortedWidths);
		return sortedWidths;
	}
}
```



---
+ 출처	
	+ 로버트 C. 마틴, 『Clean Code』, 인사이트(2013), p95-p116
