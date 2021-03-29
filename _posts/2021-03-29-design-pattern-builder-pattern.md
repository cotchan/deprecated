---
title: Design-pattern) Builder Pattern
author: cotchan
date: 2021-03-29 16:01:21 +0800
categories: [Design-Pattern]
tags: [design-pattern]
---

+ **아래 출처글을 참고하여 작성한 글입니다.**
+ 개인 공부 목적으로 작성하였습니다.
+ 계속 업데이트할 예정입니다.

---

## 1. 빌더 패턴이란

+ **빌더 패턴이란 `점층적 생성자 패턴`과 `자바 빈 패턴`의 장점을 결합한 패턴입니다.**

+ 클라이언트 코드에서 필요한 객체를 직접 생성하는 대신, 
  **1. 필수 인자들을 전달하여 `빌더 객체를 만든 뒤,`**
  **2. 빌더 객체에 정의된 `설정 메서드들을 호출하여 인스턴스를 생성`합니다.**

+ **빌더 패턴의 의도는 생성과정과 표현방법의 분리입니다.** 
  + 즉 기본 뼈대를 이루는 생성방식의 변경없이 다양한 것들을 표현 하기 위함입니다

---

## 2. 빌더 패턴 사용 시 이점
 
+ 빌더 패턴을 사용해서 얻을 수 있는 이점은 다음과 같습니다.

1. **불필요한 생성자를 만들지 않고 객체를 만든다.**
2. **데이터의 순서에 상관 없이 객체를 만든다.**
3. **사용자가 봤을 때 명시적이고 이해할 수 있다.**
4. **객체 생성 시 일관성을 깨지 않고 `Immutable`한 객체를 만들 수 있습니다.**

---

## 3. 예시 코드

```java
public interface Buildable<T> {
	T build();
}
```

---

```java
public class Student {
	private long id;
	private String name;
	private String major;
	private int age;
	private String address;

	public String() {}

	public static class Builder implements Buildable {
		//Essential param
		private final long id;
		private final String name;
		//Selective param
		private String major = "";
		private int age = 0;
		private String address = "";

		//Constructor taking required arguments as parameters
		public Builder(long id, String name) {
			this.id = id;
			this.name = name;
		}

		public Builder major(String major) {
			this.major = major;
			return this;
		}

		public Builder age(int age) {
			this.age = age;
			return this;
		}

		public Builder address(String address) {
			this.address = address;
			return this;
		}

		public Student build() {
			return new Student(this);
		}
	}

	public Student(Builder builder) {
		this.id = builder.id;
		this.name = builder.name;
		this.major = builder.major;
		this.age = builder.age;
		this.address = builder.address;
	}
}
```

---

## 4. 예시 코드 해석

+ Student 클래스안에 `Builder`라는 정적 멤버 클래스를 만듭니다.
+ 최종적으로 `build()` 라는 메서드를 통해 인스턴스를 생성할 것입니다.
+ `Student` 클래스의 필드 중 `id` 와 `name` 을 필수 인자로 선택하여 `Builder`를 생성할 때 인수로 넘겨 받게 하였습니다. 그리고 나머지 선택적 인수들은 `Builder`를 return하여 Chaining을 통해 인수를 넘겨받을 수 있도록 했습니다.

---

## 4-1. 클라이언트 코드 예시


```java
//클라이언트 코드 예시
Student student = new Student.Builder(141096, "jbee")
                             .major("Web_Server_programming")
			     .age(25)
			     .address("Jeong-Ja")
			     .build();
```

---

## 4-2. @Builder 사용 클라이언트 코드 예시

```java
//@Builder 사용 클라이언트 코드 예시
//builder()로 시작해서, build() 메서드를 통해 인스턴스 생성을 마칩니다.

final CompletableFuture<Boolean> storageResultFuture = urlStorageService.saveUrl(
                Url.builder()
                .uuid(uuid)
                .shortUrl(shortUrl)
                .originalUrl(requestedUrl)
                .build());
```

---

## 5. 빌더 패턴의 중 한 특징

+ **빌더 패턴의 `가장 중요한 특징은 두 가지` 입니다.**

1. 빌더 클래스는 **생성할 객체의 필수 프로퍼티를 매개변수로 갖는 생성자** 호출 ⇒ 빌더 객체를 얻습니다.
2. 그 다음에 **빌더 객체의** `**setter 메서드`를 호출하여 필요한 선택 매개 변수들의 값을 설정**합니다.

---

+ 출처
  - 점층적 생성자 패턴
    - [https://ifcontinue.tistory.com/6](https://ifcontinue.tistory.com/6)
  - 빌더 패턴
    - [https://asfirstalways.tistory.com/350](https://asfirstalways.tistory.com/350)
    - [https://jdm.kr/blog/217](https://jdm.kr/blog/217)
    - [https://okky.kr/article/396206](https://okky.kr/article/396206)

