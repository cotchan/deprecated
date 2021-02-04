---
title: SQLITE) Objective-C에서 SQLite3 사용법3. (쿼리 샘플코드, 파라미터 바인딩)
author: cotchan
date: 2021-02-04 16:00:00 +0800
categories: [Sqlite, Sqlite_Obj-c]
tags: [sqlite]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. 샘플 코드

---

## 1-2. 반환 결과가 없는 SQL 쿼리 실행

+ **테이블을 대상으로 하는 `CREATE`, `DROP`**
+ **레코드를 대상으로하는 `INSERT`, `UPDATE`, `DELETE`**
+ 위 명령들은 별도로 반환값이 필요하지 않습니다.
+ SQL을 실행하는 방법은 `sqlite3_exec` 함수를 사용합니다.

![Desktop View](/assets/img/post/sqlite/2021-02-04-sqlite-obj-c-query-sample-01.png)

```ruby
# .sqlite filePath: 
# /Users/${USER_NAME}/Library/Developer/Xcode/DerivedData/${PROJECT_NAME}-bnkkfhpekokionecxsfblwmzdiii/Build/Products/Debug

NSString *databasePath = @"./test001.sqlite";  # 새로 생성할 데이터베이스 파일 또는 기존에 존재하는 데이터베이스 파일
sqlite3 *database = nil;  # 데이터베이스 연결정보        
sqlite3_stmt *databaseStatement = nil;    # 쿼리 구문 컴파일러        
  
if (sqlite3_open([databasePath UTF8String], &database) == SQLITE_OK)
{
    NSLog(@"SUCCESS: sqlite3_open");

    # VER1
    # 반환 결과가 없는 QUERY
    # 1. CREATE
    databaseQuery = @"CREATE TABLE IF NOT EXISTS 'TABLE1'('id' INTEGER PRIMARY KEY, 'value' TEXT);";
            
    if (sqlite3_exec(database, [databaseQuery UTF8String], NULL, NULL, &databaseMessage) == SQLITE_OK)
    {
        NSLog(@"SUCCESS: sqlite3_exec: %@", databaseQuery);
    }
    else
    {
        NSLog(@"FAILURE: sqlite3_exec: %s", databaseMessage);
    }
         
    //2. INSERT   
    databaseQuery = @"INSERT INTO 'TABLE1'('value') VALUES('foo')";
    if (sqlite3_exec(database, [databaseQuery UTF8String], NULL, NULL, &databaseMessage) == SQLITE_OK)
    {
        NSLog(@"SULCCESS: sqlite3_exec: %@", databaseQuery);
    }
    else
    {
        NSLog(@"FAILURE: sqlite3_exec: %s", databaseMessage);
    }

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


## 1-3. 반환 결과가 있는 SQL 쿼리 실행

+ **`SELECT` 구문은 레코드 집합을 결과로써 반환합니다.**
+ 이를 실행하여 반환값을 얻기 위해서는 `sqlite3_prepare_v2` 함수를 사용합니다.
+ SQL 쿼리가 UTF-16으로 인코딩 되어 있다면 `sqlite3_prepare16_v2` 함수를 사용합니다.
+ **이 함수의 실행 결과는 `sqlite3_stmt` 형 객체가 얻어집니다.**
  + 이 객체를 통해 구문분석 및 컴파일된 SQL 쿼리가 보관되므로 잘 보관해둡니다.

![Desktop View](/assets/img/post/sqlite/2021-02-04-sqlite-obj-c-query-sample-02.png)

```ruby
NSString *databasePath = @"./test001.sqlite";  # 새로 생성할 데이터베이스 파일 또는 기존에 존재하는 데이터베이스 파일
sqlite3 *database = nil;  # 데이터베이스 연결정보
sqlite3_stmt *databaseStatement = nil;    # 쿼리 구문 컴파일러
NSString *databaseQuery = nil;  # SQL 구문
char *databaseMessage = nil;    # 오류 메시지를 전달받을 포인터 변수
        
