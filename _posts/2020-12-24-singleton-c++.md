---
title: Singleton) C++에서 Thread-Safe한 Singleton 적용방법
author: cotchan
date: 2020-12-24 14:48:21 +/-0800
categories: [Design-Pattern, Singleton]
tags: [design-pattern]
---

계속 업데이트할 예정입니다.

---

## 1. Meyers Singleton

+ `C++ 11에서` Meyers Singleton의 장점은 `자동으로` `스레드 안전`하다는 것입니다.
+ **그 이유는 `블록 범위가 있는 정적 변수`는 정확히 한 번 생성되기 때문입니다.**
    + 자세한 내용은 아래 출처를 참고하시면 됩니다.
+ 이 특성을 따서 구현한 것이 Meyers Singleton 입니다.
   
```c++
//sample
class SingletonClass
{
private:
    SingletonClass();
    
public:
    static SingletonClass& instance() {
        static SingletonClass* instance = new SingletonClass();
        return *instance;
    }
    
    void func();    
};

//UseCase
#include "SingletonClass.hpp"

int main(void)
{
    SingletonClass::instance().func();
}
```

---

## 2. Static variables with block scope

```c++
#include <thread>

class MySingleton{
public:
  static MySingleton& getInstance(){
    static MySingleton instance;
    return instance;
  }
private:
  MySingleton();
  ~MySingleton();
  MySingleton(const MySingleton&)= delete;
  MySingleton& operator=(const MySingleton&)= delete;

};

MySingleton::MySingleton()= default;
MySingleton::~MySingleton()= default;

int main(){

  MySingleton::getInstance();

}
```
 
---

+ 출처
    + [Thread-Safe Initialization of Data
](https://www.modernescpp.com/index.php/thread-safe-initialization-of-data)
    + [Thread-Safe Initialization of a Singleton
](https://www.modernescpp.com/index.php/thread-safe-initialization-of-a-singleton)
