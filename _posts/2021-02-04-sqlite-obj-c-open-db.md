---
title: SQLITE) Objective-C에서 SQLite3 사용법2. (연결, Open)
author: cotchan
date: 2021-02-04 15:30:00 +0800
categories: [Sqlite, Sqlite_Obj-c]
tags: [sqlite]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. SQLite 연결 방법

---

## 1-1. libsqlite3 라이브러리 추가

+ Xcode 프로젝트에 libsqlite3 라이브러리를 추가해줍니다. 

---

## 1-2. <sqlite3.h> 헤더 추가

+ Objective-C에서 SQLite를 사용하려면, 아래와 같이 헤더 파일을 추가해줍니다.

```ruby
#import <sqlite3.h>
```

---

## 1-3. SQLite DB 생성 OR 열기

+ **SQLite 함수는 데이터베이스를 동적으로(소스코드 상으로)생성하는 것과 기존에 존재하는 데이터베이스를 여는 방법을 구분하지 않습니다.**
+ 따라서 `sqlite3_open` 함수에 데이터베이스 파일 경로를 지정했을 때 해다이 경로에 실제로 데이터베이스 파일이 있다면 그것을 열고, 존재하지 않으면 DB를 새로 생성한 다음 이를 엽니다.
+ `.sqlite` 파일

```ruby
# .sqlite filePath: 
# /Users/${USER_NAME}/Library/Developer/Xcode/DerivedData/${PROJECT_NAME}-bnkkfhpekokionecxsfblwmzdiii/Build/Products/Debug

NSString *databasePath = @"./test001.sqlite";
sqlite3 *database = nil;  //데이터베이스 연결정보
sqlite3_stmt *databaseStatement = nil;    //쿼리 구문 컴파일러
        
if (sqlite3_open([databasePath UTF8String], &database) == SQLITE_OK)
{
    NSLog(@"SUCCESS: sqlite3_open");
    sqlite3_close(database);
    database = nil;
}
else
{
    NSLog(@"FAILURE: sqlite3_open");
    sqlite3_close(database);
    database = nil;
}
```

---

+ 출처
  + [Objective-C에서 SQLite 사용 예](https://tapito.tistory.com/613)
  + [iOS에서 SQLite3 사용하는 방법](https://soooprmx.com/archives/4656)
