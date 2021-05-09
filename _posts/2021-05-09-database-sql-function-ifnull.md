---
title: db) 표준 SQL 조건식 IFNULL
author: cotchan 
date: 2021-05-09 21:00:21 +0800 
categories: [Database]
tags: [database]
---

+ 아래 출처의 내용들을 바탕으로 작성한 내용입니다.    
+ **개인 공부 목적으로 작성한 글입니다.**

---

## 1. INFULL

```sql
IFNULL(expr, null_result)
```

+ **`expr이 NULL이 아니면` expr을 반환합니다.**
  + **expr이 NULL이 아니면 null_result가 평가되지 않습니다.**
+ **`expr이 NULL이면` null_result를 반환합니다.**

+ **RETURN VALUE**
  + expr 또는 null_result의 상위 유형이 반환됩니다.

---

## 2. 예시

+ case1

```sql
SELECT IFNULL(NULL, 0) as result

+--------+
| result |
+--------+
| 0      |
+--------+
```

---

+ case2

```sql
SELECT IFNULL(10, 0) as result

+--------+
| result |
+--------+
| 10     |
+--------+
```

---

+ case3

```java
@Override
public Optional<Post> findById(Id<Post, Long> postId, Id<User, Long> writerId, Id<User, Long> userId) {
  List<Post> results = jdbcTemplate.query(
    "SELECT " +
      "p.*,u.email,u.name,ifnull(l.seq,false) as likesOfMe " +
      "FROM " +
        "posts p JOIN users u ON p.user_seq=u.seq LEFT OUTER JOIN likes l ON p.seq=l.post_seq AND l.user_seq=? " +
      "WHERE " +
        "p.seq=? AND p.user_seq=?",
    mapper,
    userId.value(), postId.value(), writerId.value()
  );
  return ofNullable(results.isEmpty() ? null : results.get(0));
}
```

---
+ 출처
    + [표준 SQL의 조건식](https://cloud.google.com/bigquery/docs/reference/standard-sql/conditional_expressions?hl=ko#ifnull)
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694)
