---
title: JDBC) Row Mapper
author: cotchan
date: 2021-04-29 08:40:00 +0800
categories: [Java]
tags: [java]   
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 바탕으로 작성하였습니다.**

---

## 1. queryForObject의 반환형

+ **`queryForObject의 반환형은 데이터형만 가능`합니다.**
+ 하지만 그러면 `"SELECT * FROM USER"` 구문으로 `User 객체 자체를 반환 받는것`은 포기해야 하는걸까요?
+ **그걸 위해서 필요한 것이 바로 RowMapper 인터페이스입니다.**

---

## 2. RowMapper란?

+ **템플릿으로부터 `ResultSet을 전달받고`, `필요한 정보를 추출해서 리턴하는 방식으로 동작`합니다.**
+ ResultSet의 로우 하나만을 매핑하기 위해 사용됩니다.

+ **RowMapper를 사용하면, `원하는 형태의 결과값을 반환`할 수 있습니다.**

+ **`SELECT로 나온 여러개의 값을 반환`할 수 있을 뿐만 아니라, `사용자가 원하는 형태로도 얼마든지 받을 수 있습니다.`**
+ 즉, 다음과 같이 가능하다는 것입니다.

```java
// UserMapper로 인해, User 형태로 반환 가능
List<User> userList = jdbcTemplate.queryForObject(
        "SELECT * FROM USER WHERE id=?",
        UserMapper,
        1000L);
```

+ 이게 어떻게 가능한 것인지 살펴보겠습니다.

---

## 3. 과거의 순수 JDBC

+ 우리가 순수 JDBC를 배울땐 이런 고민을 하지 않았습니다.
+ 그 이유는 **우린 `결과값을 ResultSet으로 반환` 받았기 때문입니다.**

```java
// 쿼리 날리기
ResultSet rs = stat.excuteQuery("SELECT * FROM USER");

// 결과값 가져오기
while(rs.next()) {
     // user 객체에 값 저장
     user = new User();
     user.setId(rs.getInt(1));
     user.setName(rs.getString(2));
     user.setDescription(rs.getString(3));
     
     // 리스트에 추가
     userList.add(user);
}
```

---

+ 아무런 생각없이 사용했지만, 사실 아래 세 가지 단계를 밟은 것입니다.

1. **ResultSet으로 먼저 값을 받고,**
2. **그 다음 User 객체에 담아서**
3. **반환**


---

## 4. RowMapper 사용법

+ RowMapper의 mapRow 메소드는 이러한 ResultSet을 사용합니다.
+ 사용법은 아래와 같습니다.

```java
??? mapRow(ResultSet rs, int count);
```

+ **`ResultSet에 값을 담아와서 User 객체에 저장`합니다.**
+ **그리고 `그것을 count만큼 반복한다는 뜻`입니다.**

+ 이것을 실제로 작성해보면 아래와 같습니다.
  + 또한, count는 알아서 세어주므로, 작성할 필요가 없습니다.

```java
public User mapRow(ResultSet rs, int count) throws SQLException {
    // ResultSet 값을 User 객체에 저장
    User user = new User();
    user.setFirstName(rs.getString("name"));
    user.setLastName(rs.getString("description"));
    
    // user 반환
    return user;
};
```

---

+ 위 함수를 람다식으로 작성하면 아래와 같습니다.

```java
(rs, count) -> new User(
                     rs.getString("name"),
                     rs.getString("description")
                   )
```

---

+ **또한 RowMapper는 반복되는 코드이므로 `별도의 static 메서드로 분리하여 사용`하는 게 바람직합니다.**


```java
//UserRepositoryImpl.java
@Repository
public class UserRepositoryImpl implements UserRepository {

    private final JdbcTemplate jdbcTemplate;

    //...
    @Override
    public List<User> findAll() throws DataAccessException {
        return jdbcTemplate.query(
                "select * from Users",
                userMapper
        );
    }

    static RowMapper<User> userMapper = (rs, count) -> new User.Builder()
        .seq(rs.getLong("seq"))
        .email(new Email(rs.getString("email")))
        .password(rs.getString("passwd"))
        .loginCount(rs.getInt("login_count"))
        .lastLoginAt(dateTimeOf(rs.getTimestamp("last_login_at")))
        .createAt(dateTimeOf(rs.getTimestamp("create_at")))
        .build();

}
```

---

## 5. 향상된 queryForObject 

+ 이제는 User 클래스도 반환 받을 수 있습니다.

```java
// select 구문
String SELECT_BY_ID = "SELECT * FROM USER WHERE id=?";

// user 반환 받기
User user = jdbcTemplate.queryForObject(
        SELECT_BY_ID,
        (rs, count) -> new User(
                     rs.getString("name"),
                     rs.getString("description")
                   ),
        1000L);
```


---

+ 출처
  + [RowMapper에 대해!](https://velog.io/@seculoper235/RowMapper%EC%97%90-%EB%8C%80%ED%95%B4)
  + [[Spring] JdbcTemplate 사용법 - update(), queryForInt(), queryForObject(), query()](https://withseungryu.tistory.com/92#%F0%9F%92%A1%20update())
