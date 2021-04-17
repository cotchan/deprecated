---
title: sb3) Date to LocalDateTime 및 milli second 없애기
author: cotchan 
date: 2021-04-18 04:14:21 +0800 
categories: []
tags: [spring-boot3] 
---

+ **이 포스팅은 개인 공부 목적으로 작성한 포스팅입니다**
+ 아래 포스팅을 참고하여 작성하였습니다.

---

## 1. Date 타입을 LocalDateTime으로 파싱

```java
private DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyyMMddHHmmss"); // DB에 문자열로 들어간 날짜 데이터 변환에 사용

RowMapper<Product>  productMapper = (rs, rowNum) -> {
	Product product = new Product();
	product.setId(rs.getInt("id"));
	product.setName(rs.getString("name"));
	product.setPrice(rs.getLong("price"));
	product.setDescription(rs.getString("desc"));
			
	LocalDateTime regTime = LocalDateTime.parse(rs.getString("reg_time"), formatter);
	product.setRegisteredTime(regTime);
	
	return product;
};
```

---

+ 이 방법을 사용하면 milli second를 잡아낼 수 있습니다.
+ **단, milli second가 .00 이렇게 두 자리 수로 들어오면 `DateTimeParseException 예외`가 터집니다.**

```java
// Sample variable name - you'd probably name this better.
public static DateTimeFormat LONG_FORMATTER = DateTimeFormatter.forPattern("yyyy MM dd HH:mm:ss.SSS");

// Note that this could easily take a DateTime directly if that's what you've got.
// Hint hint non-null valid date hint hint.
public static String formatAndChopEmptyMilliseconds(final Date nonNullValidDate) {
    final String formattedString = LONG_FORMATTER.print(new DateTime(nonNullValidDate));
    return formattedString.endsWith(".000") ? formattedString.substring(0, formattedString.length() - 4) : formattedString;
}
```

---

+ Example Code

```java
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
```

---

## 2. withNano(0)

+ **Test Code**

```java
LocalDateTime.now();
LocalDateTime.now().withNano(0);
```

```java
import java.time.LocalDateTime;

public class DateTimeSample {

  public static void main(String[] args) {
    LocalDateTime ldt = LocalDateTime.now();
    System.out.println(ldt);
    System.out.println(ldt.withNano(0));
  }
}
```

---

+ **결과**

```
2015-07-30T16:29:11.684
2015-07-30T16:29:11
```




---

+ 출처
  + [spring-jdbc-examples](https://github.com/leeseung/spring-jdbc-examples)
  + [Joda-Time DateFormatter to display milliseconds if nonzero
](https://stackoverflow.com/questions/2806848/joda-time-dateformatter-to-display-milliseconds-if-nonzero)