if (sqlite3_open([databasePath UTF8String], &database) == SQLITE_OK)
{
    NSLog(@"SUCCESS: sqlite3_open");
            
    # VER2
    # 반환 결과가 있는 QUERY
    databaseQuery = @"SELECT * FROM 'TABLE1'";
    if (sqlite3_prepare_v2(database, [databaseQuery UTF8String], -1, &databaseStatement, nil) == SQLITE_OK)
    {
        int query_column = 0;
        int field_index = 0;
        const char *field_value = nil;
                
        query_column = sqlite3_column_count(databaseStatement);
        NSLog(@"SUCCESS: sqlite3_prepare_v2: %@, query_column = %d", databaseQuery, query_column);
                
        while (sqlite3_step(databaseStatement) == SQLITE_ROW)
        {
            field_index = (int)sqlite3_column_int(databaseStatement, 0);
            field_value = (const char *)sqlite3_column_text(databaseStatement, 1);
            NSLog(@"SUCCESS: sqlite3_step: %d, %s", field_index, field_value);
        }
                
            sqlite3_finalize(databaseStatement);
        }
        else
        {
            NSLog(@"FAILURE: sqlite3_prepare_v2");
        }
            
        databaseQuery = @"SELECT * FROM 'TABLE1' WHERE (id = ? AND value like ?);";
            
        if (sqlite3_prepare_v2(database, [databaseQuery UTF8String], -1, &databaseStatement, nil) == SQLITE_OK)
        {
            int query_column = 0;
            int field_index = 0;
            const char *field_value = nil;
                                
            sqlite3_bind_int(databaseStatement, 1, 1);
            sqlite3_bind_text(databaseStatement, 2, "foo", -1, nil);
                
            query_column = sqlite3_column_count(databaseStatement);
            NSLog(@"SUCCESS: sqlite3_prepare_v2: %@, query_column = %d", databaseQuery, query_column);
            while (sqlite3_step(databaseStatement) == SQLITE_ROW)
            {
                field_index = (int) sqlite3_column_int(databaseStatement, 0);
                field_value = (const char*) sqlite3_column_text(databaseStatement, 1);
                NSLog(@"SUCCESS: sqlite3_step: %d, %s", field_index, field_value);
            }
                
            sqlite3_finalize(databaseStatement);
        }
        else
        {
            NSLog(@"FAILURE: sqlite3_prepare_v2");
        }
             
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

## 1-4. 파라미터 바인딩

+ `UPDATE SET ...` 구문이나 `SELECT ... WHERE` 구문과 같이 쿼리에 사용자 문자열이 들어가는 경우 파라미터 바인딩을 사용합니다.

```ruby

NSString *databasePath = @"./test001.sqlite";  # 새로 생성할 데이터베이스 파일 또는 기존에 존재하는 데이터베이스 파일
sqlite3 *database = nil;  # 데이터베이스 연결정보
sqlite3_stmt *databaseStatement = nil;    # 쿼리 구문 컴파일러
NSString *databaseQuery = nil;  # SQL 구문
char *databaseMessage = nil;    # 오류 메시지를 전달받을 포인터 변수
        
if (sqlite3_open([databasePath UTF8String], &database) == SQLITE_OK)
{
    NSLog(@"SUCCESS: sqlite3_open");
            
    # VER3. 파라미터 바인딩
    databaseQuery = @"SELECT * FROM 'TABLE1' WHERE (id = ? AND value like ?);";
            
    if (sqlite3_prepare_v2(database, [databaseQuery UTF8String], -1, &databaseStatement, nil) == SQLITE_OK)
    {
        int query_column = 0;
        int field_index = 0;
        const char *field_value = nil;
                                
        sqlite3_bind_int(databaseStatement, 1, 1);
        sqlite3_bind_text(databaseStatement, 2, "foo", -1, nil);
                
        query_column = sqlite3_column_count(databaseStatement);
        NSLog(@"SUCCESS: sqlite3_prepare_v2: %@, query_column = %d", databaseQuery, query_column);
        while (sqlite3_step(databaseStatement) == SQLITE_ROW)
        {
            field_index = (int) sqlite3_column_int(databaseStatement, 0);
            field_value = (const char*) sqlite3_column_text(databaseStatement, 1);
            NSLog(@"SUCCESS: sqlite3_step: %d, %s", field_index, field_value);
        }
                
        sqlite3_finalize(databaseStatement);
    }
    else
    {
        NSLog(@"FAILURE: sqlite3_prepare_v2");
    }
             
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
