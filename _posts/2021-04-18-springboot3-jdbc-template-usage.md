---
title: sb3) JDBC Template 사용 예시
author: cotchan 
date: 2021-04-18 03:49:21 +0800 
categories: []
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ **아래 출처 글을 참고하여 작성하였습니다.**

---

## 1. pom.xml

+ **`JDBC API`, Spring Web, H2  import**

```xml
	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jdbc</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>

		<dependency>
			<groupId>com.h2database</groupId>
			<artifactId>h2</artifactId>
			<scope>runtime</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-test</artifactId>
			<scope>test</scope>
		</dependency>
	</dependencies>
```

---


## 2. UserRepository

```java
public interface UserRepository {

    int save(User user);

    Long getId(Email email);

    Optional<User> findById(Long id);

    List<User> findAll();

    int deleteAll();
}
```

---

```java
public class User implements Serializable {

    private final Email email;
    private final String password;
    private Long seq;
    private int loginCount;
    private LocalDateTime lastLoginAt;
    private final LocalDateTime createAt;

    //...
}
```

---

## 3. Jdbc Api Usage

---

## 3-1. save(User user)

```java
    @Override
    public int save(User user) throws DataAccessException, IllegalArgumentException {
       return jdbcTemplate.update(
               "insert into Users(email, passwd, create_at) values(?,?,?)",
                    user.getEmail().getEmailAddress(),user.getPassword(),user.getCreateAt());
    }
```

---

## 3-2. getId(Email email) (autoIncrement)

```java
    @Override
    public Long getId(Email email) throws DataAccessException {
        return jdbcTemplate.queryForObject(
                "select seq from Users where email = ?",
                new Object[]{email.getEmailAddress()},
                Long.class
        );
    }
```


---


## 3-3. findById(Long id)

```java
    @Override
    public Optional<User> findById(Long id) throws DataAccessException, DateTimeParseException {
        return jdbcTemplate.queryForObject(
                "select * from Users where seq = ?",
                new Object[]{id},
                (rs, rowNum) ->
                        Optional.of(new User(
                                rs.getLong("seq"),
                                new Email(rs.getString("email")),
                                rs.getString("passwd"),
                                rs.getInt("login_count"),
                                rs.getString("last_login_at") == null ? null : LocalDateTime.parse(rs.getString("last_login_at"), formatter),
                                LocalDateTime.parse(rs.getString("create_at"), formatter)
                        ))
        );
    }
```

---


## 3-4. findAll()

```java
    @Override
    public List<User> findAll() throws DataAccessException, DateTimeParseException {
        return jdbcTemplate.query(
                "select * from Users",
                (rs, rowNum) -> new User(
                        rs.getLong("seq"),
                        new Email(rs.getString("email")),
                        rs.getString("passwd"),
                        rs.getInt("login_count"),
                        rs.getString("last_login_at") == null ? null : LocalDateTime.parse(rs.getString("last_login_at"), formatter),
                        LocalDateTime.parse(rs.getString("create_at"), formatter)
                )
        );
    }
```

---

## 3-5. deleteAll()

```java
    @Override
    public int deleteAll() {
        return jdbcTemplate.update(
                "delete Users"
        );
    }
```

---

## 3-6. get AutoIncrement key value

```java
String INSERT_MESSAGE_SQL 
  = "insert into sys_message (message) values(?) ";
    
public long insertMessage(String message) {    
    KeyHolder keyHolder = new GeneratedKeyHolder();

    jdbcTemplate.update(connection -> {
        PreparedStatement ps = connection
          .prepareStatement(INSERT_MESSAGE_SQL);
          ps.setString(1, message);
          return ps;
        }, keyHolder);

        return (long) keyHolder.getKey();
    }
}
```

---

## 4. UserRepositoryImpl

```java
package com.github.prgrms.socialserver.domain;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.dao.DataAccessException;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.stereotype.Repository;

import java.time.LocalDateTime;
import java.time.format.DateTimeFormatter;
import java.time.format.DateTimeParseException;
import java.util.List;
import java.util.Optional;

@Repository
public class UserRepositoryImpl implements UserRepository {

    private final JdbcTemplate jdbcTemplate;
    private DateTimeFormatter formatter;

    @Autowired
    protected UserRepositoryImpl(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
//        this.formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
        this.formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss.SSS");
    }

    //throws DataAccessException
    @Override
    public int save(User user) throws DataAccessException, IllegalArgumentException {
       return jdbcTemplate.update(
               "insert into Users(email, passwd, create_at) values(?,?,?)",
                    user.getEmail().getEmailAddress(),user.getPassword(),user.getCreateAt());
    }

    //throws DataAccessException
    @Override
    public Long getId(Email email) throws DataAccessException {
        return jdbcTemplate.queryForObject(
                "select seq from Users where email = ?",
                new Object[]{email.getEmailAddress()},
                Long.class
        );
    }

    //throws DataAccessException
    @Override
    public Optional<User> findById(Long id) throws DataAccessException, DateTimeParseException {
        return jdbcTemplate.queryForObject(
                "select * from Users where seq = ?",
                new Object[]{id},
                (rs, rowNum) ->
                        Optional.of(new User(
                                rs.getLong("seq"),
                                new Email(rs.getString("email")),
                                rs.getString("passwd"),
                                rs.getInt("login_count"),
                                rs.getString("last_login_at") == null ? null : LocalDateTime.parse(rs.getString("last_login_at"), formatter),
                                LocalDateTime.parse(rs.getString("create_at"), formatter)
                        ))
        );
    }

    //throws DataAccessException
    @Override
    public List<User> findAll() throws DataAccessException, DateTimeParseException {
        return jdbcTemplate.query(
                "select * from Users",
                (rs, rowNum) -> new User(
                        rs.getLong("seq"),
                        new Email(rs.getString("email")),
                        rs.getString("passwd"),
                        rs.getInt("login_count"),
                        rs.getString("last_login_at") == null ? null : LocalDateTime.parse(rs.getString("last_login_at"), formatter),
                        LocalDateTime.parse(rs.getString("create_at"), formatter)
                )
        );
    }

    @Override
    public int deleteAll() {
        return jdbcTemplate.update(
                "delete Users"
        );
    }
}
```

---

+ 출처
  + [Spring Boot JDBC Examples](https://mkyong.com/spring-boot/spring-boot-jdbc-examples/)
  + [spring-jdbc-examples](https://github.com/leeseung/spring-jdbc-examples)
  + [Obtaining Auto-generated Keys in Spring JDBC](https://www.baeldung.com/spring-jdbc-autogenerated-keys)
