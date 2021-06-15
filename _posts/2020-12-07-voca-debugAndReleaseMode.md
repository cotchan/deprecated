---

title: Debug 모드, Release 모드의 차이점 

author: cotchan 

date: 2020-12-07 19:17:21 +0800 

categories: [ComputerScience, Voca]

tags: [voca]

---

+ **아래 출처의 내용들을 바탕으로 작성한 내용입니다.**

---

## Debug 모드

+ 프로그램을 배포하기 전 버그를 찾아 수정하기 위해 실행하는 모드
+ 실행파일에 디버깅 정보를 포함합니다.
  + 그러므로 실행 파일의 상태 정보를 확인할 수 있습니다.
  + 그러므로 언제든지 디버깅을 할 수 있습니다.
+ 디버깅 정보를 포함하기 때문에 속도가 릴리즈모드에 비해 느립니다.
+ 디버깅 정보를 포함하기 때문에 릴리즈모드보다 더 큰 메모리를 사용합니다.


---


## Release 모드

+ 프로그램을 배포하기 위해 최적화를 한 후 배포하기 위해 사용하는 모드입니다.
+ 디버깅 정보를 포함하지 않고, 코드를 최적화하여 실행파일의 크기를 최대한 줄입니다.
+ 속도가 디버그 모드에 비해 빠르고, 메모리 점유율이 낮습니다.
+ 초기화가 이루어지지 않습니다.


---


## 차이점

+ **가장 큰 차이점은 `실행파일(코드)에 디버깅 정보가 포함 되냐/안되냐의 차이`입니다. 따라서 소스의 크기가 다르고, 속도에서 차이가 생깁니다.**
+ **ASSERT문**
  + ASSERT문은 디버그 모드에서만 동작하고 릴리즈모드에서는 공백으로 대체됩니다. 
+ **Pointer**
  + 초기화되지 않은 포인터의 경우 `디버그모드에서는 `임의값 0xCD로 초기화를 수행합니다.`
  + 초기화되지 않은 포인터의 경우 `릴리즈모드에서는 `초기화를 수행하지 않습니다.`



---

+ 출처
  + [디버깅(Debug)모드와 릴리즈(Release)모드의 차이점](https://j2hworld.tistory.com/77)
  + [Debug와 Release모드의 차이](https://sossms.tistory.com/entry/Debug%EC%99%80-Release%EB%AA%A8%EB%93%9C%EC%9D%98-%EC%B0%A8%EC%9D%B4)
  + [[컴파일모드] 디버그모드, 릴리즈모드 정의 및 차이](https://m.blog.naver.com/PostView.nhn?blogId=helloworld8&logNo=220331020137&proxyReferer=https:%2F%2Fwww.google.com%2F)
  + [Debug와 release의 차이와 내용!!](https://chm706.tistory.com/entry/Debug%EC%99%80-release%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%99%80-%EB%82%B4%EC%9A%A9)
