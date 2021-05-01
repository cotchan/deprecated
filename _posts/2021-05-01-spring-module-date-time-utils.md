---
title: sb3) DateTimeUtils (datetime <=> LocalDateTime)
author: cotchan 
date: 2021-05-01 22:20:21 +0800 
categories: [Spring-Boot3]
tags: [spring-module] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**

---

## 1. DateTimeUtils 클래스

```java
//DateTimeUtils.java

import java.sql.Timestamp;
import java.time.LocalDateTime;

public class DateTimeUtils {

  public static Timestamp timestampOf(LocalDateTime time) {
    return time == null ? null : Timestamp.valueOf(time);
  }

  public static LocalDateTime dateTimeOf(Timestamp timestamp) {
    return timestamp == null ? null : timestamp.toLocalDateTime();
  }

}
```

---

## 2. 사용 목적

+ **`DB의 datetime`과 `Java의 LocalDateTime`의 형변환을 위해 사용합니다.**
  + DB로 얻은 데이터를 Entity화 할 때
  + Entity를 DB에 밀어넣을 때 

---

## 3. users Table

```sql
CREATE TABLE users
(
    seq           bigint      NOT NULL AUTO_INCREMENT,
    email         varchar(50) NOT NULL,
    passwd        varchar(80) NOT NULL,
    last_login_at datetime             DEFAULT NULL,
    create_at     datetime    NOT NULL DEFAULT CURRENT_TIMESTAMP(),
    PRIMARY KEY (seq),
    CONSTRAINT unq_user_email UNIQUE (email)
);
```

---

## 4. User Entity

```java
public class User implements Serializable {

    private static final long serialVersionUID = 1L;

    public static final int MAX_PASSWORD_LENGTH = 80;

    private final Long seq;

    private final Email email;

    private final String password;

    private LocalDateTime lastLoginAt;

    private final LocalDateTime createAt;
```

---

## 5. 사용 예시

+ **실제 DateTimeUtils 클래스가 사용되는 곳은 `UserRepository` 입니다.**

```java
//UserRepositoryImpl.java

//...

import static com.github.prgrms.socialserver.util.DateTimeUtils.dateTimeOf;
import static com.github.prgrms.socialserver.util.DateTimeUtils.timestampOf;

@Repository
public class UserRepositoryImpl implements UserRepository {

    private final JdbcTemplate jdbcTemplate;

    @Autowired
    protected UserRepositoryImpl(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    @Override
    public User insert(User user) throws DataAccessException, IllegalArgumentException {

        KeyHolder keyHolder = new GeneratedKeyHolder();

        jdbcTemplate.update(new PreparedStatementCreator() {
            @Override
            public PreparedStatement createPreparedStatement(Connection conn) throws SQLException {

                PreparedStatement ps = conn.prepareStatement("INSERT INTO Users(seq, email, passwd, login_count, last_login_at, create_at) values(null,?,?,?,?,?)", new String[]{"seq"});
                ps.setString(1, user.getEmail().getEmailAddress());
                ps.setString(2, user.getPassword());
                ps.setInt(3, user.getLoginCount());
                ps.setTimestamp(4, timestampOf(user.getLastLoginAt().orElse(null)));
                ps.setTimestamp(5, timestampOf(user.getCreateAt()));
                return ps;
            }

        }, keyHolder);

        Number key = keyHolder.getKey();

        long generateSeq = key != null ? key.longValue() : -1;
        return new User.Builder(user)
                .seq(generateSeq)
                .build();
    }

    //...

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

+ 출처
    + [[스터디/9기] 단순 CRUD는 그만! 웹 백엔드 시스템 구현(Spring Boot)](https://programmers.co.kr/learn/courses/11694) 
