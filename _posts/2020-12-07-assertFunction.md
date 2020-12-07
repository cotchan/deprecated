---

title: assert함수

author: cotchan 

date: 2020-12-07 19:48:21 +0800

categories: [ComputerScience, Voca]

tags: [voca]

---


아래 출처의 내용들을 바탕으로 작성한 내용입니다.


## assert 함수란

assert 매크로는 정해진 조건에 맞지 않을 때 프로그램을 중단합니다. 

+ assert에 지정한 조건식이 `거짓(false)`이면 프로그램을 중단합니다.
+ assert에 지정한 조건식이 참(true)이면 프로그램을 계속 실행합니다.


---

## assert 함수 사용 용도

+ assert 함수는 디버깅 모드에서 개발자가 오류가 생기면 치명적일 것이라는 곳에 심어놓는 에러 검출용 코드입니다.
  + assert 함수에 걸리면 오류 발생위치, 콜스택 등의 여러 정보를 알 수 있습니다.
+ assert 함수는 Debug 모드에서만 동작하며 Release 모드에서는 동작하지 않습니다.
+ assert 함수로 미리 `일어나서는 안 되는 상황을 정의`해놓으면 디버깅을 빠르게 할 수 있습니다.
  + 함수의 입력 값이 특정 조건을 만족하도록 보증하기
  + 함수의 반환 값이 특정 조건을 만족하도록 보증하기
  + 변수가 변하는 과정에서 특정 조건을 만족하도록 보증하기


---

+ 출처
  + [assert 사용하기](https://dojang.io/mod/page/view.php?id=764)
  + [가정 설정문(assert)](https://wikidocs.net/21050)
  + [[C언어/C++] assert 함수에 대해서 : 디버깅을위한, 더 안전한 코드를 위한 오류 검출 방법](https://blockdmask.tistory.com/286)


