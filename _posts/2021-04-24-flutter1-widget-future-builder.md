---
title: Flutter) [위젯] FutureBuilder
author: cotchan
date: 2021-04-24 15:04:00 +0800
categories: [Flutter, Flutter_widget]
tags: [flutter1]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. FutureBuilder

- **FutureBuilder는 Future와 상호작용한 마지막 스냅샷으로 자신을 빌드하는 위젯입니다.**
  + **즉, Flutter에서 비동기 처리를 할 때 사용하는 Builder 입니다.** 
- FutureBuilder를 사용하지 않고 서버로부터 받아온 데이터를 표시하고자 한다면
  - 데이터가 모두 도착하기를 기다린 다음에 화면을 그리거나, setState() 메소드를 호출하여 화면을 새로고침니다.
- **따라서 FutureBuilder를 사용하면**
  - 예시 1
    - 서버로부터 받아온 데이터를 사용하는 부분만 → 데이터가 도착한 이후에 표시
    - 그렇지 않은 부분 → 화면에 표시
  - 예시 2
    - API콜을 해서 응답을 받기 전까지는 로딩 위젯를 보여주고
    - 성공 응답을 받으면 데이터를 보여주는 위젯을 보여주고
    - 실패 응답을 받으면 에러메세지를 보여주는 위젯을 보여주고 싶을때 쓰는 위젯입니다.


---

+ 출처
  - [https://eunjin3786.tistory.com/272](https://eunjin3786.tistory.com/272)
  - [https://velog.io/@humblechoi/Flutter-FutureBuilder를-사용한-비동기-통신](https://velog.io/@humblechoi/Flutter-FutureBuilder%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%9C-%EB%B9%84%EB%8F%99%EA%B8%B0-%ED%86%B5%EC%8B%A0)
