---
title: Java) 싱글턴 생성 방법
author: cotchan
date: 2021-04-05 00:00:00 +0800
categories: [Java]
tags: [java]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 생성 방법 1(게으른 초기화O)

+ **`중첩된 클래스`는 `getInstance()가 호출되기 전까지는 참조되지 않습니다.`** 
+ 따라서 이 방법은 특별한 언어의 구조가 필요없는(예를들면 `volatile`이나 `synchronized`) 스레드 세이프(thread-safe)한 방법입니다.

```java
public class Singleton {

    // Private constructor prevents instantiation from other classes
    private Singleton() { }
  
    /**
    * SingletonHolder 클래스는 Singleton.getInstance()가 호출되는 시점에 메모리에 적재된다. (게으른 초기화 가능)
    * 또는 SingletonHolder.INSTANCE에 처음 접근할 때 메모리에 적재된다. (그 전까지는 X)
    */
    private static class SingletonHolder {
        public static final Singleton instance = new Singleton();
    }
  
    public static Singleton getInstance() {
        return SingletonHolder.instance;
    }
}
```

---

## 2. 생성 방법 2(게으른 초기화X)

+ **언어 상의 특별한 구조가 필요없이 스레드 세이프한 방법이지만 `게으른 초기화를 하지 않는 방법`입니다.** 
+ 싱글톤 클래스가 초기화될 때 그 객체(instance) 또한 생성됩니다.

```java
public class Singleton {
        
    private static final Singleton instance = new Singleton();
  
    // Private constructor prevents instantiation from other classes
    private Singleton() { }
  
    public static Singleton getInstance() {
        return instance;
    }
}
```

---

+ 출처
  + [Singleton pattern(싱글톤) vs. static](https://m.blog.naver.com/PostView.nhn?blogId=satang50&logNo=195684802&proxyReferer=https:%2F%2Fwww.google.com%2F)
