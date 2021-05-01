---
title: sb3) JDBC Template 메서드와 사용 예시
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

## 2. update()

+ JdbcTemplate는 DAO객체에서 DB와 연동하기 위해 SQL 연산들을 수행 할 수 있도록 도와주는 기술입니다.
+ **`update()`는 SQL 연산을 통해 데이터베이스를 갱신시켜줄 때`(INSERT, DELETE, UPDATE)에서 사용하는 메소드`입니다.**

+ `JdbcTemplate.update()` insert return values
  + 결과값 0: 실패
  + **`결과값 1: 성공`**
  + 예외: `DataAccessException`

---

## 2-1. INSERT

+ **`치환자(== ?)를 가진 SQL`로 PreparedStatement를 만들고 함께 제공하는 파라미터를 순서대로 바인딩해주는 기능을 가진 update() 메소드를 사용하면 됩니다.**
+ SQL과 함께 가변인자로 선언된 파라미터를 제공해주면 됩니다.

```java
this.jdbcTemplate.update("insert into users(id, name, password) values(?,?,?)",
						user.getId(), user.getName(), user.getPassword());
```

---

## 2-2. UPDATE

+ INSERT의 방식과 마찬가지로 치환자를 가진 SQL을 만든 후 함께 제공하는 파라미터를 활용하면 됩니다.

```java
this.jdbcTemplate.update("update users set name=? where password=?",
					user.getName() , user.getPassword());
```

---

## 2-3. DELETE

+ 치환자가 필요하면 파라미터로 넘겨주면 되고, 치환자가 필요없을 때는 SQL문만 작성해주면 됩니다.

```java
this.jdbcTemplate.update("delete from users");
```

---

## 3. query()

+ queryForInt()가 하나의 결과 값을 위한 메소드인 반면, 많은 결과 값(로우 값)을 처리 할 수 있는 메소드입니다.
+ List 형식을 활용해 여러 개의 로우 값들을 저장할 수 있습니다.

```java
//JdbcTemplate.java

//jdbcTemplate.query(arg1, arg2);
@Override
public <T> List<T> query(String sql, RowMapper<T> rowMapper) throws DataAccessException {
	return result(query(sql, new RowMapperResultSetExtractor<>(rowMapper)));
}

//jdbcTemplte.query(arg1, arg2, arg3);
@Override
public <T> List<T> query(String sql, RowMapper<T> rowMapper, @Nullable Object... args) throws DataAccessException {
	return result(query(sql, args, new RowMapperResultSetExtractor<>(rowMapper)));
}
```

+ **첫번째 파라미터 : `실행할 SQL 쿼리`를 넣는다.**
+ **두번째 파라미터 : `RowMapper 콜백`**
+ **세번째 파라미터(선택사항): `치환자에 바인딩할 파라미터`**

+ RowMapper에서 만들어진 User Entity는 JdbcTemplate이 미리 준비한 List 컬렉션에 추가됩니다. 
+ 작업을 마치면 모든 로우에 대한 User Entity를 담고 있는 List 오브젝트가 리턴됩니다.

```java
//사용 예시
    
@Override
public Optional<User> findById(Long id) throws DataAccessException, DateTimeParseException {
    List<User> results = jdbcTemplate.query("select * from Users where seq = ?", userMapper, id);
    return Optional.ofNullable(results.isEmpty() ? null : results.get(0));
}

@Override
public List<User> findAll() throws DataAccessException, DateTimeParseException {
    return jdbcTemplate.query("select * from Users", userMapper);
}
```

---

## 4. queryForObject()

+ SQL의 DML 중 SELECT를 실행했을 때 하나의 객체(Object) 결과 값이 나올 때 사용하는 메소드입니다.
+ 이를 위해, getCount()에 적용했던 RowMapper 콜백을 사용해야 합니다.
  + [RowMapper란?](https://cotchan.github.io/posts/jdbc-row-mapper/)
+ **매핑되는 객체값이 없는 경우 `리플렉션 Exception이 발생`하므로 query() 메소드를 쓰는 게 낫습니다.**


```java
return this.jdbcTemplate.queryForObject(
				"select * from users where id =?", 
				new Object[] {id},
				new RowMapper<User>() {
					public User mapRow(ResultSet rs, int rowNum) throws SQLException {
						User user = new User();
						user.setId(rs.getString("id"));
						user.setName(rs.getString("name"));
						user.setPassword(rs.getString("password"));
						return user;
					}
			
		});
```

---

## 5. queryForInt()

+ SQL 쿼리를 실행하고 정수의 결과 값(Integer 타입)을 가져올 때 사용하는 메소드입니다.


```java
return this.jdbcTempalte.queryForInt("select count(*) from users");
```

---

## 6. JdbcRepository로 사용하는 예시

---

## User Repository

```java
public interface UserRepository {

    User insert(User user);

    Optional<User> findById(Long id);

    List<User> findAll();
}
```

---

## User Entity

```java
public class User implements Serializable {

    private final Long seq;

    private final Email email;

    private final String password;

    private int loginCount;

    private LocalDateTime lastLoginAt;

    private final LocalDateTime createAt;

    //...
}
```

---

## 6-1. RowMapper

+ RowMapper는 공통되는 메서드이므로 별도로 빼는 게 낫습니다.

```java
@Repository
public class UserRepositoryImpl implements UserRepository {

    private final JdbcTemplate jdbcTemplate;

    @Autowired
    protected UserRepositoryImpl(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
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

## 6-2. Insert 연산

+ **Insert 연산은 `KeyHolder`를 사용해서 `Entity에 AutoIncrement를 적용`합니다.**
  + **Entity 생성 순서**
    + Entity Builder로 Id값 없는 Entity 생성
    + update() 메소드를 통해 Entity를 DB에 넣고 KeyHolder사용해서 ID값 발급받음(pk)
    + 발급받은 ID값 사용해서 Entity 새로 생성(Id값 있는 Entity가 만들어짐)

```java
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
}
``` 

---

## 6-3. findById(Long id)

```java
@Repository
public class UserRepositoryImpl implements UserRepository {

    private final JdbcTemplate jdbcTemplate;

    @Autowired
    protected UserRepositoryImpl(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    //throws DataAccessException
    @Override
    public Optional<User> findById(Long id) throws DataAccessException, DateTimeParseException {
        List<User> results = jdbcTemplate.query("select * from Users where seq = ?", userMapper, id);
        return Optional.ofNullable(results.isEmpty() ? null : results.get(0));
    }
}
```


---

## 6-4. findAll()

```java
@Repository
public class UserRepositoryImpl implements UserRepository {

    private final JdbcTemplate jdbcTemplate;

    @Autowired
    protected UserRepositoryImpl(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    //throws DataAccessException
    @Override
    public List<User> findAll() throws DataAccessException, DateTimeParseException {
        return jdbcTemplate.query("select * from Users", userMapper);
    }
}
```

---

+ 출처
  + [Spring Boot JDBC Examples](https://mkyong.com/spring-boot/spring-boot-jdbc-examples/)
  + [spring-jdbc-examples](https://github.com/leeseung/spring-jdbc-examples)
  + [Obtaining Auto-generated Keys in Spring JDBC](https://www.baeldung.com/spring-jdbc-autogenerated-keys)
  + [JdbcTemplate.update() insert return values](https://stackoverflow.com/questions/21174705/jdbctemplate-update-insert-return-values)
  + [[Spring] JdbcTemplate 사용법 - update(), queryForInt(), queryForObject(), query()](https://withseungryu.tistory.com/92#%F0%9F%92%A1%20update())
