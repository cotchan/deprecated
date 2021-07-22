---
title: TIL) Java enum value를 String으로 할 때 (enum vs String)
author: cotchan
date: 2021-07-22 17:06:00 +0800
categories: #
tags: [til]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. 문제

+ Java에서 `enum 타입의 value로 String을 선언했을 때` enum 타입이랑 String 타입이랑 생긴게 비슷해서 헷갈려서 정리해놓음

```java
public enum Level {

    GOLD("Gold"), SILVER("Silver");

    private String value;

    Level(String value) {
        this.value = value;
    }

    public String getValue() { return value; }
}
```

---

## 2. 문제 원인

+ **예를 들어 위 코드를 아래와 같이 바꾸면 `GOLD와 SILVER의 의미가 어떻게 되는지 헷갈림`**

```java
public enum Level {

    GOLD("GOLD"), SILVER("SILVER");

    private String value;

    Level(String value) {
        this.value = value;
    }

    public String getValue() { return value; }
}
```

---

## 3. 결론

+ 앞에 있는 enum 타입은 `instanceof ENUM`이다.
+ 뒤에 붙은 String 타입(value)은 `instanceof STRING`이다.
+ **아래와 같이 선언해도 `equals 결과는 false`다.** 그 이유는 둘의 데이터 타입이 다르기 때문 

```java
GOLD("GOLD"), SILVER("SILVER");

Level.GOLD.equals("GOLD");    //false
Level.SILVER.eqauls("SILVER");    //false
```

---

```java
public class Main {

    public enum Level {

        GOLD("Gold"), SILVER("Silver");

        private String value;

        Level(String value) {
            this.value = value;
        }

        public String getValue() { return value; }
    }


    public static void main(String[] args) throws Exception {

        System.out.println(Level.GOLD);  //GOLD
        System.out.println(Level.SILVER);    //SILVER
        System.out.println(Level.GOLD.getValue());   //Gold
        System.out.println(Level.SILVER.getValue()); //Silver

        if (Level.GOLD instanceof Level) {
            System.out.println("FSSLevel.GOLD instanceof FSSLevel");    //here
        }

        if (Level.GOLD.getValue() instanceof String) {
            System.out.println("FSSLevel.GOLD.getValue() instanceof String");   //here
        }

        if (Level.GOLD.equals("GOLD")) {
            System.out.println("FSSLevel.GOLD.equals(\"GOLD\") is true");
        } else {
            System.out.println("FSSLevel.GOLD.equals(\"GOLD\") is false");  //here
        }

        if (Level.GOLD.getValue().equals("Gold")) {
            System.out.println("FSSLevel.GOLD.getValue().equals(\"Gold\") is true");    //here
        } else {
            System.out.println("FSSLevel.GOLD.getValue().equals(\"Gold\") is true");
        }
    }
}
```

---

```
//System.out.println result

GOLD
SILVER
Gold
Silver
FSSLevel.GOLD instanceof FSSLevel
FSSLevel.GOLD.getValue() instanceof String
FSSLevel.GOLD.equals("GOLD") is false
FSSLevel.GOLD.getValue().equals("Gold") is true
```

---

