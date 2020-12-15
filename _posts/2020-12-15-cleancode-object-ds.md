---
title: CleanCode) 객체와 자료구조(객체 vs 자료구조 중 무엇을 선택해야 하나?)
author: cotchan 
date: 2020-12-15 09:17:21 +0800
categories: [CleanCode] 
tags: [cleancode]
---

CleanCode 책을 바탕으로 정리한 내용입니다.            
개인 공부 목적으로 작성한 글이며 지속적으로 업데이트할 예정입니다.        

---

## 자료구조와 객체의 차이

```java
//자료구조: 구체적인 클래스
//구현을 외부로 노출합니다. 
public class Point { 
    public double x;
    public double y;	
}

public interface Vehicle { 
    double getFuelTankCapacityInGallons();
    double getGallonsOfGasoline();
}
```


+ 변수 사이에 함수라는 계층을 넣는다고 해서 구현이 저절로 감춰지지는 않습니다.
+ **구현을 감추려면 `추상화`가 필요합니다.** 

```java
//객체: 추상적인 클래스
//구현을 완전히 숨깁니다.
public interface Point { 
    double getX();
    double getY();
    void setCartesian(double x, double y);
    double getR();
    double getTheta();
    void setPolar(double r, double theta);
}

public interface Vehicle { 
    double getPercentFuelRemaining();
}
```

객체와 자료구조의 개념은 정반대입니다.    
+ **객체는 추상화 뒤로 `자료를 숨긴 채` `자료를 다루는 함수만 공개`합니다.**
+ **자료 구조는 `자료를 그대로 공개`하며 별다른 `함수는 제공하지 않습니다.`**


---

## 자료구조와 객체의 차이 Summary 

+ 자료구조를 사용하는 절차적인 코드는   
        + 기존 자료 구조를 변경하지 않으면서 `새 함수를 추가하기가 쉽습니다.`
        + 새로운 자료 구조를 추가하기가 어렵습니다. 그러려면 모든 함수를 고쳐야 합니다.

+ 객체를 사용하는 객체 지향 코드는 
        + 기존 함수를 변경하지 않으면서 `새 클래스를 추가하기가 쉽습니다.`
        + 새로운 함수를 추가하기가 어렵습니다. 그러려면 모든 클래스를 고쳐야 합니다.


**다시 말하면 객체 지향 코드에서 어려운 변경은 절차적인 코드에서 쉬우며, 절차적인 코드에서 어려운 변경은 객체 지향 코드에서 쉽습니다.**

+ 복잡한 시스템을 짜다보면	
	1. 새로운 자료 타입이 필요한 경우에는 클래스와 객체 지향 기법이 가장 적합합니다.
	2. 새로운 함수가 필요한 경우에는 절차적인 코드와 자료구조가 좀 더 적합합니다.

---

## 자료구조와 객체의 차이: 1-1. 절차적인 Class

+ Geometry 클래스는 세 가지 도형 클래스를 다룹니다.
+ `각 도형 클래스는 간단한 자료구조`입니다. 즉, 아무 메서드도 제공하지 않습니다.
+ 도형이 동작하는 방식은 Geometry 클래스에서 구현합니다.

```java
//절차적인 도형 Class
public class Square {
	public Point topLeft;
	public double side;
}

public class Rectangle {
	public Point toLeft;
	public double height;
	public double width;
}

public class Circle {
	public Point center;
	public double radius;
}

public class Geometry {
	public final double PI = 3.14;
		
	public double area(Object shape) throws NoSuchShapeException {
			
		if (shape instanceof Square) 
		{
			Square s = (Square)shape;
			return s.side * s.side;
		}
		else if (shape instanceof Rectangle) 
		{
			Rectangle r = (Rectangle)shape;
			return r.height * r.width;
		}
 		else if (shape instanceof Circle) 
		{
			Circle c = (Circle)shape;
			return PI * c.radius * c.radius;
		}
		
		throws new NoSuchShapeException();
	} 
}
```

---

## 자료구조와 객체의 차이: 1-2. 절차적인 Class의 장점/단점

+ **`새 함수를 추가`하기가 `쉽습니다.`**
	+ 만약 Geometry 클래스에 둘레 길이를 구하는 perimeter() 함수를 추가하고 싶다면 도형 클래스는 아무런 영향을 받지 않습니다.

+ **새 클래스(자료형)을 추가하기는 어렵습니다.**
	+ 만약 새 도형을 추가하고 싶다면? Geometry 클래스에 속한 함수를 모두 고쳐야 합니다. (새로운 자료형에 대한 분기를 모두 추가해야 함) 


---

## 자료구조와 객체의 차이: 2-1. 다형적인 Class

```java
//다형적인 도형 Class
public class Square implements Shape { 
	private Point toLeft;
	private double side;
	public double area() { 
		return side * side;
	}
}

public class Rectangle implements Shape { 
	private Point toLeft;
	private double height;
	private double width;
	public double area() {
		return height * width;
	}
}

public class Circle implements Shape { 
	private Point center;
	private double radius;
	public final double PI = 3.14;
	public double area() {
		return PI * radius * radius; 
	}
}
```


---

## 자료구조와 객체의 차이: 2-2. 다형적인 Class의 장점/단점

+ **`새 클래스(자료형)를 추가하기가 쉽습니다.`**
	+ 만약 새 클래스를 추가하고 싶다면 다른 도형 클래스는 아무런 영향을 받지 않습니다. 

+ **새 함수를 추가하기가 어렵습니다.**
	+ 만약 새 함수를 추가하고 싶다면 Shape를 상속받는 모든 클래스의 함수를 고쳐야 합니다.

---

+ 출처	
	+ 로버트 C. 마틴, 『Clean Code』, 인사이트(2013), p68-p94
