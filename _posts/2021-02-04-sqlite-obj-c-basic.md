---
title: SQLITE) Objective-C에서 SQLite3 사용법1. (핵심 객체, 함수)
author: cotchan
date: 2021-02-04 15:00:00 +0800
categories: [Sqlite, Sqlite_Obj-c]
tags: [sqlite]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**
+ **직접 사용하는 방법은 2편부터 나와있습니다.**

---

## 1. SQLite 핵심 객체(2개)

```c++
sqlite3 *datebase;    // 데이터베이스 연결정보
sqlite3_stmt *databaseStatement;    // 쿼리구문 컴파일러
```
---

## 1-1. sqlite3

+ **데이터베이스 커넥션 정보를 갖고 있는 객체입니다.**
+ **`sqlite3_open()` 함수를 호출**하여 생성합니다.
+ **`sqlite3_close()` 함수를 통해** 연결을 닫으면서 해제됩니다.

---

## 1-2. sqlite3_stmt

+ **쿼리를 데이터베이스로 전송하기 전 컴파일을 하는 객체입니다.**
+ 데이터베이스에 보낼 질의를 컴파일한 객체입니다.
+ **`sqlite3_prepare()` 함수를 통해** 질의를 컴파일하면서 생성됩니다.
+ **`sqlite3_finalize()` 함수를 통해** 해제됩니다.

---

## 2. SQLite 핵심 함수(6개)

+ 실제로 sqlite3을 사용할 때는 위의 함수들을 순서대로 호출해서 사이클을 끝내면 됩니다.
+ 또한 사용하려는 함수에서는 V2, V16 이런 접미사가 붙는 함수가 종종 있습니다.
  + `V16`은 `UTF-16` 인코딩을 사용한다는 의미입니다.
  + `V2`는 주로 sqlite3_prepare_v2()로 많이 쓰는데, 이 버전으로 생성한 stmt 객체에는 추후 `쿼리에 바인딩`을 붙일 수 있습니다.
    + 바인딩시 인덱스는 `1부터` 시작합니다.

---

## 2-1. sqlite3_open

+ DB와 연결을 생성합니다.

---

## 2-2. sqlite3_prepare

+ **`쿼리를 컴파일`합니다.**

---

## 2-3. sqlite3_step

+ **`쿼리를 실행`합니다. 각 row를 fetch 합니다.**

---

## 2-4. sqlite3_column

+ fetch한 row에 대해 각 컬럼의 데이터를 리턴합니다.
+ 특정 순번의 컬럼을 가져오기 위해서는 해당하는 데이터타입으로 받아와야합니다. 

```c++
//example
field_index = (int) sqlite3_column_int(databaseStatement, 0);
field_value = (const char*) sqlite3_column_text(databaseStatement, 1);
```

---

## 2-5. sqlite3_finalize

+ 질의를 완료합니다.
+ **`데이터베이스에 커밋`되고 prepared_statement(sqlite3_stmt) `객체가 해제`됩니다.**

---

## 2-6. sqlite3_close

+ 연결을 종료합니다. 데이터베이스의 lock이 해제되고, 연결 객체 또한 파괴됩니다. 

---

+ 출처
  + [Objective-C에서 SQLite 사용 예](https://tapito.tistory.com/613)
  + [iOS에서 SQLite3 사용하는 방법](https://soooprmx.com/archives/4656)
