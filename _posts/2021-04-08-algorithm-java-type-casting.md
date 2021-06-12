---
title: Java) 자료형 변환(형변환) 모음
author: cotchan
date: 2021-04-08 08:20:21 +0800
categories: [Algorithm, Algorithm_java]
tags: [jagorithm]     
---

+ 아래 출처를 바탕으로 본인의 공부 목적으로 작성한 글입니다.    
+ **계속 업데이트할 예정입니다.**

---

## 1. String

+ **임의의 다른 자료형을 String으로 만들 때는 `String.valueOf()` 연산을 적용합니다.**

---

## 1-1. char[] to String

+ **`String.valueOf()`**
+ String.valueOf() 메소드에 매개변수로 `char[]`를 넣으면, `String`으로 바로 변환해줍니다.

```java
char[] ary = {'a','b','c','d','e'};
String arrayString = String.valueOf(ary);
System.out.println(arrayString);
```

---

+ **`String 생성자`**
+ char 배열을 `String 생성자`의 인자로 넣고 String을 생성하면 됩니다.

```java
public void charArrayToString1() {
    char[] charArray = { 'H', 'e', 'l', 'l', 'o', 'W', 'o', 'r', 'l', 'd' };
    String str = new String(charArray);
    System.out.println(str);
}
```

---

## 1-2. char to String

+ **`한 글자 char`에 대해서 `String 연산을 적용`해야 할 때**
+ **`String s = char c + "";`**

```java
//example1
char c = 'a';
String oneLetterString = c + "";
```

```java
//example2
for (int idx = 0; idx < source.length(); ++idx)
{
    String target = source.charAt(idx) + "";
    
    if (target.compareTo("-") == 0 || target.compareTo("_") == 0 || target.compareTo(".") == 0)
    {
        //...
    }
}
```

---

## 2. Long & Integer

---

## 2-1. Long to int

+ **`int Long.valueOf(${long value}).intValue();`연산을 통해서 형변환이 가능합니다.**

```java
Long oper = Long.parseLong(oper[2]);

int intValue = Long.valueOf(oper).intValue();
```

---

## 3. int to char

---

## 3-1. 아스키 오프셋 활용 

+ **아스키 오프셋 `65`**
+ 타입 캐스팅을 사용해서 **`char result` = `(char)(intValue + 65)`** 를 만드는 방식입니다.

```java
public class SimpleTesting {
    static final int ASCII_OFFSET = 65;
    public static void main(String[] args) {
        int node = 0;
        int alphaValue = node + ASCII_OFFSET;
        char alpha = (char)alphaValue;
        System.out.println(alpha);
        //'A'
    }
}
```

---

## 3-2. Integer.toString 사용

1. **`int` → `string`**
2. **`string.charAt(0)` → `char`**

```java
public class SimpleTesting {
    public static void main(String[] args) {
    int value_int = 1;
    char value_char = Integer.toString(value_int).charAt(0);
    System.out.println(value_char);
    //1
    }
}
```

---

## 3-3. Character.forDigit 

+ **10진수, 16진수 표현과 관련된 메서드입니다.**
  + `특정 진법`에서 사용하는 `숫자에 대한 문자 표현`이 필요할 때 사용합니다.
  + 기수 또는 숫자값이 유효하지 않으면 `null`을 반환합니다.
  + 아래와 같이 사용하면 10진법에서 여섯번째 숫자를 문자로 리턴합니다.

```java
public class SimpleTesting {
    public static void main(String[] args) {
        //radix 10 is for decimal number, for hexa use radix 16 
        int radix = 10; 
        int value_int = 6;
        char value_char = Character.forDigit(value_int , radix);
        System.out.println(value_char);
				//6
    }
}
```

- 16진수 값을 얻으려면 두 번째 인수값으로 16을 주면 됩니다.

```java
public class SimpleTesting {
    public static void main(String[] args) {
        //radix 16 is for Hexa number
        int radix = 16; 
        int value_int = 12;
        char value_char = Character.forDigit(value_int , radix);
        System.out.println(value_char);
        //c
    }
}
```


---

+ 출처
  + [자바 String을 Char로, Char를 String으로 변환하기](https://kutar37.tistory.com/entry/%EC%9E%90%EB%B0%94-String%EC%9D%84-Char%EB%A1%9C-Char%EB%A5%BC-String%EC%9C%BC%EB%A1%9C-%EB%B3%80%ED%99%98%ED%95%98%EA%B8%B0) 
  + [Java - Char 배열을 String으로 변환하는 방법](https://codechacha.com/ko/java-convert-chararray-to-string/)
  + [Java에서 int를 char로 변환하는 방법](https://www.delftstack.com/ko/howto/java/how-to-convert-int-to-char-in-java/)
